# Publish awesome-cloud-emulators

The archive includes hidden configuration directories but no `.git` history. Run these commands inside the extracted folder. Install Git, Node.js 22 or newer, and the [GitHub CLI](https://cli.github.com) first.

## Create a public repository

```bash
gh auth login
git init -b main
git add .
git commit -m "Initialize Awesome Cloud Emulators"
gh repo create awesome-cloud-emulators --public --source=. --remote=origin --push
```

This creates a public repository on your authenticated personal account. For an organization, use `ORGANIZATION/awesome-cloud-emulators` instead. If the name exists, inspect that repository before proceeding; do not overwrite its history. Do not initialize a separate README or license remotely.

## Apply description and topics

```bash
node -e 'const m=require("./docs/github-metadata.json"); require("node:child_process").execFileSync("gh",["repo","edit","--description",m.description,"--add-topic",m.topics.join(",")],{stdio:"inherit"})'
```

The metadata file supplies a search-friendly description and 20 relevant topics, GitHub's maximum. Use lowercase letters, numbers, and hyphens in topics.

## Finish setup

1. Confirm `main` is the default branch and GitHub Actions are enabled.
2. Run `npm ci` and `npm run lint:full` after publication. Inspect Lint and manually run Check links in the Actions tab.
3. Optionally add a branch ruleset requiring pull requests and a passing lint check.
4. Add a public contact method to your GitHub profile or customize CODE_OF_CONDUCT.md with a private reporting address.
5. Optionally enable Discussions and upload a social preview image.

## Discoverability

Use the description in GitHub's About field. Keep the README opening clear about cloud emulators, AWS, Azure, Google Cloud, local development, and integration testing. Add useful service coverage and descriptions as the list grows. Share comparisons and practical examples with relevant communities. Avoid keyword stuffing, duplicate entries, and unrelated topics. Topics can aid discovery; they cannot guarantee search-engine rankings.

See [GitHub's topic documentation](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics) and the [maintenance guide](maintaining.md).
