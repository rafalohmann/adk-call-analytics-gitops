# AGENT.md – adk-call-analytics-gitops

Guidance for AI agents working on this GitOps repo.

## Core principles

- **Declarative everything**: This repo defines the desired state of environments.
- **Git as the source of truth**: Argo CD reconciles cluster state to match this repo.
- **Environment separation**: Avoid cross-contamination between `dev`, `stage`, `prod` overlays.

## Editing rules

- Prefer small, focused changes to values or Application definitions.
- Do not store raw secrets; use SealedSecrets or SOPS-encrypted files and reference them here.
- For image updates:
  - Prefer script or CI automation that updates only the image tag while keeping configs intact.

## Safety

- For `stage` and `prod`, changes should go through PRs with human review.
- Avoid using `latest` image tags; use immutable tags (commit SHA or version).
- When in doubt, document assumptions in commit messages or PR descriptions.

