<div align="center">
  <a href="https://github.com/activist-org/i18n-check-action"><img src="https://raw.githubusercontent.com/activist-org/i18n-check-action/main/.github/resources/i18nCheckGitHubBanner.png" width=1024 alt="i18n check logo"></a>
</div>

[![issues](https://img.shields.io/github/issues/activist-org/i18n-check-action?label=%20&logo=github)](https://github.com/activist-org/i18n-check-action/issues)
[![python](https://img.shields.io/badge/Python-4B8BBE.svg?logo=python&logoColor=ffffff)](https://github.com/activist-org/i18n-check-action/blob/main/CONTRIBUTING.md)
[![license](https://img.shields.io/github/license/activist-org/i18n-check-action.svg?label=%20)](https://github.com/activist-org/i18n-check-action/blob/main/LICENSE.txt)
[![coc](https://img.shields.io/badge/Contributor%20Covenant-ff69b4.svg)](https://github.com/activist-org/i18n-check-action/blob/main/.github/CODE_OF_CONDUCT.md)
[![matrix](https://img.shields.io/badge/Matrix-000000.svg?logo=matrix&logoColor=ffffff)](https://matrix.to/#/#activist_community:matrix.org)

### A GitHub action to check i18n/L10n keys and values

`i18n-check-action` is a GitHub action for [i18n-check](https://github.com/activist-org/i18n-check) used to automate the validation of keys and values of your internationalization and localization processes.

Developed by the [activist community](https://github.com/activist-org), this action is meant to assure that development and i18n/L10n teams are in sync when using JSON based localization processes. The action can be expanded later to work for other file type processes as needed.

> [!NOTE]
> For the Python package please see [activist-org/i18n-check](https://github.com/activist-org/i18n-check).

<a id="contents"></a>

# **Contents**

- [Conventions](#contentions)
- [How it works](#how-it-works)
- [Configuration](#configuration)
- [Contributors](#contributors)

<a id="conventions"></a>

# Conventions [`⇧`](#contents)

[activist](https://github.com/activist-org/activist) i18n keys follow the following conventions that are enforced by `i18n-check`:

- All key base paths should be the file path where the key is used prepended with `i18n.`
  - Starting i18n keys with a common identifier allows them to be found within checks
- If a key is used in more than one file, then the lowest common directory followed by `_global` is the base path
- Base paths should be followed by a minimally descriptive content reference
  - Only the formatting of these content references is checked via `i18n-check`
- Separate base directory paths by periods (`.`)
- Separate all directory and file name components as well as content references by underscores (`_`)
- Repeat words in file paths for sub directory organization should not be repeated in the key

> [!NOTE]
> An example valid key is:
>
> Key: `"components.component_name.CONTENT_REFERENCE"`
>
> File: `components/component/ComponentName.ext`

<a id="how-it-works"></a>

# How it works [`⇧`](#contents)

### Arguments

You provide `i18n-check-action` with the following arguments:

- `src-dir`: The path to the directory that has source code to check
- `i18n-dir`: The directory path to your i18n files
- `i18n-src`: The name of the i18n source file

### Checks

From there the following checks are ran across your codebase:

- `invalid_keys`: Does the include i18n keys that are not within the source file?
- `key_identifiers`: Does the source file have keys that don't match the above format or name conventions?
  - Rename them so i18n key usage is consistent and their scope is communicated in their name.
- `unused_keys`: Does the source file have keys that are not used in the codebase?
  - Remove them so the localization team isn't working on strings that aren't used.
- `non_source_keys`: Do the target locale files have keys that are not in the source file?
  - Remove them as they won't be used in the application.
- `repeat_values`: Does the source file have repeat values that can be combined into a single key?
  - Combine them so the localization team only needs to localize one of them.

Each of the above checks is ran in parallel with directions for how to fix the i18n files being provided when errors are raised. Checks can also be disabled in the workflow via options passed in the configuration YAML file.

<a id="configuration"></a>

# Configuration [`⇧`](#contents)

To use this action, make a file `.github/workflows/i18n-check.yml` and include the following template configuration:

```yaml
name: i18n-check

on:
  pull_request:
  push:
    branches: [opened, reopened, synchronize]
    paths:
      - "**/SOURCE_I18N_FILE"

  push:
    branches:
      - main
    paths:
      - "**/SOURCE_I18N_FILE"

jobs:
  i18n-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v3
      - uses: activist/i18n-check-action@v1
        with:
          src-dir: PATH_TO_CODE_TO_CHECK
          i18n-dir: PATH_TO_I18N_FILES
          i18n-src: PATH_TO_SOURCE_I18N_FILE
```

<a id="contributors"></a>

# Contributors [`⇧`](#contents)

Thanks to all our amazing [contributors](https://github.com/activist-org/i18n-check-action/graphs/contributors)! ❤️

<a href="https://github.com/activist-org/i18n-check-action/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=activist-org/i18n-check-action" />
</a>
