# adk-call-analytics-gitops

GitOps “environments” repository for the Call Analytics project.

Argo CD points to this repo to determine **what runs where** across environments.

## Responsibilities

- Define Argo CD `Application` (and optionally `ApplicationSet`) resources.
- Provide per-environment Kustomize overlays and Helm values for:
  - `adk-call-analytics-api`
  - `adk-call-analytics-worker`
  - `adk-call-analytics-frontend`
  - Postgres + pgvector
  - RabbitMQ (or Kafka) and other core dependencies
- Control image tags, scaling, and configuration per environment.

## Structure

- `argo-apps/`: Argo CD Application definitions for platform and apps.
- `envs/`: Environment overlays:
  - `dev/`, `stage/`, `prod/` with their own app values and namespaces.

## Workflow

- CI in application repos updates the image tags here (e.g. in `envs/dev/apps/app-api/values.yaml`).
- Changes to `stage`/`prod` are made via pull requests for review and approval.

