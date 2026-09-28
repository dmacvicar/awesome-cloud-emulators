# Maintaining the list

Review suggestions for scope, evidence, usefulness, and duplication. Inspect scheduled link failures weekly; transient rate limits do not prove abandonment. Review upstream maintenance, compatibility, and access requirements quarterly.

Run `npm ci` and `npm run lint` after edits. Scheduled link checks are separate from PR linting because external sites can be unavailable. Review and test Dependabot updates before merging.

## Provenance

This scaffold and starter descriptions were prepared with AI assistance. Upstream documentation was consulted on 2026-09-28; the listed tools were not installed or functionally tested during scaffolding. Maintainers should validate entries through their own use and review before recommending them as mature curation.

## Awesome directory eligibility

The Awesome format and badge do not imply endorsement or inclusion in the central directory. Its current checklist requires at least 30 days of history and states that AI-generated lists are not accepted. Do not submit this generated scaffold as if it satisfies those requirements. Check current rules and respect the maintainers' admission policy.

- [Creating a list](https://github.com/sindresorhus/awesome/blob/main/create-list.md)
- [Submission checklist](https://github.com/sindresorhus/awesome/blob/main/pull_request_template.md)
- [Awesome lint](https://github.com/sindresorhus/awesome-lint)

Content checks and directory eligibility are separate concerns. Repository age and GitHub metadata cannot be established from a downloaded scaffold.

`npm run lint` applies Awesome content checks while excluding only the GitHub repository-metadata rule, so it works before publication and without API access. After publishing, run `npm run lint:full` to include that rule. A passing lint run is not proof of directory eligibility.
