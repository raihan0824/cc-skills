# RCA Markdown Template

Use this structure for all RCA outputs.

```markdown
# Root Cause Analysis: [Incident Title]

**Date**: [Incident Date]
**Severity**: [Critical/High/Medium/Low]
**Status**: [Resolved/Ongoing/Monitoring]
**Author**: [Name/Team]

---

## Executive Summary

[2-3 sentence summary: what happened, impact, and resolution status]

---

## Timeline

| Time (UTC) | Event |
|------------|-------|
| HH:MM | [First indicator/alert] |
| HH:MM | [Escalation/Investigation started] |
| HH:MM | [Root cause identified] |
| HH:MM | [Mitigation applied] |
| HH:MM | [Service restored] |

---

## Impact

### Affected Services
- [Service/endpoint affected]

### User Impact
- [Number of users affected, if known]
- [Error rate/availability during incident]
- [Duration of impact]

### Business Impact
- [SLA breach, revenue impact, customer complaints]

---

## Root Cause

### Primary Cause
[Clear statement of the root cause]

### Contributing Factors
- [Factor 1]
- [Factor 2]

### Technical Details
[Detailed technical explanation with evidence from logs/metrics]

---

## Resolution

### Immediate Actions Taken
1. [Action 1]
2. [Action 2]

### Verification
- [How resolution was verified]

---

## Prevention

### Short-term (< 1 week)
- [ ] [Action item with owner]

### Medium-term (< 1 month)
- [ ] [Action item with owner]

### Long-term
- [ ] [Architectural/process changes]

---

## Lessons Learned

### What went well
- [Positive aspect of incident response]

### What could be improved
- [Area for improvement]

---

## Appendix

### Supporting Evidence
- [Links to dashboards, logs, metrics]

### Related Incidents
- [Previous similar incidents, if any]
```

## Severity Guidelines

| Severity | Definition |
|----------|------------|
| Critical | Complete service outage, all users affected |
| High | Major functionality degraded, >50% users affected |
| Medium | Partial degradation, <50% users affected |
| Low | Minor issue, minimal user impact |
