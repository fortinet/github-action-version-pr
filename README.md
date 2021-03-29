# (GitHub Action) Version PR

[![GitHub Release](https://img.shields.io/github/package-json/v/fortinet/github-action-version-pr)]()
[![Node version](https://img.shields.io/badge/node-12.x-brightgreen.svg?style=flat)]()

A GitHub Action that creates a pull request for versioning.

# Usage

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

# Support

Fortinet-provided scripts in this and other GitHub projects do not fall under the regular Fortinet technical support scope and are not supported by FortiCare Support Services.
For direct issues, please refer to the [Issues](https://github.com/fortinet/github-action-version-pr/issues) tab of this GitHub project.
For other questions related to this project, contact [github@fortinet.com](mailto:github@fortinet.com).

# License

[License](./LICENSE) © Fortinet Technologies. All rights reserved.
