---
type: work-item
key: LEAD-0051
title: Retired-word excepts do not cover the migrated work items
description: 'The ideas migration copied prose into .luma/backlog/work-items/, and the except lists in .luma/config/luma-foreman.toml point only at .luma/backlog/ideas/ and docs/. Retired-vocabulary notices roughly doubled: projection 12, outfit 12, preload 69, applies_to 23, with 18 cited lines being the new copies. Extend the excepts, or drop the ideas/ entries once those files go.'
workflow_status: captured
rank: 010.0510.000
kind: defect
stage: draft
created: {by: 'human:benlinton', at: '2026-09-25T16:08:38Z'}
---
# Retired-word excepts do not cover the migrated work items

## The problem

The ideas migration copied every idea's prose into
`.luma/backlog/work-items/LEAD-NNNN-…/index.md`. The retired-term `except` lists
in `.luma/config/luma-foreman.toml` name only the old locations —
`.luma/backlog/ideas/…`, `docs/…`, `.luma/records/` — so the new copies are not
excepted and `inspect` now reports them.

Retired-vocabulary notices roughly doubled:

| term | notices | retired by |
| --- | --- | --- |
| `preload` | 69 | SPEC.md section 13, v0.0.12 |
| `applies_to` | 23 | SPEC.md section 13, v0.0.15 |
| `projection` | 12 | ADR-0003, ADR-0005 |
| `outfit` | 12 | ADR-0003, ADR-0004, ADR-0005 |

18 of the cited lines are the new copies under `work-items/`. The heaviest are
`LEAD-0039-loading-mechanisms` and `LEAD-0043-decision-ideas` — the two largest
bodies, and both already excepted at their old paths for the reason the config
states: they *are* the record of the retirement, so they have to name the words.

## What is being delivered

**Either** extend each `except` list to the corresponding `work-items/` path,
**or** wait and drop the `ideas/` entries once those files are deleted after the
migration review. The second is less work and less duplication, but leaves the
notices standing until then.

The pattern is already established — `.luma/backlog/plans/retirement-framework.md`
is excepted on exactly this grounds.

## Constraints

**Do not except `.luma/backlog/work-items/` wholesale.** A revival in a work item
is a real thing to catch; only the migrated copies of retirement-recording prose
have a claim to exemption.

---

## Notes from capture — `agent:claude-opus-5`, not part of the report

**This is smaller as a fix than as a record**, and it partly evaporates on its
own: once the `ideas/` files are deleted after the migration review, the old
`except` entries go stale and the new ones become the only ones needed. Doing it
before that decision means editing the same four lists twice.

**The duplication is the real cost, not the notices.** The same prose now exists
at two paths, and every vocabulary except, link and sweep has to name both until
one copy goes.

### Overlaps

- **[[work-items/LEAD-0041-retiring-a-concept]]** — designs the retirement
  framework: how a retired idea comes back, and what has to defend both
  directions at once. **The seam:** LEAD-0041 is the design; this is maintenance
  inside the framework that already exists. If LEAD-0041 lands first it may
  remove the need for hand-maintained path lists entirely, which would close this
  as superseded rather than fixed.

**No duplicates.** `.luma/backlog/plans/retirement-framework.md` is the plan this
framework came from and is worth reading alongside LEAD-0041; it is a plan rather
than a work item, so it is not a relation.
