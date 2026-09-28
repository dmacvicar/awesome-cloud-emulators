# Reusable Moto baselines and process snapshots

[Back to the selection guide](selection-guide.md#reusable-emulator-baselines)

Use this workflow to evaluate a prepared Moto server as a reusable starting point for tests or research. It is a proposed validation procedure, not a tested integration recipe. The upstream features below were reviewed on **2026-09-28**.

## Choose what to preserve

**Replay** rebuilds resources from requests. [Moto Recorder](https://docs.getmoto.org/en/stable/docs/configuration/recorder/index.html) is built in, supports ServerMode, and is enabled with `MOTO_ENABLE_RECORDING=True`. Its APIs start/stop recording, download/upload the request log, and replay it. Generated IDs are not saved automatically; Moto documents seeding for repeatable identifiers. Validate dependent requests against the pinned version. Resetting the recording clears the log, not the resource backends; [server reset](https://docs.getmoto.org/en/stable/docs/server_mode.html) clears server state.

**Process checkpoints** capture execution state. [CRIU](https://github.com/checkpoint-restore/criu) checkpoints Linux tasks; [Podman](https://podman.io/docs/checkpoint) integrates it for containers. [DMTCP](https://github.com/dmtcp/dmtcp) is a separate application-level approach using a launcher and coordinator. Its general Python support makes it worth evaluating, but is not proof that a particular Moto configuration restores correctly.

**Container image commits** capture filesystem changes, not live process memory. [Docker commit](https://docs.docker.com/reference/cli/docker/container/commit) also excludes mounted-volume data. Installing and seeding Moto, then committing the container image, is not a snapshot of its running backend state. Docker's separate [checkpoint/restore feature](https://docs.docker.com/reference/cli/docker/checkpoint) uses CRIU and remains experimental.

## Prepare and capture

1. **Define a baseline contract.** List the supported service operations, accounts, regions, resources, and synthetic data required. Treat a “landing zone” as a modeled subset; real network controls and organization policy enforcement need separate validation.
2. **Make setup reproducible.** Keep SDK seed code or IaC in version control, pin client/provider versions, and explicitly route calls to Moto. Use dummy credentials and assert the endpoint before applying resources. A Terraform/OpenTofu state file is resource bookkeeping, not the emulator's live state; version or isolate it consistently with the baseline.
3. **Prepare a clean instance.** Apply the baseline and assert observable resources and representative reads. If recording requests, begin before setup and stop after it; retain the log as a fixture artifact.
4. **Quiesce before a process checkpoint.** Finish requests and stop test clients. Prefer reconnecting after restore to trying to preserve live client sessions. Decide what to do with pending work and time-sensitive fixtures before capture.
5. **Capture one consistent artifact set.** With Podman, review [checkpoint export](https://docs.podman.io/en/latest/markdown/podman-container-checkpoint.1.html) and [restore import](https://docs.podman.io/en/latest/markdown/podman-container-restore.1.html). Include required filesystem/volume state and separately account for bind mounts and external dependencies. Treat checkpoint artifacts as sensitive because they can contain process memory and test data.
6. **Record the environment.** Store the Moto version/image digest, baseline revision, architecture, Linux kernel, runtime and checkpoint-tool versions, network assumptions, and checksums. A checkpoint is not a universal image for arbitrary hosts.

## Restore and prove isolation

For each candidate, run this acceptance sequence on the actual CI runner class:

1. Start a fresh instance and replay the baseline, or restore a fresh instance from the checkpoint. Allocate isolated ports, identities, and any writable state per test worker.
2. Wait for readiness, reconnect clients, and compare resource inventory plus representative payloads against the baseline contract.
3. Mutate and delete fixtures, run a test, and dispose of that instance and its mutable state.
4. Restore the original baseline again. Assert that prior mutations are absent. Repeat for two workers to expose shared volumes or port collisions.
5. Test the time-dependent behavior your suite relies on, such as expiry and delayed processing; a process snapshot is not a guarantee of identical wall-clock behavior.
6. Measure complete rebuild/replay time versus restore-and-readiness time, artifact size, and failure rate. Rebuild snapshots when the baseline or execution environment changes.

A successful restore must satisfy application assertions; an HTTP health response is insufficient. Keep real-cloud tests for semantics Moto does not model.

## Platform and consistency boundaries

CRIU is Linux-specific. Verify kernel features, required privileges, container runtime support, and security settings on the host that actually executes the workload. Do not assume rootless runners or Docker Desktop on macOS/Windows provide the same checkpoint capability as a compatible Linux host.

Use the selected tool's documented handling of [TCP connections](https://criu.org/Advanced_usage), file locks, storage, and namespaces. Resume external dependencies consistently or reconnect/recreate them; restoring Moto alone does not restore the client application or another database. Podman exposes options for root-filesystem and volume inclusion, so inspect the archive contents rather than assuming all data is present.

DMTCP candidates should be launched under its control from the start; see its [quick start](https://github.com/dmtcp/dmtcp/blob/main/QUICK-START.md). Container checkpointing and application checkpointing have different integration requirements. None of these tools extends the set of AWS operations or enforcement semantics implemented by Moto.
