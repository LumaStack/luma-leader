---
type: work-item
type_version: "0.0.2"
key: LEAD-0044
title: 'Design first working mode'
description: 'The maintainer works spec-first — nail the design docs before any implementation, and expects sessions scoped to one or the other.'
workflow_status: captured
rank: 010.0440.000
kind: idea
stage: draft
created: {by: 'human:benlinton', at: '2026-08-30T21:24:09-05:00'}
# created derived from the file's first commit — the source had no frontmatter
---

# Design first working mode

> **Migrated from `.luma/backlog/ideas/` — read this first.** This was not an idea record. It arrived in the corpus shaped as an agent-memory file — `name`, `description`, `metadata.type` — and its body is reproduced verbatim below. **I am not sure this belongs in the backlog as a work item**; it reads as a standing convention rather than work to do, and may want to live in agent memory or as a project convention. It is here as an idea so that decision gets made rather than lost.

Projects run design-first: the specification is developed and settled in markdown docs before implementation starts. A session may be explicitly scoped to design-only ("I do not want to write any code for this session"), and that scope is expected to hold for the whole session, not just the first reply.

**Why:** This mirrors how the sibling repository `luma-knowledge-format` is run — a docs-only specification repository (SPEC / PRINCIPLES / ROADMAP / GUIDELINES / CHANGELOG) whose GUIDELINES.md encodes the rule that agents draft and a human ratifies. The same trust model applies to working sessions: get the thinking right on paper, under human sign-off, before code exists to argue with.

**How to apply:** When a session is scoped to design, produce and iterate on markdown docs — do not scaffold projects, write source files, or reach for code as a way of "showing" a design. Prefer discussion plus doc edits. Also prefer few, well-argued recommendations over exhaustive option surveys; a formal multi-question prompt was pushed back on in favor of open discussion.

## Related work

- Born from the idea it replaces: [`design-first-working-mode`](../../ideas/design-first-working-mode.md)
