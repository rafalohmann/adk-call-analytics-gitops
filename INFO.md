# INFO – adk-call-analytics-gitops

- **Repo ID**: adk-call-analytics-gitops
- **Full name**: Call Analytics – GitOps Environments
- **Owner**: Personal / rafaellohmann
- **Description**: GitOps repository defining the desired state of Call Analytics environments (dev/stage/prod) using Argo CD, Helm, and Kustomize.

## Relationships

- Consumed by:
  - Argo CD installation defined in `adk-call-analytics-platform`
- References:
  - Helm charts and addons from `adk-call-analytics-platform`
  - Container images built by:
    - `adk-call-analytics-api`
    - `adk-call-analytics-worker`
    - `adk-call-analytics-frontend`

## Promotion model

- `dev` may track frequent updates (e.g. direct CI commits).
- `stage` and `prod` use pull-request-based promotion and manual approvals.
- Rollbacks are performed by reverting or adjusting commits in this repo.

