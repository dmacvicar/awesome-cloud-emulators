# Awesome Cloud Emulators [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Cloud emulators reproduce selected cloud service APIs and behavior for local development and automated testing.

Discover cloud emulators and cloud-specific mocks for **AWS, Microsoft Azure, and Google Cloud (GCP)**. Build repeatable integration tests, shorten feedback loops, and develop without provisioning every dependency in a cloud account.

This list includes provider-built and community tools. Emulators implement subsets of cloud behavior; they do not establish production parity for IAM, networking, quotas, performance, or resilience. Review each project's compatibility, licensing, and runtime requirements before adoption.

## Contents

- [AWS](#aws)
- [Microsoft Azure](#microsoft-azure)
- [Google Cloud and Firebase](#google-cloud-and-firebase)
- [Supporting Tools](#supporting-tools)
- [Choosing an Emulator](#choosing-an-emulator)

## AWS

- [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) - AWS-provided local DynamoDB implementation for developing and testing database interactions.
- [ElasticMQ](https://github.com/softwaremill/elasticmq) - Community message queue with an Amazon SQS-compatible interface, usable as a standalone server or embedded dependency.
- [LocalStack](https://docs.localstack.cloud/aws) - Third-party AWS emulator for applications that integrate multiple cloud services; coverage and access depend on the product edition.
- [Moto](https://github.com/getmoto/moto) - Community AWS mocking library for Python tests, with a standalone server mode for other SDKs and languages.
- [S3Mock](https://github.com/adobe/S3Mock) - Community implementation of a subset of the Amazon S3 API for local integration testing, with Docker and Testcontainers support.

## Microsoft Azure

- [Azure Cosmos DB Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/emulator) - Microsoft-provided local Cosmos DB environment; supported APIs and features vary by emulator variant and platform.
- [Azure Event Hubs Emulator](https://learn.microsoft.com/en-us/azure/event-hubs/overview-emulator) - Microsoft-provided local environment for developing and testing Event Hubs producers and consumers.
- [Azure Service Bus Emulator](https://learn.microsoft.com/en-us/azure/service-bus-messaging/overview-emulator) - Microsoft-provided local Service Bus environment for testing messaging applications in isolation.
- [Azurite](https://github.com/Azure/Azurite) - Open-source Azure Storage emulator from Microsoft for Blob, Queue, and Table service development and testing.

## Google Cloud and Firebase

- [Bigtable Emulator](https://cloud.google.com/bigtable/docs/emulator) - Google-provided local Bigtable environment for developing and testing client applications.
- [Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator) - Google-provided local environment for testing applications that use the Datastore API.
- [Fake GCS Server](https://github.com/fsouza/fake-gcs-server) - Community Google Cloud Storage emulator usable as a standalone server or Go testing library.
- [Firebase Local Emulator Suite](https://firebase.google.com/docs/emulator-suite) - Google-provided suite for local testing across Firebase services, including Authentication, Firestore, Realtime Database, and Cloud Functions.
- [Firestore Emulator](https://cloud.google.com/firestore/native/docs/emulator) - Google-provided local Firestore environment for testing database operations without connecting to a production database.
- [Pub/Sub Emulator](https://cloud.google.com/pubsub/docs/emulator) - Google-provided local Pub/Sub environment for testing publishers and subscribers.
- [Spanner Emulator](https://cloud.google.com/spanner/docs/emulator) - Google-provided local Spanner environment for testing application behavior against supported database APIs.

## Supporting Tools

These tools run local workloads or manage emulator lifecycles; they are not full cloud emulators.

- [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-testing.html) - AWS tooling for local invocation and debugging of serverless applications, including Lambda functions.
- [Testcontainers Google Cloud Module](https://java.testcontainers.org/modules/gcloud) - Java test integrations that manage the lifecycle of Google Cloud emulator containers.

## Choosing an Emulator

Start with the service and API operations your tests actually need. Check authentication behavior, data persistence, event delivery, error responses, CPU architecture, and CI resource requirements in upstream documentation. Account requirements, offline operation, and redistribution rights differ between tools.

Use mocks for isolated logic, emulators for integration behavior, and real cloud environments for deployment and production-specific validation. See the [selection guide](docs/selection-guide.md) for an evaluation checklist.

## Contributing

Suggestions and corrections are welcome. Read the [contribution guidelines](CONTRIBUTING.md) and [code of conduct](CODE_OF_CONDUCT.md), then open a pull request or use an issue template.
