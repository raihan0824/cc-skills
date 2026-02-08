---
name: cloudeka-git-cicd
description: Generate GitHub Actions CI/CD workflows for Cloudeka deployments using dekaregistry.cloudeka.id. Use when the user wants to set up CI/CD pipelines, GitHub workflows, automated Docker builds, release automation, or versioning for projects deployed on Cloudeka. Triggers on phrases like "create CI/CD", "set up GitHub Actions", "automate builds", "create a release workflow", "add CI pipeline", "github workflows for cloudeka".
---

# Cloudeka Git CI/CD

Generate GitHub Actions workflows for trunk-based development with Cloudeka's container registry (`dekaregistry.cloudeka.id`).

## Strategy

- **Trunk-based**: `main` is the only long-lived branch
- **Dev builds**: every push to `main` → images tagged `dev-<short-sha>`
- **Stable releases**: push git tag `v*` → images tagged with version + `latest` + GitHub Release
- **PR checks**: typecheck + build on every pull request

## Workflow

1. Discover the project structure: identify services, Dockerfiles, and build tools
2. Ask the user which workflows they need (CI, dev build, release — or all three)
3. Read the appropriate templates from `references/` and adapt them:
   - Replace `APP_NAME` with the actual application/image name
   - Adjust jobs to match the actual services (may not be frontend+backend)
   - Adjust build commands to match the project (npm, yarn, pnpm, go, python, etc.)
   - Add/remove jobs as needed (e.g., single service = single build job)
4. Write workflows to `.github/workflows/`

## Templates

Read these reference templates and adapt to the project:

- [references/ci-template.yml](references/ci-template.yml) — PR quality checks (typecheck + build)
- [references/dev-build-template.yml](references/dev-build-template.yml) — Dev image build on push to main
- [references/release-template.yml](references/release-template.yml) — Stable release on tag push + GitHub Release

## Customization Points

When adapting templates:

- **Registry path**: default `dekaregistry.cloudeka.id/cloudeka-system`. Change `cloudeka-system` to the user's project namespace if different.
- **Services**: templates assume frontend + backend. For single-service apps, use one build job. For 3+ services, add more parallel jobs.
- **Build tool**: templates use Node.js (`npm ci`, `npm run build`). Adapt for Go (`go build`), Python (`pip install`, `python -m build`), etc.
- **CI checks**: templates run typecheck + build. Add lint, test, or other checks as the project supports.
- **Branch name**: default `main`. Change if the project uses `master` or another default branch.

## Required GitHub Secrets

Remind the user to configure these in repo Settings → Secrets and variables → Actions → Repository secrets:

- `REGISTRY_USERNAME` — dekaregistry.cloudeka.id login
- `REGISTRY_PASSWORD` — dekaregistry.cloudeka.id password

## Usage Reference

```bash
# Dev builds happen automatically on push to main

# Create a stable release:
git tag -a v1.0.0 -m "Release description"
git push origin v1.0.0
```
