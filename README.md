# CODEOWNERS Coverage Extended

An [Action](https://docs.github.com/en/actions) that checks if files are covered by the [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) file.

Forked from [sibipro/codeowners-coverage](https://github.com/sibipro/codeowners-coverage) to add PR comment support and debug logging improvements.
[sibipro/codeowners-coverage](https://github.com/sibipro/codeowners-coverage) is itself a fork of [austenstone/codeowners-coverage](https://github.com/austenstone/codeowners-coverage), which is no longer actively maintained.

### Changes from sibipro fork

- **PR comments** — New `comment-on-pr` input to post/update a comment on PRs listing uncovered files with coverage percentage. The comment is automatically updated when all files become covered.
- **Debug logging** — Verbose output (file lists, glob patterns) is now gated behind debug mode instead of always logging.

## Usage
Create a workflow (eg: `.github/workflows/seat-count.yml`). See [Creating a Workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

<!-- 
### PAT(Personal Access Token)

You will need to [create a PAT(Personal Access Token)](https://github.com/settings/tokens/new?scopes=admin:org) that has `admin:org` access.

Add this PAT as a secret so we can use it as input `github-token`, see [Creating encrypted secrets for a repository](https://docs.github.com/en/enterprise-cloud@latest/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository). 
### Organizations

If your organization has SAML enabled you must authorize the PAT, see [Authorizing a personal access token for use with SAML single sign-on](https://docs.github.com/en/enterprise-cloud@latest/authentication/authenticating-with-saml-single-sign-on/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on).
-->

#### Example
```yml
name: CODEOWNERS
on:
  workflow_dispatch:

jobs:
  run:
    name: Run Action
    runs-on: ubuntu-latest
    steps:
      - uses: marcofranzen99/codeowners-coverage@v2.0
```

#### Example with PR comment
Post a comment on pull requests listing files not covered by CODEOWNERS.
```yml
name: CODEOWNERS
on:
  pull_request:

permissions:
  pull-requests: write

jobs:
  run:
    name: Check Coverage
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: marcofranzen99/codeowners-coverage@v2.0
        with:
          comment-on-pr: 'true'
```

#### Example changed files in PR
Pass the files in to check only specific files. Combine with [tj-actions/changed-files](https://github.com/tj-actions/changed-files) to check only files changed.
```yml
name: CODEOWNERS
on:
  push:
  pull_request:

jobs:
  run:
    name: Check Coverage
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - id: changed-files
        uses: tj-actions/changed-files@v29.0.3
      - uses: marcofranzen99/codeowners-coverage@v2.0
        with:
          ignore-default: 'true'
          files: ${{ steps.changed-files.outputs.all_changed_files }}
```

## ➡️ Inputs
Various inputs are defined in [`action.yml`](action.yml):

| Name | Description | Default |
| --- | - | - |
| github&#x2011;token | Token to use to authorize. | ${{&nbsp;github.token&nbsp;}} |
| include-gitignore | Whether to filter our files in .gitignore | true |
| ignore-default | Whether to ignore the default rule `*` in CODEOWNERS file | false |
| files          | Filter check to only specific files | N/A |
| comment-on-pr  | Post a PR comment listing uncovered files (requires `pull-requests: write` permission) | false |
<!-- 
## ⬅️ Outputs
| Name | Description |
| --- | - |
| output | The output. |
-->

## Further help
To get more help on the Actions see [documentation](https://docs.github.com/en/actions).
