<!-- markdownlint-disable -->
<p align="center">
    <a href="https://github.com/CICDToolbox/">
        <img src="https://cdn.wolfsoftware.com/assets/images/github/organisations/cicdtoolbox/black-and-white-circle-256.png" alt="CICDToolbox logo" />
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/markdown-lint/actions/workflows/cicd.yml">
        <img src="https://img.shields.io/github/actions/workflow/status/CICDToolbox/markdown-lint/cicd.yml?branch=master&label=build%20status&style=for-the-badge" alt="Github Build Status" />
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/blob/master/LICENSE.md">
        <img src="https://img.shields.io/github/license/CICDToolbox/markdown-lint?color=blue&label=License&style=for-the-badge" alt="License">
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint">
        <img src="https://img.shields.io/github/created-at/CICDToolbox/markdown-lint?color=blue&label=Created&style=for-the-badge" alt="Created">
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/markdown-lint/releases/latest">
        <img src="https://img.shields.io/github/v/release/CICDToolbox/markdown-lint?color=blue&label=Latest%20Release&style=for-the-badge" alt="Release">
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/releases/latest">
        <img src="https://img.shields.io/github/release-date/CICDToolbox/markdown-lint?color=blue&label=Released&style=for-the-badge" alt="Released">
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/releases/latest">
        <img src="https://img.shields.io/github/commits-since/CICDToolbox/markdown-lint/latest.svg?color=blue&style=for-the-badge" alt="Commits since release">
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/markdown-lint/blob/master/.github/CODE_OF_CONDUCT.md">
        <img src="https://img.shields.io/badge/Code%20of%20Conduct-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/blob/master/.github/CONTRIBUTING.md">
        <img src="https://img.shields.io/badge/Contributing-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/blob/master/.github/SECURITY.md">
        <img src="https://img.shields.io/badge/Report%20Security%20Concern-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/markdown-lint/issues">
        <img src="https://img.shields.io/badge/Get%20Support-blue?style=for-the-badge" />
    </a>
</p>

## Overview

A tool to validate your markdown files in using [markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli).

This tool has been tested against the following:

1. GitHub Actions
2. Travis CI
3. CircleCI
4. BitBucket pipelines
5. Local command line

However due to the way that they are built they should work on most CICD platforms where you can run arbitrary scripts.

We provide a [script](https://github.com/CICDToolbox/get-all-tools) which pulls the latest copy of all the CICD tools and
places them in a local bin directory to allow them to be run any time locally for added validation.

## Basic Usage

```yml
on: [push, pull_request]

jobs:
  build:
    name: Markdown Lint
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the Repository
      - uses: actions/checkout@v4

      - name: Setup node 22
        uses: actions/setup-node@v4
        with:
          node-version: "22"

      - name: Run Markdown Lint
        run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/markdown-lint/master/pipeline.sh)
```

### Configuration Options

The following environment variables can be set in order to customise the script.

| Name          | Default Value | Purpose                                                                                                         |
| :------------ | :-----------: | :-------------------------------------------------------------------------------------------------------------- |
| INCLUDE_FILES |     Unset     | A comma separated list of files to include for being scanned. You can also use `regex` to do pattern matching.  |
| EXCLUDE_FILES |     Unset     | A comma separated list of files to exclude from being scanned. You can also use `regex` to do pattern matching. |
| NO_COLOR      |     False     | Turn off the color in the output.                                                                               |
| REPORT_ONLY   |     False     | Generate the report but do not fail the build even if an error occurred.                                        |
| SHOW_ERRORS   |     True      | Show the actual errors instead of just which files had errors.                                                  |
| SHOW_SKIPPED  |     False     | Show which files are being skipped.                                                                             |

> If you set INCLUDE_FILES - it will skip ALL files that do not match, including anything in EXCLUDE_FILES.

You can use any combination of the above settings.

```yml
on: [push, pull_request]

jobs:
  build:
    name: Markdown Lint
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the Repository
      - uses: actions/checkout@v4

      - name: Setup node 22
        uses: actions/setup-node@v4
        with:
          node-version: "22"

      - name: Run Markdown Lint
        env:
          REPORT_ONLY: true
          SHOW_ERRORS: true
        run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/markdown-lint/master/pipeline.sh)
```

### Example Output

This is an example of the output report generated by this tool, this is the actual output from the tool running against itself.

```html
--------------------------------------------------------------------- Stage 1: Parameters --
 No parameters given
---------------------------------------------------------- Stage 2: Install Prerequisites --
 [ OK ] markdownlint is already installed
----------------------------------------------------- Stage 3: Run markdownlint (v0.32.2) --
 [ OK ] .github/CODE_OF_CONDUCT.md
 [ OK ] .github/CONTRIBUTING.md
 [ OK ] .github/PULL_REQUEST_TEMPLATE.md
 [ OK ] .github/SECURITY.md
 [ OK ] LICENSE.md
 [ OK ] README.md
------------------------------------------------------------------------- Stage 4: Report --
 Total: 6, OK: 6, Failed: 0, Skipped: 0
----------------------------------------------------------------------- Stage 5: Complete --
```

### File Identification

Markdown files are identified using the following code:

```shell
[[ ${filename} =~ \.md$ ]]
```

There is not magic type for markdown files so file -b is of not use for identifying the files.

<br />
<p align="right"><a href="https://wolfsoftware.com/"><img src="https://img.shields.io/badge/Created%20by%20Wolf%20on%20behalf%20of%20Wolf%20Software-blue?style=for-the-badge" /></a></p>
