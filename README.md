# (GitHub Action) Version Branch

[![GitHub Release](https://img.shields.io/github/package-json/v/fortinet/github-action-version-branch)]()
[![Node version](https://img.shields.io/badge/node-^12.10-brightgreen.svg?style=flat)]()

A GitHub Action that resolves a version branch (aka: head branch), and a set of version details for a semver-style head version.

The name of the `head branch`, and the the `head version` are both resolved by the given inputs. They remain idempotent and can be passed down to any other action in a workflow.

If the `head branch` already exists, it returns the existing branch, otherwise, a new branch will be created and returned. Therefore, an indicator `is-new-branch` will be provided. Even though this action may resolve to an existing `head branch`, it does not do `git diff` between the `base branch` and the `head branch`.

The `head version` is resolved to a semver (see: [semver.org](semver.org)) with the given inputs. It returns details including: `major`, `minor`, `patch`, `pre-id`, `pre-num`, `is-prerelease`.


## Usage

```yaml
- uses: fortinet/github-action-version-branch@main
  with:
    # The GitHub automatically created secret to use in your workflow for authentications.
    # see: https://docs.github.com/en/actions/reference/authentication-in-a-workflow
    # Can be obtained from ${{ secrets.GITHUB_TOKEN }}.
    # Must be provided explicitly.
    # Required. No default.
    github-token: ''
    # The base branch that the pull request will be going to.
    # Required. No default.
    base-branch: ''
    # The name prefix to include in the new branch name.
    # The new branch name format is: <prefix><version>. Example: Rel_branch1.0.0-rc.1
    # The version will be retrieved from the base-branch, bumped according to the version-level.
    # Optional. Default: 'rel_'.
    name-prefix: 'rel_'
    # The level of the semver version.
    # Accepted inputs: major, minor, patch, and prerelease.
    # Optional. No default.
    version-level: ''
    # A valid semver pre-release identifier.
    # Used in command: npm version --preid=<pre-id>
    # If specified, the versioning behavior will become
    #  'premajor', 'preminor', 'prepatch', and 'prerelease'.
    # Required. No default.
    pre-id: ''
    # The custom version to bump to. If specified, it will be used as the version.
    # It needs to be a valid semver format.
    # Optional. No default.
    custom-version: ''
    # The pull request number for info retrieval uses. If present, the information will be returned
    # in the action output, and no new branch will be created as opposed to creating a new branch by
    # default without the pr-number.
    # Optional. No default.
    pr-number: ''
```

## Inputs

The following table contains the inputs this action accepts. Please find the description for each in the [action.yml](action.yml).

| Name                | Required | Default value                        | Remark |
|---------------------|----------|--------------------------------------|--------|
| github-token        | Yes      |                                      |  |
| base-branch         | Yes      |                                      |  |
| name-prefix         | No       | rel_                                 |  |
| version-level       | Conditional      | No default but only accepts 'major', 'minor', 'patch', and 'prerelease'                     | Required only if custom-version isn't specified |
| pre-id              | No       |                                      |  |
| custom-version      | No       |                                      | If set, version-level and pre-id will be ignored |
| pr-number           | No       |                                      |

## Outputs

The following table contains the outputs of this action.

| Name                | Description                        |
|---------------------|--------------------------------------|
| base-branch         | The same value as the input.         |
| base-version        | The version extracted from the top level package.json in the base-branch. |
| head-branch         | The name of the returned version branch.         |
| head-version        | The version resolved by the given inputs. |
| is-new-branch       | A boolean indicator for whether the head branch is a new branch or existing one. |
| is-prerelease       | A boolean indicator for whether the head version is resolved as a prerelease or not. |
| major               | The major version of the new version. |
| minor               | The minor version of the new version. |
| patch               | The patch version of the new version. |
| pre-id              | The non-incremental part of the new version, if it is a prerelase. |
| pre-inc             | The incremental part of the new version, if it is a prerelase. |

## Support

Fortinet-provided scripts in this and other GitHub projects do not fall under the regular Fortinet technical support scope and are not supported by FortiCare Support Services.
For direct issues, please refer to the [Issues](https://github.com/fortinet/github-action-version-branch/issues) tab of this GitHub project.
For other questions related to this project, contact [github@fortinet.com](mailto:github@fortinet.com).

## License

[License](./LICENSE) © Fortinet Technologies. All rights reserved.
