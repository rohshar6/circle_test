# CircleCI PR Workflow 

This repository contains a basic CircleCI configuration with one workflow that runs on every build and another workflow for pull request checks.

## CircleCI Configuration

The workflow is defined in `.circleci/config.yml`.

It includes:

- An `always-run` workflow that runs for every CircleCI build.
- A `pull-request` workflow.
- An `always-run` job that prints basic branch and commit output.
- A `pr-checks` job using the `cimg/base:stable` Docker image.
- A guard that confirms the build has pull request metadata.
- Branch filters that ignore direct builds on `main` and `master`.
- A placeholder checks step that can be replaced with project-specific test or build commands.

## Updating Checks

Replace the placeholder command in the `Run checks` step with the commands needed for this project.

For example:

```yaml
- run:
    name: Run checks
    command: |
      npm install
      npm test
```

## Notes

CircleCI pull request behavior can depend on the VCS integration and project settings. This config uses CircleCI pull request environment variables to ensure the job only proceeds for pull request builds. 