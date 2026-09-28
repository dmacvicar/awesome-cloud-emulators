# Contributing

Thank you for helping maintain Awesome Cloud Emulators.

## Scope and inclusion

Include tools that emulate or mock identifiable cloud service APIs for local development or automated testing. Supporting tools must have a direct emulator use case.

- Link to upstream repositories or official documentation.
- Explain the provider, service, and practical use case.
- Check documentation, installation instructions, maintenance, licensing, and access requirements.
- Disclose relevant paid features or account requirements when material. Use 💰 for paid commercial plans and 📜 for verified vendor-specific terms/EULAs, following the README legend. Keep markers consistent with the selection guide and link primary evidence; a vendor EULA does not by itself imply payment.
- Prefer useful, documented tools over popularity or star counts.
- Exclude generic HTTP mocks, production databases, and Kubernetes distributions without specific cloud emulation capabilities.
- Keep archived, deprecated, and unsupported tools out of the main list. If a maintained product has moved away from an archived source repository, link its current distribution and document the distinction in the selection guide.
- Disclose any affiliation with a proposed project.

## Entry format

```markdown
- [Project Name](https://example.com/project) - Concise description of the service and useful distinction.
```

Alphabetize entries within sections. Start descriptions with a capital letter and end with a period. Use canonical names, direct HTTPS links, and neutral language. Avoid referral links and unsupported claims of full compatibility.

## Pull requests

1. Check for duplicate entries and open suggestions.
2. Review upstream documentation and, where feasible, try the tool.
3. Update the README and its matching comparison row in `docs/selection-guide.md`. Keep the scenario shortlists and known limitations consistent.
4. Explain usefulness, evidence, verification, and limitations in the PR.
5. Run `npm ci` and `npm run lint` with Node.js 22 or newer.

For corrections, cite upstream evidence. Report broken links with a proposed replacement. Maintainers decide inclusion based on usefulness, evidence, and scope.

Contributions use this repository's CC0 dedication. Listed tools retain their own licenses.
