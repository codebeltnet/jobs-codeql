# Reusable Workflows for GitHub CodeQL

This repository contains reusable workflows for integrating GitHub CodqQL into your CI/CD pipeline.

> These workflows is part of the Codebelt umbrella and ensures a consistent way of: 
> 
> - Defining your CI/CD pipeline 
> - Structuring your repository
> - Keeping your codebase small and feasible
> - Writing clean and maintainable code
> - Deploying your code to different environments
> - Automating as much as possible
>
> A paved path to excel as a DevSecOps Engineer.

## Available Workflows

- [default.yml](.github/workflows/default.yml) - the default workflow that:
  - [fetches the codebase](https://github.com/codebeltnet/git-checkout),
  - [installs the .NET SDK](https://github.com/codebeltnet/install-dotnet),
  - [restores the dependencies](https://github.com/codebeltnet/dotnet-restore),
  - [runs the GitHub CodeQL analysis](https://github.com/codebeltnet/codeql-scan),
  - [builds the solution](https://github.com/codebeltnet/dotnet-build),
  - [finalizes the GitHub CodeQL analysis](https://github.com/codebeltnet/codeql-scan-finalize).

### Usage

To call this workflow in your GitHub repository, you can follow these steps:

```yaml
codeql-call:
    uses: codebeltnet/jobs-codeql/.github/workflows/default.yml@v3
```

### Inputs

```yaml
with:
  # Optional path to the project(s) file to build. Pass empty to have MSBuild use the default behavior. Supports globbing. Default is an empty string.
  projects:
  # Optional checkout branch, tag or SHA. Omit to retain the triggering ref.
  ref: ''
  # Optional reporting identity, independent of checkout. Set both or omit both.
  analysis-ref: ''
  analysis-sha: ''
  # Build configuration. Defaults to Debug for existing callers.
  configuration: Debug
  # When set to true, includes preview versions of .NET. Default is false.
  include-preview: false
  # The maximum time in minutes to allow the job to run. Default is 15 minutes.
  timeout-minutes: 15
```

For post-release assurance, set `ref` and `analysis-sha` to the exact released SHA, `analysis-ref: refs/heads/main`, and `configuration: Release`. The checkout `ref` selects source; `analysis-ref` and `analysis-sha` are forwarded as `ref` and `sha` to `codeql-scan-finalize`, then to the official analyze action. Existing callers can omit these inputs to retain their triggering checkout, automatic reporting identity and Debug build. The upstream analyze action requires both reporting inputs when overriding identity.

### Secrets

This workflow has no secrets.

### Outputs

This workflow has no outputs.

### Example

```yaml
jobs:
  sonarcloud:
    name: call-codeql
    needs: [build,test]
    uses: codebeltnet/jobs-codeql/.github/workflows/default.yml@v3
    with:
      include-preview: true
```

## Contributing to Reusable Workflows for SonarQube Cloud

Contributions are welcome! 
Feel free to submit issues, feature requests, or pull requests to help improve these workflows.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
