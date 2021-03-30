# (GitHub Action) Version PR

[![GitHub Release](https://img.shields.io/github/package-json/v/fortinet/github-action-version-pr)]()
[![Node version](https://img.shields.io/badge/node-^12.x-brightgreen.svg?style=flat)]()

A GitHub Action that creates a pull request for versioning.

## Usage

```yaml
- uses: fortinet/github-action-version-pr@main
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
    # The head branch which the pull request is taking from.
    # Required. No default.
    head-branch: ''
    # A boolean indicator for the behavior of re-issuing a pull request with the same head and
    # base branch. If set 'true', the action will fail for existing open pr.
    # Optional. Default: 'false'.
    pr-fail-if-exist: 'false'
    # A boolean indicator for whether to create a draft pull request or actual one.
    # Optional. Default: 'false'.
    pr-create-draft: 'false'
    # The uri of the template of pre-configured values for the pull request to be used.
    # The template includes the following properties for pre-configuration:
    # title
    # description
    # reviewers
    # team-reviewers
    # labels
    #
    # See examples/templates/version-pr.yml
    #
    # The location must be a relative path to where this GitHub action is used.
    # It defaults to .github/workflows/templates/version-pr.yml
    # of the workflow that uses it. If such pre-configured template does not exist, these
    # will be left empty.
    # Optional. Default: .github/workflows/templates/version-pr.yml
    pr-template-uri: ''
    # The title for the pull request. If non-empty value is specified, the value here will be
    # used and override the title property value in pr-template-uri.
    # Optional. No default.
    pr-title: ''
    # The description for the pull request. If non-empty value is specified,
    # the value here will be used and override the description property value in pr-template-uri.
    # Optional. No default.
    pr-description: ''
    # A comma-separated list of the reviewers (usernames) for the pull request.
    # If non-empty value is specified, the value here will be used and override the
    # reviewers property value in pr-template-uri.
    # Giving a single comma as the value here, i.e. ',' can override and disable this setting.
    # Optional. No default.
    pr-reviewers: ''
    # A comma-separated list of the team reviewers (usernames) for the pull request.
    # If non-empty value is specified, the value here will be used and override the
    # team reviewers property value in pr-template-uri.
    # Giving a single comma as the value here, i.e. ',' can override and disable this setting.
    # Optional. No default.
    pr-team-reviewers: ''
    # A comma-separated list of the assignees (usernames) for the pull request.
    # If non-empty value is specified, the value here will be used and override the
    # assignees property value in pr-template-uri.
    # Giving a single comma as the value here, i.e. ',' can override and disable this setting.
    # Only users with push access can add assignees to an issue. Assignees are silently ignored otherwise.
    # Optional. No default.
    pr-assignees: ''
    # A comma-separated list of the labels for the pull request.
    # If pr-template-uri is also used, this parameter will override the 'labels' field in the template.
    # Giving a single comma as the value here, i.e. ',' can override and disable this setting.
    # Optional. No default.
    pr-labels: ''
```

## Inputs

The following table contains the inputs this action accepts. Please find the description for each in the [action.yml](action.yml).

| Name                | Required | Type     | Default value                        |
|---------------------|----------|----------|--------------------------------------|
| github-token        | Yes      | string   |                                      |
| base-branch         | Yes      | string   |                                      |
| head-branch         | Yes      | string   |                                      |
| pr-fail-if-exist         | No       | string   | false                                 |
| pr-create-draft         | No       | string   | false                                 |
| pr-template-uri         | No       | string   | .github/workflows/templates/version-pr.yml     |
| pr-title              | No       | string   |                                      |
| pr-description      | No       | string   |                                      |
| pr-reviewers           | No       | csv (*1)   |                                      |
| pr-team-reviewers           | No       | csv    |                                      |
| pr-assignees           | No       | csv    |                                      |
| pr-labels           | No       | csv    |                                      |

note:

*1: csv stands for comma separated values

## Outputs

The following table contains the outputs of this action.

| Name                | Description                        |
|---------------------|--------------------------------------|
| base-branch         | The same value as the input.         |
| base-version        | The version extracted from the top level package.json in the base-branch. |
| head-branch         | The same value as the input.         |
| head-version        | The version extracted from the top level package.json in the head-branch. |
| is-prerelease       | A boolean indicator for whether the version is considered as a prerelease or not. |
| is-draft-pr         | A boolean indicator for whether it is a draft pull request or actual one. |
| pull-request-number         | The new pull request number. |
| pull-request-url         | The new pull request url. |
| assignees         | The comma-separated list of assignees to the new pull request. |
| reviewers         | The comma-separated list of reviewers to the new pull request. |
| team-reviewers         | The comma-separated list of team-reviewers to the new pull request. |
| labels         | The comma-separated list of labels to the new pull request. |

## PR template

The PR template is a yaml file that stores pre-configurations for the pull request and can be referenced in a handy way. It requires the template yaml file to be located in the same project where the GitHub workflow uses this action.

### Template structure

```yaml
pull-request: # the node for the pull request pre-configurations
  title: # the title of the pull request
  description: # the description of the pull request
  assignees: # array of GitHub login to be added as assignees
  reviewers: # array of GitHub login to be added as reviewers
  team-reviewers: # array of GitHub login to be added as team reviewers
  labels: # array of string literal to be added as labels. Each string can include spaces.
```

### Template place holders

Some place holders are available for the content placement in the pull request title and description.

| Symbol                | Description                 | Examples             |
|-----------------------|-------------------------|---------------------|
| ${base-branch}        | name of the base branch | main |
| ${base-version}        | version number extracted from the package.json in the top level directory of the base branch | 1.0.0 |
| ${head-branch}        | name of the head branch | rel_1.0.1 |
| ${head-version}        | version number extracted from the package.json in the top level directory of the head branch | 1.0.1 |
| ${is-prerelease}        | boolean indicator for whether the ${head-version} is a semver prerelease format or not | true, false |
| ${is-draft-pr}        | boolean indicator for whether the pull request should be a draft or not | true, false |

## Input precedences

There are precedences for the input values to use in pull request title, description, assignees, reviewers, and labels. Precedences are listed in the ascending order. The table below uses *title* as an example, and these precedences apply to title, description, assignees, reviewers, and labels.

| No. | Condition | Value to use |
|-----|-----------|--------------|
| 1    | action input *pr-title* is non-empty | action input value *pr-title* |
| 2    | *pr-title* is empty, action input *pr-template-uri* is non-empty | node value *pull-request.title* |
| 3    | both action input *pr-title* and *pr-template-uri* are empty | '' (empty string) |
| 4    | condition 2 met, but action input *pr-template-uri* isn't a valid pr-template yaml file | look for node value *pull-request.title* in the *.github/workflows/templates/version-pr.yml* |
| 5    | condition 4 met but *.github/workflows/templates/version-pr.yml* doesn't exist | '' (empty string) |

In some situations, even though the `assignees`, `reviewers`, or `labels` have been set in the pre-configuratioin template, it can still be able to unset them by providing `,` (a single comma) as the value for their corresponding action input.

However, if `title` or `description` is set in the pre-configuration template, they cannot be reset to '' (emtpy string).
## Support

Fortinet-provided scripts in this and other GitHub projects do not fall under the regular Fortinet technical support scope and are not supported by FortiCare Support Services.
For direct issues, please refer to the [Issues](https://github.com/fortinet/github-action-version-pr/issues) tab of this GitHub project.
For other questions related to this project, contact [github@fortinet.com](mailto:github@fortinet.com).

## License

[License](./LICENSE) © Fortinet Technologies. All rights reserved.
