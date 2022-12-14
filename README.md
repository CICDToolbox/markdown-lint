<p align="center">
    <a href="https://github.com/CICDToolbox/">
        <img src="https://cdn.wolfsoftware.com/assets/images/github/organisations/cicdtoolbox/black-and-white-circle-256.png" alt="CICDToolbox logo" />
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/markdown-lint/actions/workflows/cicd-pipeline.yml">
        <img src="https://img.shields.io/github/workflow/status/CICDToolbox/markdown-lint/CICD%20Pipeline/master?style=for-the-badge" alt="Github Build Status">
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/releases/latest">
        <img src="https://img.shields.io/github/v/release/CICDToolbox/markdown-lint?color=blue&label=Latest%20Release&style=for-the-badge" alt="Release">
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/releases/latest">
        <img src="https://img.shields.io/github/commits-since/CICDToolbox/markdown-lint/latest.svg?color=blue&style=for-the-badge" alt="Commits since release">
    </a>
    <br />
    <a href=".github/CODE_OF_CONDUCT.md">
        <img src="https://img.shields.io/badge/Code%20of%20Conduct-blue?style=for-the-badge" />
    </a>
    <a href=".github/CONTRIBUTING.md">
        <img src="https://img.shields.io/badge/Contributing-blue?style=for-the-badge" />
    </a>
    <a href=".github/SECURITY.md">
        <img src="https://img.shields.io/badge/Report%20Security%20Concern-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/issues">
        <img src="https://img.shields.io/badge/Get%20Support-blue?style=for-the-badge" />
    </a>
    <br />
    <a href="https://wolfsoftware.com/">
        <img src="https://img.shields.io/badge/Created%20by%20Wolf%20Software-blue?style=for-the-badge" />
    </a>
</p>

## Overview

A tool to lint your markdown files in CI/CD pipelines using [markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli).

This tool has been written and tested using GitHub Actions but it should workout of the box with a lot of other CI/CD tools.

## Usage

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup node 16
        uses: actions/setup-node@v3
        with:
          node-version: 16
      - name: Run Markdown Lint
        run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/markdown-lint/master/pipeline.sh)
```

### Other Options

The following environment variables can be set in order to customise the script.

| Name          | Purpose | Default Value |
| ------------- | ------- | ------------- |
| EXCLUDE_FILES | A comma separated list of files to exclude from being scanned. You can also use `regex` to do pattern matching. | Unset |
| REPORT_ONLY   | Generate the report but do not fail the build even if an error occurred. | False |
| SHOW_ERRORS   | Show the actual errors instead of just which files had errors. | True |
| SHOW_SKIPPED  | Show which files are being skipped. | False | 

You can use any combination of the above settings.

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup node 16 
        uses: actions/setup-node@v3
        with:
          node-version: 16
      - name: Run Markdown Lint
        env:
          REPORT_ONLY: true
          SHOW_ERRORS: true
        run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/markdown-lint/master/pipeline.sh)
```

### Example Output

This is an example of the output report generated by this tool, this is the actual output from the tool running against itself.

```html
-------------------------------------------------------------------------- Stage 1 - Parameters --
 No parameters given
--------------------------------------------------------------- Stage 2 - Install Prerequisites --
 [  OK  ] npm install -g markdownlint-cli
---------------------------------------------------------- Stage 3 - Run markdownlint (v0.32.2) --
 [  OK  ] .github/CODE_OF_CONDUCT.md
 [  OK  ] .github/CONTRIBUTING.md
 [  OK  ] .github/PULL_REQUEST_TEMPLATE.md
 [  OK  ] .github/SECURITY.md
 [  OK  ] LICENSE.md
 [  OK  ] README.md
------------------------------------------------------------------------------ Stage 4 - Report --
 Total: 6, OK: 6, Failed: 0, Skipped: 0
---------------------------------------------------------------------------- Stage 5 - Complete --
```

### File Identification

Markdown files are identified using the following code:

```shell
[[ ${filename} =~ \.md$ ]]
```

> There is not magic type for markdown files so file -b is of not use for identifying the files.
