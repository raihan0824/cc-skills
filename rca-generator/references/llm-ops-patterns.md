# LLM Ops Diagnostic Patterns

Common issues and diagnostic patterns for LLM-as-a-Service platforms using LiteLLM, vLLM, and SGLang.

## Table of Contents
- [vLLM Issues](#vllm-issues)
- [LiteLLM Issues](#litellm-issues)
- [SGLang Issues](#sglang-issues)
- [Key Metrics to Analyze](#key-metrics-to-analyze)

---

## vLLM Issues

### High Latency / Low Throughput

**Symptoms**: TTFT > expected, tokens/sec degradation, request queuing

**Common Causes**:
| Cause | Diagnostic | Solution |
|-------|-----------|----------|
| KV cache exhaustion | `gpu_cache_usage_perc` near 100% | Reduce `max_num_seqs`, increase `gpu_memory_utilization` |
| Insufficient batch size | Low `num_requests_running` vs `num_requests_waiting` | Tune `max_num_seqs` |
| CPU offloading active | Check swap space metrics | Add GPU memory or reduce model size |
| Prefill bottleneck | High TTFT, normal ITL | Enable chunked prefill |

### OOM Errors

**Symptoms**: CUDA out of memory, worker crashes

**Common Causes**:
- `gpu_memory_utilization` set too high (>0.95)
- `max_model_len` exceeds available memory
- Concurrent long-context requests

**Diagnostics**:
```bash
# Check GPU memory
nvidia-smi --query-gpu=memory.used,memory.total --format=csv
```

### Request Timeouts

**Symptoms**: 504 errors, client timeouts

**Common Causes**:
- Queue depth too high (`num_requests_waiting` spike)
- Model loading issues
- Health check failures

---

## LiteLLM Issues

### Proxy Errors (5xx)

**Symptoms**: 500/502/503/504 responses from LiteLLM proxy

| Error | Likely Cause | Check |
|-------|-------------|-------|
| 500 | Backend model error | Underlying vLLM/model logs |
| 502 | Backend unreachable | Network, model endpoint health |
| 503 | Rate limiting / overload | `max_parallel_requests` config |
| 504 | Backend timeout | `timeout` setting, model latency |

### Routing Issues

**Symptoms**: Requests not reaching expected model, load imbalance

**Common Causes**:
- Misconfigured `model_list` in config
- Health check failures marking backends unhealthy
- Router strategy not optimal for workload

### Authentication Failures

**Symptoms**: 401/403 errors

**Check**:
- API key configuration
- Virtual key validity
- Team/user permissions in LiteLLM DB

---

## SGLang Issues

### Runtime Errors

**Symptoms**: Server crashes, inference failures

**Common Causes**:
- Incompatible model format
- Tokenizer issues
- Memory allocation failures

### Performance Degradation

**Symptoms**: Lower throughput than expected

**Check**:
- RadixAttention cache hit rate
- Batch scheduling efficiency
- Tensor parallelism configuration

---

## Key Metrics to Analyze

### Latency Metrics
| Metric | Description | Alert Threshold |
|--------|-------------|-----------------|
| TTFT (Time to First Token) | Prefill latency | > 500ms for short prompts |
| ITL (Inter-Token Latency) | Decode latency per token | > 50ms |
| E2E Latency | Total request time | Context-dependent |

### Throughput Metrics
| Metric | Description |
|--------|-------------|
| Tokens/sec | Output token generation rate |
| Requests/sec | Request completion rate |
| Queue depth | `num_requests_waiting` |

### Resource Metrics
| Metric | Description | Alert Threshold |
|--------|-------------|-----------------|
| GPU Memory Usage | VRAM utilization | > 95% |
| GPU Utilization | Compute usage | < 50% (underutilized) |
| KV Cache Usage | `gpu_cache_usage_perc` | > 90% |

### Error Metrics
| Metric | Description |
|--------|-------------|
| Error rate | 4xx/5xx responses / total |
| Timeout rate | Timed out / total requests |
| OOM count | CUDA OOM occurrences |

---

## Documentation References

When analyzing issues, fetch relevant documentation:

- **vLLM**: `https://docs.vllm.ai/en/stable/`
- **LiteLLM**: `https://docs.litellm.ai/docs/`
- **SGLang**: `https://sgl-project.github.io/`

Use WebFetch to retrieve specific docs when deeper investigation is needed.
