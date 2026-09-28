# Awesome Cloud Emulators [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Cloud emulators reproduce selected cloud service APIs and behavior for local development and automated testing.

Discover emulators and cloud-specific test doubles for **AWS, Microsoft Azure, and Google Cloud (GCP)**. Build repeatable integration tests and shorten feedback loops without provisioning every dependency in a cloud account.

Entries are grouped by the APIs they emulate, not where they run. Multi-service means a suite or collection spanning services; it does not promise integrated behavior between them. Provider-built, community, and commercial tools are included. Supporting tools are listed separately.

**Emulation is not full service parity.** Check supported operations, persistence, identity behavior, runtime requirements, licensing, and access conditions. Use real-cloud tests for production-specific guarantees. The [selection guide](docs/selection-guide.md) includes scenario-based shortlists, a comparison of every entry, and an evaluation checklist.

**License markers:** 💰 Paid commercial plan or license for commercial use; a free tier or exception may exist. 📜 Vendor-specific software terms or an EULA apply; this does **not** mean a fee is required. Open-source licenses still apply to unmarked tools. Markers highlight verified conditions, not an exhaustive license audit.

See [license and access notes](docs/selection-guide.md#license-and-access-notes) for the marked tools and primary sources.

## Contents

- [AWS](#aws)
  - [AWS Multi-Service](#aws-multi-service)
  - [AWS Single-Service](#aws-single-service)
- [Microsoft Azure](#microsoft-azure)
  - [Azure Multi-Service](#azure-multi-service)
  - [Azure Single-Service](#azure-single-service)
- [Google Cloud and Firebase](#google-cloud-and-firebase)
  - [Google Cloud Multi-Service](#google-cloud-multi-service)
  - [Google Cloud Single-Service](#google-cloud-single-service)
- [Cross-Cloud](#cross-cloud)
- [Supporting Tools](#supporting-tools)
- [Choosing an Emulator](#choosing-an-emulator)

## AWS

### AWS Multi-Service

- [fakecloud](https://fakecloud.dev) - Local AWS API emulator with test SDKs for inspecting effects, resetting state, and controlling asynchronous processors.
- [Floci](https://floci.io/floci) - Community AWS emulator that exposes multiple service APIs through a shared local endpoint.
- [LocalEmu](https://localemu.cloud) - Python-based AWS emulator with persistent local state and Docker-backed execution for selected services.
- [LocalStack 💰 📜](https://docs.localstack.cloud/aws) - Vendor-maintained AWS emulation platform distributed as a container; commercial use requires an appropriate paid plan, subject to vendor exceptions; a free non-commercial Hobby plan is available and activation requires authentication.
- [MiniStack](https://ministack.org) - Community AWS emulator with multi-account and multi-region support and optional engine-backed services.
- [Moto](https://github.com/getmoto/moto) - Community AWS mocking library for Python tests, with a standalone server mode for other SDKs and languages.

### AWS Single-Service

- [DynamoDB Local 📜](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) - AWS-provided local DynamoDB implementation for developing and testing database interactions.
- [ElasticMQ](https://github.com/softwaremill/elasticmq) - Community message queue with an Amazon SQS-compatible interface, usable as a standalone server or embedded dependency.
- [S3Mock](https://github.com/adobe/S3Mock) - Community implementation of a subset of the Amazon S3 API for local integration testing, with Docker and Testcontainers support.


## Microsoft Azure

### Azure Multi-Service

- [Floci AZ](https://github.com/floci-io/floci-az) - Community Azure emulator exposing multiple service APIs, with Docker-backed execution for selected services.

### Azure Single-Service

- [Azure Cosmos DB Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/emulator) - Microsoft-provided local Cosmos DB environment; supported APIs and features vary by emulator variant and platform.
- [Azure Event Hubs Emulator 📜](https://learn.microsoft.com/en-us/azure/event-hubs/overview-emulator) - Microsoft-provided local environment for developing and testing Event Hubs producers and consumers.
- [Azure Key Vault Emulator](https://github.com/james-gould/azure-keyvault-emulator) - Community Key Vault emulator for testing Azure SDK clients locally, with Docker and .NET Aspire integration.
- [Azure Service Bus Emulator 📜](https://learn.microsoft.com/en-us/azure/service-bus-messaging/overview-emulator) - Microsoft-provided local Service Bus environment for testing messaging applications in isolation.
- [Azurite](https://github.com/Azure/Azurite) - Open-source Azure Storage emulator from Microsoft for Blob, Queue, and Table service development and testing.


## Google Cloud and Firebase

### Google Cloud Multi-Service

- [Firebase Local Emulator Suite](https://firebase.google.com/docs/emulator-suite) - Google-provided suite for local testing across Firebase services, including Authentication, Firestore, Realtime Database, and Cloud Functions.
- [Floci GCP](https://floci.io/floci-gcp) - Community Google Cloud emulator covering services such as Cloud Storage, Pub/Sub, and Firestore through local APIs.
- [Fullstory Emulators](https://github.com/fullstorydev/emulators) - Community collection of Bigtable and Cloud Storage emulators, usable as Go libraries or standalone servers with optional persistence.

### Google Cloud Single-Service

- [BigQuery Emulator](https://github.com/goccy/bigquery-emulator) - Community BigQuery API emulator implemented in Go for local database and query integration tests.
- [Bigtable Emulator](https://cloud.google.com/bigtable/docs/emulator) - Google-provided local Bigtable environment for developing and testing client applications.
- [Cloud Tasks Emulator](https://github.com/aertje/cloud-tasks-emulator) - Community Cloud Tasks emulator for local queue and task-dispatch testing.
- [Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator) - Google-provided local environment for testing applications that use the Datastore API.
- [Fake GCS Server](https://github.com/fsouza/fake-gcs-server) - Community Google Cloud Storage emulator usable as a standalone server or Go testing library.
- [Firestore Emulator](https://cloud.google.com/firestore/native/docs/emulator) - Google-provided local Firestore environment for testing database operations without connecting to a production database.
- [Pub/Sub Emulator](https://cloud.google.com/pubsub/docs/emulator) - Google-provided local Pub/Sub environment for testing publishers and subscribers.
- [Spanner Emulator](https://cloud.google.com/spanner/docs/emulator) - Google-provided local Spanner environment for testing application behavior against supported database APIs.


## Cross-Cloud

- [cloudemu](https://github.com/stackshy/cloudemu) - In-memory simulation of AWS, Azure, and Google Cloud APIs, runnable as a server or embedded in Go tests.
- [Vera](https://github.com/project-vera/vera) - Local simulation of AWS EC2 and Google Compute APIs for infrastructure automation tests.


## Supporting Tools

These tools run local workloads or manage emulator lifecycles; they are not full cloud emulators.

- [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-testing.html) - AWS tooling for local invocation and debugging of serverless applications, including Lambda functions.
- [Testcontainers Google Cloud Module](https://java.testcontainers.org/modules/gcloud) - Java test integrations that manage the lifecycle of Google Cloud emulator containers.

## Choosing an Emulator

Start with the operations and failure paths your tests need. Use mocks for isolated logic, service emulators for API integration, and broader suites when the interactions between services matter. Infrastructure API simulation does not necessarily provision a working database, VM, network, or function runtime.

Compare candidates using the [selection guide](docs/selection-guide.md#start-with-your-test-scenario). Verify your SDK or IaC provider version, endpoints, resource lifecycle, and cleanup behavior against a controlled cloud environment.

## Contributing

Suggestions and corrections are welcome. Read the [contribution guidelines](CONTRIBUTING.md) and [code of conduct](CODE_OF_CONDUCT.md), then open a pull request or use an issue template. Include upstream evidence, important limitations, and any affiliation.
