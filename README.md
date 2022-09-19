# action-armttk - Preview/In-Progress 

[![Test](https://github.com/microsoft/action-armttk/workflows/Test/badge.svg)](https://github.com/microsoft/action-armttk/actions?query=workflow%3ATest)
[![reviewdog](https://github.com/microsoft/action-armttk/workflows/reviewdog/badge.svg)](https://github.com/microsoft/action-armttk/actions?query=workflow%3Areviewdog)
[![depup](https://github.com/microsoft/action-armttk/workflows/depup/badge.svg)](https://github.com/microsoft/action-pylint/actions?query=workflow%3Adepup)
[![release](https://github.com/microsoft/action-armttk/workflows/release/badge.svg)](https://github.com/microsoft/action-armttk/actions?query=workflow%3Arelease)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/microsoft/action-armttk?logo=github&sort=semver)](https://github.com/microsoft/action-armttk/releases)
[![action-bumpr supported](https://img.shields.io/badge/bumpr-supported-ff69b4?logo=github&link=https://github.com/haya14busa/action-bumpr)](https://github.com/haya14busa/action-bumpr)

This repo contains a action to run [armttk](https://github.com/azure/armttk).

## Input

```yaml
inputs:
  github_token:
    description: "GITHUB_TOKEN"
    required: true
    default: "${{ github.token }}"
  workdir:
    description: "Working directory relative to the root directory."
    required: false
    default: "."
  glob_pattern:
    description: "Glob pattern of the files to lint"
    required: false
    default: "."
  ### Flags for reviewdog ###
  tool_name:
    description: "Tool name to use for reviewdog reporter."
    required: false
    default: "armttk"
  level:
    description: "Report level for reviewdog [info,warning,error]"
    required: false
    default: "error"
  reporter:
    description: "Reporter of reviewdog command [github-pr-check,github-pr-review]."
    required: false
    default: "github-pr-check"
  filter_mode:
    description: |
      Filtering mode for the reviewdog command [added,diff_context,file,nofilter].
      Default is added.
    required: false
    default: "added"
  fail_on_error:
    description: |
      Exit code for reviewdog when errors are found [true,false]
      Default is `false`.
    required: false
    default: "false"
  reviewdog_flags:
    description: "Additional reviewdog flags"
    required: false
    default: ""
```

## Usage

```yaml
name: reviewdog
on: [pull_request]
jobs:
  armttk:
    name: runner / armttk
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: microsoft/action-armttk@v1
        with:
          github_token: ${{ secrets.github_token }}
          # Change reviewdog reporter if you need [github-pr-check,github-check,github-pr-review].
          reporter: github-pr-review
          # Change reporter level if you need.
          # GitHub Status Check won't become failure with warning.
          level: warning
          glob_pattern: "**/*.py"
```

## Development

### Release

#### [haya14busa/action-bumpr](https://github.com/haya14busa/action-bumpr)
You can bump version on merging Pull Requests with specific labels (bump:major,bump:minor,bump:patch).
Pushing tag manually by yourself also work.

#### [haya14busa/action-update-semver](https://github.com/haya14busa/action-update-semver)

This action updates major/minor release tags on a tag push. e.g. Update v1 and v1.2 tag when released v1.2.3.
ref: https://help.github.com/en/articles/about-actions#versioning-your-action

### Lint - reviewdog integration

This reviewdog action template itself is integrated with reviewdog to run lints
which is useful for Docker container based actions.

![reviewdog integration](https://user-images.githubusercontent.com/3797062/72735107-7fbb9600-3bde-11ea-8087-12af76e7ee6f.png)

Supported linters:

- [reviewdog/action-shellcheck](https://github.com/reviewdog/action-shellcheck)
- [reviewdog/action-hadolint](https://github.com/reviewdog/action-hadolint)
- [reviewdog/action-misspell](https://github.com/reviewdog/action-misspell)

### Dependencies Update Automation

This repository uses [reviewdog/action-depup](https://github.com/reviewdog/action-depup) to update
reviewdog version.

[![reviewdog depup demo](https://user-images.githubusercontent.com/3797062/73154254-170e7500-411a-11ea-8211-912e9de7c936.png)](https://github.com/reviewdog/action-template/pull/6)
