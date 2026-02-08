---
name: cloudeka-helm
description: Generate Kubernetes Helm charts tailored for the Cloudeka environment. Use when the user wants to create a Helm chart, scaffold a new Kubernetes deployment, or deploy an application on Cloudeka. Triggers on phrases like "create a helm chart", "scaffold a deployment", "deploy on cloudeka", "kubernetes chart for cloudeka", "helm chart for my app". Handles both general workloads and GPU workloads (ML/AI/LLM inference).
---

# Cloudeka Helm Chart Creator

Generate production-ready Helm charts for the Cloudeka environment with mandatory security constraints and optional GPU support.

## Cloudeka Mandatory Constraints

Every chart MUST include:

```yaml
securityContext:
  runAsUser: 1000
  runAsNonRoot: true
  allowPrivilegeEscalation: false
```

Resources MUST always be defined in `values.yaml`, never hardcoded in templates.

Default storage class: `storage-nvme-c1`.

## Workflow

1. Ask user: app name, container image, ports, and what the app does
2. Determine if GPU is needed (ML/AI/LLM workload = yes)
3. Determine which K8s resources are needed (Deployment is always included)
4. Generate the full chart directory:
   - `Chart.yaml`
   - `templates/_helpers.tpl`
   - `templates/deployment.yaml` (always)
   - `templates/service.yaml` (if networking needed)
   - `templates/pvc.yaml` (if persistent storage needed)
   - `templates/configmap.yaml` (if config data needed)
   - `templates/hpa.yaml` (if autoscaling needed)
   - `templates/ingress.yaml` (if ingress needed)
   - `templates/serviceaccount.yaml` (if service account needed)
   - `values.yaml`
5. Populate `values.yaml` with sensible defaults

## Template Reference

Read [references/base-templates.md](references/base-templates.md) for all Helm template patterns (Deployment, Service, PVC, ConfigMap, HPA, Ingress, ServiceAccount, _helpers.tpl, Chart.yaml, values.yaml structure).

## GPU Workloads

When the workload requires GPU (user mentions ML, AI, LLM, inference, training, CUDA, GPU):

1. Read [references/gpu-config.md](references/gpu-config.md) for GPU-specific configuration
2. Add to deployment: `runtimeClassName: nvidia`, GPU resources, shared memory volume (`/dev/shm`)
3. Add node affinity for GPU type selection (L40S or H100)
4. Add `dekallm` toleration
5. Use higher failureThreshold for probes (30-40) since GPU workloads take longer to initialize

When the workload does NOT require GPU, omit all GPU-specific config (no affinity, no tolerations, no runtimeClassName, no nvidia resources, no /dev/shm).

## Image Registry

- Cloudeka registry: `dekaregistry.cloudeka.id/<namespace>/<image>:<tag>`
- Public registries (Docker Hub, ghcr.io, etc.) are also supported
- Default to Cloudeka registry when user doesn't specify

## Output

Generate all files as actual files in the user's specified directory. After generation, list the created files and briefly explain what each values.yaml section controls.
