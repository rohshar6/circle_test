# CircleCI PR Workflow 



This repository contains a basic CircleCI configuration with workflows that run on every build and a PR-check workflow that runs only for GitHub merge queue builds.

## CircleCI Configuration

The workflow is defined in `.circleci/config.yml`.

It includes:

- An `always-run` workflow that runs for every CircleCI build.
- A `test-queue` workflow that runs for every CircleCI build.
- A `pull-request` workflow for merge queue PR checks before merge.
- An `always-run` job that prints basic branch and commit output.
- A `test-queue` job that prints basic branch and commit output.
- A `pr-checks` job using the `cimg/base:stable` Docker image.
- A workflow condition that runs PR checks only on branches starting with `gh-readonly-queue/`.
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

The YAML condition controls which workflow runs after CircleCI receives a pipeline. It does not create the GitHub event trigger.

In CircleCI:

1. Open **Project Settings**.
2. Open **Project Setup**.
3. Add a GitHub trigger to the pipeline.
4. Select **Pushes to merge queues** from the **Run on** menu.
5. Save the trigger.

CircleCI documents this trigger as running for pushes to branches prefixed with `gh-readonly-queue/`. The `pull-request` workflow checks the same prefix:

```yaml
when: pipeline.git.branch starts-with "gh-readonly-queue/"
```

GitHub must also have a merge queue enabled for the protected target branch. Add the PR to the merge queue instead of merging it directly. A normal merge into `main` does not create a `gh-readonly-queue/` pipeline, so `pr-checks` will not appear on that merge commit.

Do not select **PR merged** for this requirement. That trigger starts after the merge has already completed.

CircleCI documentation:

- [Trigger options](https://circleci.com/docs/guides/orchestrate/triggers-overview/)
- [GitHub trigger event options](https://circleci.com/docs/guides/orchestrate/github-trigger-event-options/)
- [Configuration reference](https://circleci.com/docs/reference/configuration-reference/)

## Notes

CircleCI merge queue trigger behavior depends on a GitHub App pipeline trigger. The `always-run` and `test-queue` workflows have no condition, so they run for every pipeline, including merge queue pipelines.
