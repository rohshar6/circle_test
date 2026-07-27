# CircleCI PR Workflow 



This repository contains a basic CircleCI configuration with workflows that run on every build and another workflow that runs pull request checks only for GitHub merge queue builds.

## CircleCI Configuration

The workflow is defined in `.circleci/config.yml`. 

It includes:

- An `always-run` workflow that runs for every CircleCI build.
- A `test-queue` workflow that runs for every CircleCI build.
- A `pull-request` workflow for merge queue PR checks before merge.
- An `always-run` job that prints basic branch and commit output.
- A `test-queue` job that prints basic branch and commit output.
- A `pr-checks` job using the `cimg/base:stable` Docker image.
- A workflow condition that only runs PR checks on branches starting with `gh-readonly-queue/`.
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

## Merge Queue Trigger

In CircleCI project settings, configure the GitHub trigger option as `Pushes to merge queues`.

CircleCI runs that trigger for pushes to branches that start with `gh-readonly-queue/`. The `pull-request` workflow also checks that branch prefix in `.circleci/config.yml`.

## Notes

CircleCI merge queue trigger behavior depends on using a GitHub App pipeline trigger. The `always-run` and `test-queue` workflows have no condition, so they run for every pipeline.
