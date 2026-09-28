# Reusable cloud emulator baselines and snapshots

[Back to the selection guide](selection-guide.md#reusable-emulator-baselines)

Use this workflow to prepare a repeatable starting state for tests or research with a cloud emulator. It applies to single-service emulators and broader cloud API suites. Choose a capture method based on where the emulator actually stores state; the tools below are candidates to evaluate, not tested integrations with every emulator. The linked upstream features were reviewed on **2026-09-28**.

## Choose what to preserve

| Method | What it preserves | When to consider it | Boundary |
| --- | --- | --- | --- |
| Declarative setup or seed code | Instructions and fixture data | The default for portable, reviewable baselines | Must reapply; unsupported API operations may require another setup path. |
| Request recording and replay | Calls used to build a baseline | The emulator or client supports repeatable recording | Replays calls, not process state; generated IDs and ordering may need explicit handling. |
| Persisted-state copy | Files or volumes used by the emulator | State is documented as durable and can be captured consistently | Does not capture in-memory state or external dependencies. |
| Process or container checkpoint | Running process state, subject to tool support | Rebuilding is costly and the runner supports checkpoint/restore | Host, runtime, filesystem, network, and dependency compatibility require validation. |

Examples of checkpoint candidates include [CRIU](https://github.com/checkpoint-restore/criu) for Linux processes and [Podman checkpoint/restore](https://podman.io/docs/checkpoint) for containers. [DMTCP](https://github.com/dmtcp/dmtcp) is a separate application-level approach; it requires launching the workload under its control. Docker's [checkpoint/restore](https://docs.docker.com/reference/cli/docker/checkpoint) uses CRIU and is experimental. These are mechanisms to trial, not assurances that a particular emulator restores correctly.

[Docker commit](https://docs.docker.com/reference/cli/docker/container/commit) captures container filesystem changes, not process memory or mounted-volume data. Use an image commit for a prepared runtime or files, not as a live-state snapshot. For a disk-backed emulator, confirm which paths hold state and how to stop or quiesce writes before copying them.

## Prepare and capture

1. **Define a baseline contract.** List the emulator version, supported API operations, accounts or projects, regions, resources, and synthetic data required. A modeled landing zone is a subset of cloud behavior; validate real-cloud network and policy enforcement separately.
2. **Make setup reproducible.** Keep SDK seed code or IaC in version control, pin client and provider versions, route calls explicitly to local endpoints, use dummy credentials, and assert the destination before applying resources. Keep IaC state files consistent with the baseline; they track managed resources but do not necessarily contain the emulator's live state.
3. **Prepare a clean instance.** Apply setup, then assert resource inventory and representative reads. If recording requests, start before setup, stop after it, and retain the log as a fixture artifact. Check how IDs, timestamps, and non-idempotent operations behave during replay.
4. **Quiesce for capture.** Finish requests, stop test clients, and make writes consistent. For process checkpoints, reconnect clients after restore instead of assuming existing sessions survive. Decide how pending work and time-sensitive fixtures should behave.
5. **Capture a complete artifact set.** Record what the method includes and excludes: process memory, root filesystem, volumes, bind mounts, request logs, and external services. Podman documents [checkpoint export](https://docs.podman.io/en/latest/markdown/podman-container-checkpoint.1.html) and [restore import](https://docs.podman.io/en/latest/markdown/podman-container-restore.1.html), with options affecting filesystem and volume inclusion. Inspect the artifact rather than assuming it contains all state.
6. **Record the environment.** Store emulator image digest or version, baseline revision, architecture, OS/kernel, runtime and checkpoint-tool versions where applicable, network assumptions, and checksums. Protect artifacts that may contain process memory, credentials, or test data.

## Restore and prove isolation

Run this acceptance sequence on the actual CI runner class for each candidate method:

1. Start a fresh emulator and rebuild or replay the baseline, or restore a fresh instance from its captured state. Allocate isolated ports, identities, and writable storage per test worker.
2. Wait for readiness, reconnect clients, and compare resource inventory and representative payloads with the baseline contract.
3. Mutate and delete fixtures, run a test, and dispose of the instance and its mutable state.
4. Restore the original baseline again and assert that the prior mutations are absent. Repeat with two workers to find shared-volume or port collisions.
5. Test time-dependent behavior your suite relies on, such as expiry, scheduled events, or delayed processing. A process snapshot does not guarantee identical wall-clock behavior.
6. Compare complete rebuild/replay time with restore-and-readiness time, artifact size, and failure rate. Rebuild artifacts when the baseline or execution environment changes.

Application assertions determine whether the restore worked; a health response alone is insufficient. Keep real-cloud tests for semantics the emulator does not model.

## Platform and consistency boundaries

CRIU is Linux-specific. Verify kernel features, privileges, security settings, and container runtime support on the host running the emulator. Do not assume a rootless runner or Docker Desktop on macOS/Windows offers the same checkpoint capability as a compatible Linux host. Review the tool's handling of [TCP connections](https://criu.org/Advanced_usage), file locks, storage, and namespaces. Recreate or restore external dependencies consistently.

For DMTCP, launch the candidate process under its control from the start; see its [quick start](https://github.com/dmtcp/dmtcp/blob/main/QUICK-START.md). Application and container checkpointing have different integration requirements. None of these capture methods adds cloud APIs or enforcement behavior missing from an emulator.

## Example: Moto server request replay

[Moto Recorder](https://docs.getmoto.org/en/stable/docs/configuration/recorder/index.html) is built in, supports ServerMode, and is enabled with `MOTO_ENABLE_RECORDING=True`. Its APIs start and stop recording, download or upload the request log, and replay it. Moto documents seeding for repeatable generated IDs. Validate dependent requests against the pinned Moto version. Resetting a recording clears its log, whereas [server reset](https://docs.getmoto.org/en/stable/docs/server_mode.html) clears the server's resource state.

For a Moto baseline, seed supported resources through the server's local endpoint, save the request log, start a fresh server, replay, and run the restore acceptance sequence above. This illustrates request replay; it does not establish checkpoint/restore compatibility for Moto or any other emulator.
