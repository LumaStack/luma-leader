# Audit template

Copy the blocks to `.luma/records/audits/<DATE>-<SHA>/audit.md`. **Copy the
blocks, not this file.**

## Frontmatter

```yaml
---
type: audit
title: <what was audited, in a few words>
audited: YYYY-MM-DD
commit: <at least 12 characters>
scope: <what was examined — and say what was not, in the body>
auditor: human:<id> | agent:<model> | process:<tool>
---
```

## Body

```markdown
# Audit: <what was audited>

## Scope

<What was examined.>

**Not examined:** <the half people skip. Without it a reader cannot tell
"examined and clean" from "never looked".>

## Method

<How. Tools run, what was read, what was sampled rather than exhausted.>

## Findings

<A line per finding — id, severity, one clause. Not a restatement: a reader
should be able to decide from this table alone whether to read further.>

| id | severity | summary |
| --- | --- | --- |
| F-001 | high | … |

<Or, if a single auditor kept findings in this file rather than in findings/,
each finding as a section here — accepting that they are then not addressable
individually. See the layout policy.>

## Summary

<What the state of this thing is, in a few sentences. The part somebody reads
when deciding whether this audit changes their plans.>
```
