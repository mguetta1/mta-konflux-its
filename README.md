# mta-konflux-its

A catalog of [Tekton](https://tekton.dev/) pipelines and tasks used as
[Konflux](https://konflux-ci.dev/) `IntegrationTestScenario` (ITS) pipelines for the MTA tenant.

## Repository structure

```
pipelines/    # integration-test pipelines referenced by IntegrationTestScenarios
tasks/        # reusable Tekton tasks referenced by the pipelines
README.md
```

## How it's used in Konflux

An `IntegrationTestScenario` references a pipeline from this repo via the Tekton
**git resolver** — an `url` + `revision` + `pathInRepo` triple:

```yaml
apiVersion: appstudio.redhat.com/v1beta2
kind: IntegrationTestScenario
spec:
  resolverRef:
    resolver: git
    resourceKind: pipeline
    params:
      - name: url
        value: https://github.com/migtools/mta-konflux-its
      - name: revision
        value: main   # branch name, tag, or commit SHA
      - name: pathInRepo
        value: pipelines/<pipeline-file>.yaml
```

Because files are addressed by exact `pathInRepo`, the folder layout above is a convention for
readability, not a resolver requirement. Pipelines in `pipelines/` resolve their tasks from
`tasks/` in this same repository.
