# gh-action-workflows

Reusable GitHub Actions workflows for CI/CD pipelines at ZeitOnline.

## Overview

The workflows provided here are intended for two main purposes, (continuously) deploying projects and creating releases for them as well as building and deploying so called "nightwatch" tests that are meant to help with monitoring the health/status of said projects.

The first use-case uses a number of them as building bricks and for a project-specific CI/CD workflow that typically looks like this:

``` yaml title=".github/workflows/backend-ci.yaml"
name: Backend CI
on:
  workflow_dispatch:
  push:
    branches:
      - 'main'
concurrency:
  group: backend
  cancel-in-progress: true
jobs:
  linter:
    uses: zeitonline/gh-action-workflows/.github/workflows/lefthook.yaml
  k8s:
    uses: zeitonline/gh-action-workflows/.github/workflows/k8s-validation.yaml
  # run 'release-please' first
  release-please:
    needs: [linter, k8s]
    uses: zeitonline/gh-action-workflows/.github/workflows/release-please.yaml
    secrets: inherit
  # without release: build new images for 'staging'
  build:
    needs: release-please
    if: ${{ ! needs.release-please.outputs.release_created }}
    uses: zeitonline/gh-action-workflows/.github/workflows/build-test-push.yaml
    secrets: inherit
    with:
      targets: postgrest migrator
      versions: k8s/staging/versions
  # with release: promote images to 'production' and notify
  promote-images:
    needs: release-please
    if: needs.release-please.outputs.release_created
    uses: zeitonline/gh-action-workflows/.github/workflows/add-tag.yaml
    secrets: inherit
    with:
      tag: ${{ needs.release-please.outputs.tag_name }}
  notify:
    needs: release-please
    if: needs.release-please.outputs.release_created
    uses: zeitonline/gh-action-workflows/.github/workflows/release-notification.yaml
    secrets: inherit
    with:
      environment: production
      version: ${{ needs.release-please.outputs.tag_name }}
      emoji: tada
```

The second just uses [nightwatch-build](#nightwatch-build) to build, test and push updated "nightwatch" images. It is typically used like this:

``` yaml title=".github/workflows/nightwatch-build.yaml"
name: Build nightwatch tests
on:
  workflow_dispatch:
  push:
    paths:
      - 'smoketest/**'
jobs:
  build:
    uses: zeitonline/gh-action-workflows/.github/workflows/nightwatch-build.yaml
    secrets: inherit
    with:
      versions: k8s/base/nightwatch/versions
      gke_cluster: main-staging-25-01
```

## Workflows

- [**build-test-push**](#build-test-push) — Build, test and push Docker images for staging deployments
- [**add-tag**](#add-tag) — Re-tag existing images in the registry for production deployments
- [**release-please**](#release-please) — Manage releases via [Release Please](https://github.com/googleapis/release-please)
- [**commit-lint**](#commit-lint) — Lint commit messages using [commitlint](https://commitlint.js.org/)
- [**lefthook**](#lefthook) — Run pre-commit code checks via [Lefthook](https://github.com/evilmartians/lefthook)
- [**pre-commit**](#pre-commit) — Run code checks via [pre-commit](https://pre-commit.com/)
- [**k8s-validation**](#k8s-validation) — Validate Kubernetes manifests using [kubeval](https://www.kubeval.com/)
- [**k8s-tests**](#k8s-tests) — Run the `pytest`-based assertions over the generated Kubernetes manifests
- [**k8s-policy-tests**](#k8s-policy-tests) — Run the generic `zeit.k8stesting` policy suite without any project-side test files
- [**nightwatch-build**](#nightwatch-build) — Build, push and run Nightwatch smoke test images
- [**release-notification**](#release-notification) — Notify Slack and Prometheus about deployments

---

### build-test-push

Runs tests inside Docker Compose, publishes test results, then builds each target, scans it with the Sysdig CLI scanner (policy `sysdig-best-practices`) and pushes the resulting multi-target Docker images to Google Artifact Registry. When a `versions` directory is provided, it also updates the Kustomize image tags and commits them back — intended for automatic staging deployments.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `service` | no | `backend` | Compose service to build and test |
| `project` | no | repo name | GCP project name |
| `targets` | no | — | Space-separated list of Docker build targets to publish |
| `versions` | no | — | Kustomize directory for updating image tags (staging) |
| `compose_file` | no | `testing.yaml` | Docker Compose file to use |
| `run_args` | no | — | Additional Arguments for `docker compose run` |
| `test_args` | no | `-- --verbose --junitxml=…` | Arguments passed to the test runner |
| `artifacts` | no | — | Path to extract build artifacts from the container |
| `build_args` | no | `.` | Additional arguments for `docker build` |

---

### add-tag

Adds an additional tag (typically the release version) to images that already exist in Google Artifact Registry. This enables production deployments by re-using the exact same images that were previously built and tested for staging — without rebuilding them.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `project` | no | repo name | GCP project name |
| `tag` | **yes** | — | Tag to add (e.g. the release version) |
| `versions` | no | `k8s/staging/versions/kustomization.yaml` | Kustomization file listing image names and their current tags |

---

### release-please

Runs [Release Please](https://github.com/googleapis/release-please) to manage versioned releases via conventional commits. Can be used both as a reusable workflow (via `workflow_call`) and standalone (triggered on push to `main` or via `workflow_dispatch` to create releases for this repo itself). Outputs whether a release was created and the tag name, so callers can decide between a staging build or a production re-tag.

#### Outputs

| Name | Description |
|------|-------------|
| `release_created` | `true` if a release was created |
| `tag_name` | The release version tag (if created) |

---

### commit-lint

Validates commit messages against [conventional commit](https://www.conventionalcommits.org/) rules using `commitlint`.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `configFile` | no | `commitlint.config.mjs` | Path to commitlint config |
| `failOnWarnings` | no | `false` | Fail on warnings |
| `failOnErrors` | no | `true` | Fail on errors |
| `helpURL` | no | commitlint docs | Link shown on failure |
| `commitDepth` | no | — | Only check the latest X commits |

---

### lefthook

Runs code quality checks using [Lefthook](https://github.com/evilmartians/lefthook) (as a replacement for the [pre-commit](#pre-commit) workflow below). Sets up `uv` and optionally Python and/or Node.js, then executes `lefthook run pre-commit --all-files`.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `python-version` | no | — | Python version to install (if needed). The special value `docker` extracts the version from the `FROM python:…` line of the `Dockerfile` |
| `node-version` | no | — | Node.js version to install (if needed) |
| `setup-command` | no | `uv sync --frozen --only-group lint` | Command to install linting tools |
| `baseproject-name` | no | — | Project name passed to `gh-action-baseproject`. When set, authenticates to Google Cloud so linting tools can be installed from the private `pypi-zon` index |
| `baseproject-environment` | no | `devel` | Environment passed to `gh-action-baseproject` |

---

### pre-commit

Runs [pre-commit](https://pre-commit.com/) hooks via `uvx`. Optionally sets up Python and/or Node.js runtimes for hooks that need them.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `python-version` | no | — | Python version to install |
| `node-version` | no | — | Node.js version to install |

---

### k8s-validation

Validates Kubernetes manifests by running `kubectl kustomize` for each environment and checking the output with [kubeval](https://www.kubeval.com/).

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `environments` | no | `staging production` | Space-separated list of environments to validate (expects `k8s/<env>` directories) |

---

### k8s-tests

Runs the `pytest`-based test suite that makes assertions about the generated Kubernetes
manifests (rendered via `kustomize`). The suite lives in its own directory with its own
`pyproject.toml` and typically builds on the [`zeit.k8stesting`](https://github.com/ZeitOnline/zeit.k8stesting)
plugin, which contributes the shared policy tests. This complements [k8s-validation](#k8s-validation)
(schema validation): validation checks that the manifests are *valid*, these tests check
that they are *correct* for our conventions (variant `-pgXX` suffixes, placeholder
replacement, PGSERVICE routing, …).

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `working-directory` | no | `k8s/tests` | Directory containing the pytest suite (with its own `pyproject.toml`) |
| `uv-version` | no | `0.11.31` | Version of `uv` to install |
| `project` | no | — | GCP project name. When set, authenticates to Google Cloud and exposes the private `pypi-zon` index credentials to `uv` (needed once the test dependencies live on the private index). Leave empty for suites that only use public dependencies. |

#### Example

``` yaml
jobs:
  k8s-tests:
    uses: zeitonline/gh-action-workflows/.github/workflows/k8s-tests.yaml
    with:
      project: premium-services
```

---

### k8s-policy-tests

Runs the **generic** [`zeit.k8stesting`](https://github.com/ZeitOnline/zeit.k8stesting) policy
suite against a project's rendered manifests **without any project-side test files**. The job
materialises an ephemeral `k8s/tests/` from the inputs below and runs it — ideal for the many
simple projects that only need the shared policy (A1–A8) and no custom `kustomize_jq` tests.
Projects that *do* need custom assertions keep their own suite and use [k8s-tests](#k8s-tests)
instead (this workflow fails fast if a `k8s/tests/` already exists).

The inputs mirror `zeit.k8stesting`'s `K8sPolicy`. Default posture: universal checks on,
variant-versioning off — placeholder-leaks (A1), explicit replicas (A6) and readiness probes
(A7) run; the `-pgXX` variant-versioning check (A2) is off by default; A3/A4/A5/A8 are opt-in
(enabled by providing their inputs).

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `project` | **yes** | — | GCP/baseproject identity of the calling repo (authenticates to GCP and exposes the private `pypi-zon` credentials to `uv`) |
| `environment` | no | `production` | baseproject environment |
| `environments` | no | `devel staging production` | Space-separated kustomize environments to parametrize over |
| `overlays_dir` | no | `k8s/overlays` | Directory holding one sub-directory per environment |
| `switched_services` | no | — | A4: space-separated services whose selector must point at the active variant |
| `active_variant` | no | — | A4: JSON map environment → active variant, e.g. `{"production":"pg17"}` |
| `service_selector_key` | no | `app.kubernetes.io/instance` | A4: label key a Service selects on |
| `version_exempt` | no | — | A2: space-separated workloads allowed to carry no `-pgXX` suffix |
| `forbidden_bare_resources` | no | — | A3: JSON list of `["Kind","name"]` that must never appear un-versioned |
| `placeholder_tokens` | no | *(built-in set)* | A1: space-separated forbidden placeholder tokens |
| `alert_min` | no | — | A8: JSON map environment → minimum `Alert` count, e.g. `{"production":1}` |
| `require_readiness_probes` | no | `true` | A7 on/off |
| `require_explicit_replicas` | no | `true` | A6 on/off |
| `require_variant_versioning` | no | `false` | A2 on/off (enable for multi-DB variant projects) |
| `uv-version` | no | `0.11.31` | Version of `uv` to install |
| `zeit_k8stesting_version` | no | `>=0.1.0` | Version constraint for the `zeit.k8stesting` package |

#### Example

``` yaml
jobs:
  k8s-policy:
    uses: zeitonline/gh-action-workflows/.github/workflows/k8s-policy-tests.yaml
    secrets: inherit
    with:
      project: my-service
      environments: "staging production"
```

---

### nightwatch-build

Builds a Docker image containing per project "smoke" or "nightwatch" tests, pushes it to GAR, runs a security scan (on PRs), and executes the tests in a Kubernetes pod created via `kustomize`. On `main`, it updates the image tag in Kustomize and commits it back.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `project` | no | repo name | GCP project name |
| `environment` | no | `staging` | Target environment |
| `gke_cluster` | no | `main-staging-25-01` | GKE cluster name (falls back to the environment name when set to an empty string) |
| `kustomization` | no | `k8s/base/nightwatch/test` | Directory containing the Kustomization for the test pod |
| `versions` | no | `k8s/base/nightwatch/versions` | Kustomize directory for updating image tags |
| `k8s_base` | no | `k8s` | Directory containing the Kustomize base with the image name |

#### Outputs

| Name | Description |
|------|-------------|
| `tag` | Timestamp-based tag of the built image |

---

### release-notification

Posts a deployment notification to Slack (via Hackbot) and pushes a deployment timestamp metric to Prometheus (via pushgateway). Both halves are optional: setting `emoji` to an empty string skips the Slack notification, setting `prometheus_job` to an empty string skips the metric.

#### Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `environment` | **yes** | — | Deployment environment |
| `version` | **yes** | — | Deployed version |
| `project` | no | repo name | Project name |
| `changelog` | no | `CHANGELOG.md` | Path to changelog file |
| `emoji` | no | `loudspeaker` | Slack emoji for the notification (set empty to skip the Slack notification) |
| `prometheus_job` | no | `gha-deployments` | Prometheus job name (set empty to skip) |
