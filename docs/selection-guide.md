# Selecting a cloud emulator

[Back to the list](../README.md)

Choose against a representative test, not a service count, implementation language, or a claim of full compatibility. This guide combines the catalog's service grouping with practical runtime, ownership, and validation questions.

## Contents

- [Start with your test scenario](#start-with-your-test-scenario)
- [Comparison by cloud](#comparison-by-cloud)
- [Important distinctions](#important-distinctions)
- [Evaluation checklist](#evaluation-checklist)
- [Reusable emulator baselines](#reusable-emulator-baselines)
- [Adoption sequence](#adoption-sequence)
- [Evidence and reconciliation notes](#evidence-and-reconciliation-notes)

## Start with your test scenario

These are candidates to evaluate, not a benchmark ranking or a claim of interchangeable behavior.

| Scenario | Candidates | First validation |
| --- | --- | --- |
| Isolate Python logic using AWS SDK calls | Moto | Supported calls, mock lifecycle, and state isolation. |
| Exercise an AWS event-driven application | fakecloud, Floci, LocalEmu, LocalStack, MiniStack | A complete publish → consume → retry path, not just resource creation. |
| Test one AWS dependency | DynamoDB Local, ElasticMQ, S3Mock | Exact database, queue, or object operations required by the application. |
| Test an Azure application dependency | Azurite, Cosmos DB, Event Hubs, Service Bus, or Key Vault emulators | SDK connectivity, TLS, and required data-plane behavior. |
| Test several Azure APIs together | Floci AZ | Cross-service behavior and which functions or engines require Docker. |
| Test a Firebase application | Firebase Local Emulator Suite | Auth, Security Rules, database events, and function triggers used by the app. |
| Test Google Cloud data and messaging clients | Provider emulators; Fake GCS Server, Fullstory, BigQuery, or Cloud Tasks emulators | Client endpoints, SQL/queries, messages, tasks, and failure semantics. |
| Test several Google Cloud APIs together | Floci GCP | REST/gRPC coverage and integrated behavior for your specific flow. |
| Test infrastructure automation across clouds | cloudemu; Vera for EC2/Compute scope | Full create/read/update/delete lifecycle with the real CLI or IaC provider. |
| Run emulators in automated Java tests | Testcontainers Google Cloud Module | Image readiness, fixture isolation, cleanup, and parallel execution. |
| Invoke and debug Lambda locally | AWS SAM CLI | Runtime and event handling; configure external service dependencies separately. |
| Rebuild a baseline from recorded requests | Emulator recording or a client-side request log, such as Moto Recorder | Replay into a clean instance and verify generated identifiers and dependencies. |
| Resume a prepared emulator process | CRIU, Podman Checkpoint/Restore, Docker Checkpoint/Restore, DMTCP | Restore on the target runner and verify memory, external state, and clean test isolation. |

## Comparison by cloud

Every README entry appears below once. Tool links lead to primary documentation. **AWS, Microsoft, and Google** in the maintainer column mean cloud-provider supplied; **community** includes independent individuals and company-sponsored open-source projects; **vendor** identifies a commercial platform. These labels do not determine quality or licensing.

Delivery describes how you consume the tool, rather than guessing the implementation language of closed-source runtimes. Runtime versions and CPU architecture support should be checked for the exact release you intend to use.

### AWS

| Tool | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- |
| [fakecloud](https://fakecloud.dev) | AWS APIs | Binary or Docker; test SDKs | Community | Exercise async control and assertions; verify the operations you need. |
| [Floci](https://floci.io/floci) | AWS APIs | Docker / Compose | Community | Check service-to-service flows and persistence for your workload. |
| [LocalEmu](https://localemu.cloud) | AWS APIs | Python CLI; Docker for selected engines | Community | Separate in-process API behavior from engine-backed requirements. |
| [LocalStack](https://docs.localstack.cloud/aws) | AWS APIs | Container, managed through CLI or Docker | Vendor | Plan coverage, authentication, CI entitlements, and network requirements. |
| [MiniStack](https://ministack.org) | AWS APIs | Docker; selected external engines | Community | Check account/region isolation and which data planes actually execute. |
| [Moto](https://github.com/getmoto/moto) | AWS API mocks | Python library, server, or Docker | Community | Choose library or server mode; validate supported operations and behaviors. |
| [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) | DynamoDB | Java archive, Maven dependency, or Docker | AWS | Compare transactions, indexes, and cloud-only behavior. |
| [ElasticMQ](https://github.com/softwaremill/elasticmq) | SQS-compatible queues | JVM server, embedded library, or Docker | Community / SoftwareMill | Verify FIFO, visibility timeouts, redelivery, and dead-letter behavior. |
| [S3Mock](https://github.com/adobe/S3Mock) | S3 API subset | Docker / Testcontainers; JVM integrations | Community / Adobe | Check supported object operations and your chosen integration version. |

### Microsoft Azure

| Tool | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- |
| [Floci AZ](https://github.com/floci-io/floci-az) | Azure APIs | Docker / Compose; engine-backed services | Community | Verify SDK routing, TLS, auth mode, and Docker requirements per service. |
| [Azure Cosmos DB Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/emulator) | Cosmos DB | Windows or container variants | Microsoft | Choose the correct API, OS, and architecture variant; check TLS setup. |
| [Azure Event Hubs Emulator](https://learn.microsoft.com/en-us/azure/event-hubs/overview-emulator) | Event Hubs | Container with Azurite dependency | Microsoft | Check protocols, partition behavior, limits, and restart persistence. |
| [Azure Key Vault Emulator](https://github.com/james-gould/azure-keyvault-emulator) | Key Vault APIs | Docker; .NET Aspire integration | Community | Check secrets/keys/certificates coverage, TLS trust, and persistence. |
| [Azure Service Bus Emulator](https://learn.microsoft.com/en-us/azure/service-bus-messaging/overview-emulator) | Service Bus | Container with database dependency | Microsoft | Check messaging features, dependencies, limits, and state reset. |
| [Azurite](https://github.com/Azure/Azurite) | Blob, Queue, Table Storage | npm CLI, Docker, or VS Code extension | Microsoft | Verify required API versions and storage-specific behavior. |

### Google Cloud and Firebase

| Tool | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- |
| [Firebase Local Emulator Suite](https://firebase.google.com/docs/emulator-suite) | Firebase products and overlapping GCP APIs | Firebase CLI with service-specific runtimes | Google / Firebase | Check enabled products, Security Rules, triggers, and fixture import/export. |
| [Floci GCP](https://floci.io/floci-gcp) | Google Cloud APIs | Docker / Compose; selected engine containers | Community | Check REST/gRPC coverage and service-specific credential behavior. |
| [Fullstory Emulators](https://github.com/fullstorydev/emulators) | Bigtable and Cloud Storage | Go libraries or standalone servers | Community / Fullstory | Check client setup and persistent storage; this is a collection, not one shared endpoint. |
| [BigQuery Emulator](https://github.com/goccy/bigquery-emulator) | BigQuery APIs | Go server or Docker | Community | Test required SQL dialect, jobs, types, and storage APIs against real BigQuery. |
| [Bigtable Emulator](https://cloud.google.com/bigtable/docs/emulator) | Bigtable | gcloud component; container workflows | Google | Check mutations and filters; do not infer production replication behavior. |
| [Cloud Tasks Emulator](https://github.com/aertje/cloud-tasks-emulator) | Cloud Tasks | Go server or Docker | Community | Test dispatch, retry, scheduling, and authentication behavior explicitly. |
| [Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator) | Datastore APIs | gcloud component with Java runtime | Google | Check database mode, queries, indexes, and persistence settings. |
| [Fake GCS Server](https://github.com/fsouza/fake-gcs-server) | Cloud Storage | Go library, binary, or Docker | Community | Check signed URLs, endpoint configuration, and supported operations. |
| [Firestore Emulator](https://cloud.google.com/firestore/native/docs/emulator) | Firestore | gcloud or Firebase CLI with Java runtime | Google | Check transactions, queries, and differences from production indexes and limits. |
| [Pub/Sub Emulator](https://cloud.google.com/pubsub/docs/emulator) | Pub/Sub | gcloud component with Java runtime; container workflows | Google | Check acknowledgments, ordering, retry, and subscription features. |
| [Spanner Emulator](https://cloud.google.com/spanner/docs/emulator) | Spanner | gcloud component or Docker | Google | Check SQL dialect, transactions, and documented operational differences. |

### Cross-Cloud

| Tool | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- |
| [cloudemu](https://github.com/stackshy/cloudemu) | AWS, Azure, GCP API simulation | Server, Docker, or embedded Go | Community | In-memory API state is not provisioned infrastructure; test reset and endpoint routing. |
| [Vera](https://github.com/project-vera/vera) | AWS EC2 and Google Compute APIs | Docker Compose | Community | Scope is compute/infrastructure simulation, not all AWS or GCP services. |

### Supporting Tools

| Tool | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- |
| [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-testing.html) | Local serverless execution | CLI with local/container runtimes | AWS | Supporting tool; supply emulators for service dependencies separately. |
| [CRIU](https://criu.org) | Linux process state | Linux checkpoint/restore utility | CRIU project | Verify kernel capabilities, privileges, sockets, and the target host; no emulator-specific certification implied. |
| [DMTCP](https://github.com/dmtcp/dmtcp) | Application process state | Linux launcher and coordinator | DMTCP project | Validate the Python process, libraries, threads, and external connections; not a drop-in OCI snapshot. |
| [Docker Checkpoint/Restore](https://docs.docker.com/reference/cli/docker/checkpoint) | Container process state | Docker Engine with CRIU | Docker / Moby | Experimental; verify daemon/runtime support and restore behavior on the actual Linux host. |
| [Moto Recorder](https://docs.getmoto.org/en/stable/docs/configuration/recorder/index.html) | Recorded Moto requests | Moto feature; ServerMode APIs | Moto project | Enable recording; replay into clean state; generated identifiers need explicit validation. |
| [Podman Checkpoint/Restore](https://podman.io/docs/checkpoint) | Container process state | Podman with CRIU and compatible OCI runtime | Podman project | Verify export/import, host compatibility, privileges, mounted data, and network identity. |
| [Testcontainers Google Cloud Module](https://java.testcontainers.org/modules/gcloud) | Emulator lifecycle in Java tests | Java test library controlling containers | Testcontainers project | Supporting tool; fidelity and licensing come from the selected emulator image. |

## Important distinctions

- **Mocks, API emulators, and real engines:** a mocked `CreateDBInstance` response is different from a reachable database. Some suites start real database or function containers for selected services. Test both control-plane lifecycle and data-plane use where relevant.
- **Official does not mean complete:** provider emulators also have limits. Local results cannot establish production IAM, network isolation, performance, availability, or multi-region behavior.
- **SDK compatibility is not semantic parity:** accepting a request does not prove ordering, authorization, consistency, or error behavior. Test negative paths and retry handling.
- **Suite versus collection:** Firebase integrates multiple local products; Fullstory ships separate Bigtable and Storage implementations. Do not assume a shared endpoint or integrated events because tools occupy the same category.
- **Language versus runtime:** Go code may be used through a server by any SDK; Java may be a runtime prerequisite rather than a verified implementation language. Compare setup and integration requirements first.
- **Local does not necessarily mean offline or free:** downloads, activation, telemetry, feature tiers, commercial-use conditions, and CI terms vary. Inspect the chosen release and plan.

### LocalStack source and product status

The [former LocalStack source repository](https://github.com/localstack/localstack) is archived, but its notice directs users to the unified product. The catalog therefore links current product documentation rather than treating an archived checkout as the supported distribution. Current [installation documentation](https://docs.localstack.cloud/aws/getting-started/installation) requires authentication to activate AWS features; check plan and CI conditions before adopting it.

### Fullstory coverage

Fullstory's repository ships Bigtable and Cloud Storage implementations. Its README also discusses using Google's `pubsub/pstest`; that is not presented here as a third standalone Fullstory emulator. Use the Pub/Sub entry for the provider's server emulator.

## Evaluation checklist

| Dimension | Questions to answer | Evidence to capture |
| --- | --- | --- |
| API coverage | Are required operations, SDK versions, protocols, and errors supported? | Operation checklist tied to application tests. |
| Behavior | Do transactions, ordering, retries, timeouts, and consistency meet test assumptions? | Positive and negative test results against emulator and cloud. |
| Identity and networking | Are permissions enforced, bypassed, or partially modeled? Is TLS required? | Auth mode, trust configuration, and real-cloud test gaps. |
| Lifecycle | Can tests seed, reset, and destroy state? Is persistence intentional? | Repeatable fixtures and a clean teardown run. |
| Runtime | Are OS, architecture, dependencies, and container privileges compatible with CI? | Successful run on the actual CI runner class. |
| Connectivity | Do installation, startup, licensing, or service execution require network access? | Observed egress and offline-start result if needed. |
| Access and terms | Are accounts, tokens, EULA acceptance, paid features, or commercial/CI terms relevant? | Links to the selected release's license and terms. |
| Maintenance | Are releases, documentation, and issue handling sufficient for your use? | Dated review of upstream evidence; no undated “active” badge. |
| Isolation | Can parallel jobs avoid shared-state collisions or real-cloud calls? | Unique endpoints/resources, dummy credentials, and endpoint guards. |
| Operational support | Can failures be diagnosed and upgrades rolled back? | Logs, version/image pin, and upgrade comparison results. |
| Production validation | Which properties are outside the emulator's model? | A named real-cloud suite and an owner for each gap. |

## Reusable emulator baselines

A baseline can capture a modeled landing-zone subset: supported account or project setup, storage, messaging, databases, policies, and test data. It does not establish real-cloud policy enforcement or landing-zone compliance.

| Strategy | What is reused | Suitable starting point | Important boundary |
| --- | --- | --- | --- |
| Declarative setup or seed code | Instructions to recreate resources | SDK/IaC baseline targeting local endpoints | Re-run setup; retain versioned fixture code and account for unsupported APIs. |
| Emulator-native export or persistence | State in an emulator-supported format or data directory | Firebase export/import, LocalStack snapshots, Azurite persistence, DynamoDB Local database | Check feature coverage, version compatibility, licensing, and worker isolation. |
| Request replay | Previously recorded API calls | Moto Recorder | Recreates state; does not serialize process memory or automatically preserve generated IDs. |
| Process/container checkpoint | Captured execution state | CRIU, Podman, experimental Docker checkpointing; DMTCP for suitable processes | Environment-dependent restore; external services and mounted files need separate consistency handling. |
| Filesystem or volume copy | Persisted files | Emulators with documented disk-backed state | A disk copy alone cannot restore in-memory state; stop or quiesce writes before capture. |

Start with reproducible setup. Consider native export or persistence when the baseline is reused across runs or teams, workers need isolated copies, or setup is costly; then consider request replay and consistent disk copies. Trial process checkpoints on compatible Linux hosts only when simpler methods do not meet the need. Compare rebuild and restore time, reliability, artifact size, and the work to regenerate artifacts after baseline changes. Keep a rebuild path when artifacts become incompatible.

See the [reusable emulator baseline workflow](reusable-emulator-baselines.md) for capture steps, restore acceptance tests, and primary documentation. These combinations are research candidates; no emulator-specific checkpoint/restore compatibility test has been performed for this list.

## Adoption sequence

1. **Define a small vertical slice.** Record the SDK/IaC version, operations, event flow, expected errors, and acceptable differences.
2. **Shortlist the smallest useful tool.** A single-service emulator may be enough; select a suite when interactions matter.
3. **Run the same scenario locally and in a controlled cloud account.** Include a negative case, retry path, restart, and cleanup. For IaC, include refresh and destroy rather than plan alone.
4. **Record the result.** Use the decision record below. Pin the tool version or image digest and document gaps.
5. **Integrate in CI.** Wait for readiness, isolate jobs, reset state, capture logs, and confirm cleanup after failure.
6. **Keep cloud validation.** Retain tests for permissions, networking, deployment, performance, and resilience properties that local emulation cannot establish.
7. **Review on upgrades.** Re-run the comparison when the emulator, SDK, provider plugin, or required service behavior changes.

### Minimal decision record

| Field | Record |
| --- | --- |
| Candidate and version | Tool, release or image digest, and primary documentation. |
| Scenario and client | Required services, operations, SDK/IaC version, and test entry point. |
| Results | Passed/failed cases, logs, runtime, and resource use measured on your runner. |
| Known gaps | Semantic differences and features intentionally not modeled. |
| Access and setup | License/plan, activation, dependencies, endpoint and state configuration. |
| Cloud backstop | Tests that remain in a controlled real-cloud environment. |
| Decision | Adopt, trial, or reject; owner and review date. |

## Evidence and reconciliation notes

Documentation review: **2026-09-28**. This is a documentation-based comparison, not a hands-on compatibility certification or performance benchmark.

- Reconciled the original repository catalog with `awesome-cloud-emulators-README-v2.txt`: retained all 18 existing entries, added 12 distinct tools, and merged naming/URL variants rather than duplicating them. That reconciliation produced 28 emulator/mock entries plus 2 supporting tools. A subsequent checkpoint/baseline review added 5 supporting tools or features, bringing the catalog to 35 entries: 28 emulators/mocks and 7 supporting entries.
- Preserved the attachment's provider and multi-service/single-service organization, while keeping detailed comparisons in this guide and a concise catalog in the README.
- Used upstream documentation linked in the tables. Removed exact service counts, benchmark claims, blanket “active” labels, and hard-coded Java versions that do not establish suitability and can quickly become stale.
- Removed the attachment's Cloud Tasks inactivity claim: the [repository metadata](https://api.github.com/repos/aertje/cloud-tasks-emulator) reported a push on 2026-09-09 during this review. A recent push alone does not establish maintenance quality or compatibility.
- Narrowed Fullstory's shipped coverage to Bigtable and Cloud Storage, and Vera's scope to EC2 and Google Compute. Distinguished LocalStack's archived source from its current product.
- Treated the attachment's Hacker News link as discovery context, not technical evidence or an independently verified origin claim for this repository.
- Preserved the existing CC0 license and contribution policy; omitted the attachment's obsolete instruction to add a license file.
