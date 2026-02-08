# GPU Configuration for Cloudeka DekaGPU

## Table of Contents

1. [When to Use GPU Config](#when-to-use-gpu-config)
2. [Deployment Additions](#deployment-additions)
3. [GPU Affinity Patterns](#gpu-affinity-patterns)
4. [Tolerations](#tolerations)
5. [GPU Resource Sizing](#gpu-resource-sizing)
6. [Shared Memory](#shared-memory)
7. [values.yaml GPU Additions](#valuesyaml-gpu-additions)

---

## When to Use GPU Config

Apply GPU configuration when the workload requires:
- Machine learning inference (vLLM, TGI, Triton, etc.)
- Model training or fine-tuning
- Any CUDA/GPU-accelerated workload
- User explicitly mentions GPU, ML, AI, or LLM

## Deployment Additions

Add these to the Deployment spec when GPU is needed:

### runtimeClassName

Add at `spec.template.spec` level:

```yaml
spec:
  template:
    spec:
      runtimeClassName: nvidia
```

### GPU Resources

Add `nvidia.com/gpu` to both limits and requests, and add ephemeral-storage:

```yaml
resources:
  limits:
    cpu: {{ .Values.resources.limits.cpu }}
    memory: {{ .Values.resources.limits.memory }}
    nvidia.com/gpu: {{ index .Values.resources.limits "nvidia.com/gpu" }}
    ephemeral-storage: {{ .Values.resources.limits.ephemeralStorage | default "25Gi" }}
  requests:
    cpu: {{ .Values.resources.requests.cpu }}
    memory: {{ .Values.resources.requests.memory }}
    nvidia.com/gpu: {{ index .Values.resources.requests "nvidia.com/gpu" }}
    ephemeral-storage: {{ .Values.resources.requests.ephemeralStorage | default "25Gi" }}
```

Note: Use `index .Values.resources.limits "nvidia.com/gpu"` because the key contains a dot.

### Shared Memory Volume

Required for PyTorch/CUDA multiprocessing (tensor parallelism, etc.):

```yaml
volumes:
  - name: shm
    emptyDir:
      medium: Memory
      sizeLimit: "2Gi"

# In volumeMounts:
volumeMounts:
  - name: shm
    mountPath: /dev/shm
```

## GPU Affinity Patterns

### Available GPU Types on Cloudeka

| GPU | Label Value | VRAM |
|-----|-------------|------|
| NVIDIA L40S | `NVIDIA-L40S` | 48GB |
| NVIDIA H100 | `NVIDIA-H100-80GB-HBM3` | 80GB |

### Node Affinity for GPU Selection

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: nvidia.com/gpu.product
          operator: In
          values:
          - NVIDIA-L40S  # or NVIDIA-H100-80GB-HBM3
```

### Optional: Target Specific Nodes

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: nvidia.com/gpu.product
          operator: In
          values:
          - NVIDIA-L40S
        - key: kubernetes.io/hostname
          operator: In
          values:
          - node-hostname-1
          - node-hostname-2
```

## Tolerations

GPU nodes on Cloudeka have the `dekallm` taint. Always include:

```yaml
tolerations:
- key: dekallm
  operator: Equal
  value: "true"
  effect: NoSchedule
```

## GPU Resource Sizing

Common sizing patterns:

| Workload | GPU Count | CPU | Memory | Notes |
|----------|-----------|-----|--------|-------|
| Small inference (7B model) | 1 | 4-8 | 16-32Gi | Single GPU |
| Medium inference (30B model FP8) | 2 | 8-16 | 32-64Gi | tensor-parallel-size=2 |
| Medium inference (30B model FP16) | 4 | 8-16 | 32-64Gi | tensor-parallel-size=4 |
| Large inference (70B+ model) | 8 | 8-16 | 32-64Gi | tensor-parallel-size=8 |
| Training/Fine-tuning | 1-8 | 8-32 | 32-128Gi | Depends on model size |
| General CUDA workload | 1 | 4-8 | 16-32Gi | Adjust per workload |

## values.yaml GPU Additions

Additional values for GPU workloads (merge with base values):

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: nvidia.com/gpu.product
          operator: In
          values:
          - NVIDIA-L40S

tolerations:
- key: dekallm
  operator: Equal
  value: "true"
  effect: NoSchedule

resources:
  limits:
    cpu: "16"
    memory: "64Gi"
    nvidia.com/gpu: "1"
    ephemeralStorage: "25Gi"
  requests:
    cpu: "8"
    memory: "32Gi"
    nvidia.com/gpu: "1"
    ephemeralStorage: "25Gi"

# Add shared memory volume
volumes:
  - name: shm
    emptyDir:
      medium: Memory
      sizeLimit: "2Gi"

volumeMounts:
  - name: shm
    mountPath: /dev/shm
```

## Cloudeka Registry for GPU Images

Common GPU image base paths:
```
dekaregistry.cloudeka.id/cloudeka-system/vllm-openai:v0.11.2
```

For custom GPU images, push to dekaregistry.cloudeka.id with appropriate namespace.
