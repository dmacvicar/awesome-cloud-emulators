# Selecting a cloud emulator

Evaluate an emulator against a representative test, not the number of services on its homepage.

| Dimension | Questions |
| --- | --- |
| API coverage | Are your operations, SDK versions, protocols, and error responses supported? |
| Behavior | Do transactions, ordering, retries, and consistency match application assumptions? |
| Identity | Are permissions enforced, bypassed, or partially modeled? |
| Lifecycle | Can tests seed, reset, and destroy state independently? |
| Runtime | Are your OS, CPU architecture, container platform, and CI environment supported? |
| Connectivity | Can it run offline? Do startup, licensing, or downloads require a network? |
| Access and terms | Is an account, token, paid edition, or EULA acceptance required? |
| Maintenance | Are supported versions and compatibility gaps documented? |
| Isolation | Can parallel tests avoid collisions and unintended real-cloud calls? |
| Production validation | Which tests still need a real cloud environment? |

## Adoption sequence

1. Pick an integration scenario and record expected cloud behavior.
2. Run it against the emulator and a controlled cloud environment.
3. Document differences and decide which are acceptable for each test layer.
4. Pin tool versions or image digests, isolate credentials and endpoints, and reset state.
5. Keep real-cloud tests for deployment, identity, networking, and service guarantees.

Emulators provide feedback, not evidence of production security, availability, or performance.
