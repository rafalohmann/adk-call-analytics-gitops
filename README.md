# adk-call-analytics-gitops

GitOps "environments" repository for the Call Analytics project. Argo CD points to this repo to determine **what runs where** across environments.

## Structure

- **argo-apps/apps/**: Argo CD `Application` manifests for dev (api, worker, frontend, postgres, rabbitmq). Each Application references the platform repo for Helm charts and inlines env-specific values in `spec.source.helm.valuesObject`.
- **argo-apps/dev/app-of-apps.yaml**: Optional root Application that syncs the directory `argo-apps/apps` so all dev apps are deployed when Argo CD applies it.
- **envs/dev/**: Kustomize for the `dev` namespace (namespace.yaml, kustomization.yaml). Apply separately or let Argo CD create the namespace when syncing apps.

## Adding or changing an app

1. Edit the corresponding file under `argo-apps/apps/` (e.g. `app-api-dev.yaml`).
2. Chart source is the platform repo; override image, env, replicas, and probes via `spec.source.helm.valuesObject`.
3. Commit and push. If sync is automated, Argo CD will apply changes.

## Image tags

Update `spec.source.helm.valuesObject.image.tag` (and optionally `image.repository`) in the relevant Application YAML. Use immutable tags (e.g. commit SHA or semver) for production. CI in app repos can commit tag updates to this repo for dev.

## Promotion

- **dev**: Direct pushes to `main` or PR; automated sync.
- **stage / prod**: Use PR-based changes to the corresponding env paths (e.g. `argo-apps/apps/` for stage) with review and approval. No raw secrets in Git; use placeholders (e.g. `CHANGEME`) and inject real values via External Secrets or `kubectl create secret`.
