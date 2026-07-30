# CircleCI PR Workflow 



This repository contains a basic CircleCI configuration with two workflows that run for every pipeline and a conditional PR and tag workflow.

## CircleCI Configuration

The workflow is defined in `.circleci/config.yml`. 

It includes:

- An `always-run` workflow that runs for every CircleCI build.
- A `test-queue` workflow that runs for every CircleCI build.
- A `test-and-validate` workflow that runs when a PR is opened, a draft PR is marked ready for review, new commits are pushed to an open non-draft PR, or the `run-ci` label is added to a PR.
- An `always-run` job that prints basic branch and commit output.
- A `test-queue` job that prints basic branch and commit output.
- A `run-tests` job using the `cimg/base:stable` Docker image.
- A placeholder checks step that can be replaced with project-specific test or build commands.

## Updating Checks

Replace the placeholder command in the `Show workflow output` step with the commands needed for this project.

For example:

```yaml
- run:
    name: Run checks
    command: |
      npm install
      npm test
```

## Notes

Configure four separate GitHub triggers in **CircleCI Project Settings > Project Setup**:

- **PR opened**
- **PR marked ready for review**
- **Pushes to open non-draft PRs**
- **"run-ci" label added to PR**

CircleCI does not combine these options into one trigger. The workflow condition rejects other event actions and labels.
