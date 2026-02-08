---
name: rca-generator
description: Generate Root Cause Analysis (RCA) documents in markdown format from user-provided resources. Use when users ask to create an RCA, incident report, post-mortem, or analyze service issues. Specialized for LLM Ops (vLLM, LiteLLM, SGLang) but works for any infrastructure incident. Accepts any input format (CSV metrics, log files, PDFs, documents, screenshots, images). Triggers on phrases like "create an RCA", "analyze this incident", "why did this fail", "generate post-mortem", or "what caused this outage".
---

# RCA Generator

Generate structured Root Cause Analysis documents from user-provided incident data.

## Workflow

### 1. Gather Input

Accept any format the user provides:
- **CSV/metrics**: Load test results, performance data, error counts
- **Logs**: Application logs, system logs, vLLM/LiteLLM output
- **Documents**: Incident tickets, runbooks, previous RCAs
- **Screenshots**: Dashboards, error messages, monitoring graphs

Read all provided files. For images, analyze visually.

### 2. Clarify Context

Ask the user to confirm understanding:
- What service/component was affected?
- When did the incident occur (timeline)?
- What was the observed vs expected behavior?
- What is the current status (resolved/ongoing)?

### 3. Analyze with LLM Ops Patterns

For DekaLLM/LLM service issues, consult [llm-ops-patterns.md](references/llm-ops-patterns.md) for:
- vLLM issues (latency, OOM, timeouts)
- LiteLLM proxy errors (5xx, routing, auth)
- SGLang runtime problems
- Key metrics interpretation

**Fetch documentation when needed**:
- vLLM: `https://docs.vllm.ai/en/stable/`
- LiteLLM: `https://docs.litellm.ai/docs/`
- SGLang: `https://sgl-project.github.io/`

### 4. Generate RCA

Use the template from [rca-template.md](references/rca-template.md).

Required sections:
1. **Executive Summary** - 2-3 sentences: what, impact, status
2. **Timeline** - Chronological events in table format
3. **Impact** - Services, users, business affected
4. **Root Cause** - Primary cause + contributing factors with evidence
5. **Resolution** - Actions taken and verification
6. **Prevention** - Action items with owners (short/medium/long term)
7. **Lessons Learned** - What worked, what to improve

### 5. Output

Save the RCA as markdown file. Suggest filename: `RCA-[YYYY-MM-DD]-[brief-description].md`

## Quick Reference

| Input Type | How to Analyze |
|------------|---------------|
| CSV metrics | Look for anomalies, compare against baselines |
| Error logs | Extract error patterns, stack traces, timestamps |
| Screenshots | Identify metrics spikes, error states |
| Documents | Extract timeline, previous actions taken |

## Example Triggers

- "Create an RCA for yesterday's outage"
- "Analyze these logs and tell me what went wrong"
- "Generate a post-mortem from this incident"
- "Why did our vLLM service timeout?"
- "Here's our load test results, can you create an RCA?"
