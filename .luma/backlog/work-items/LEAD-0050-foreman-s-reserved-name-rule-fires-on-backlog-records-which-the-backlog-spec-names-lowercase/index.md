---
type: work-item
key: LEAD-0050
title: Foreman's reserved-name rule fires on backlog records, which the backlog spec names lowercase
description: inspect reports MEDIUM on all 49 work-item index.md files, wanting INDEX.md. The backlog work-item definition names index.md as the record's identity, which wikilinks resolve against, so renaming would break every record. Foreman's bundles rule is scoping into .luma/backlog/work-items/. The fix belongs in foreman; luma-leader only sees the symptom.
workflow_status: captured
rank: 010.0500.000
kind: defect
stage: draft
created: {by: 'human:benlinton', at: '2026-09-25T16:08:38Z'}
---
# Foreman's reserved-name rule fires on backlog records, which the backlog spec names lowercase

## The problem

`luma-foreman inspect` reports **MEDIUM, 49 reserved name(s) in the wrong case**
(`rule=bundles surface=working-tree`) — every work item's `index.md`, wanting
`INDEX.md`.

**Renaming them would break the records.** The `work-item` Type Definition names
the lowercase path as the record's identity:

> **path** | `backlog/work-items/WORK-0001-lint-the-corpus/index.md` — the
> **identity**. A wikilink resolves against it, and it is what a record *is*
> (`spec.md` §7.1).

So `index.md` is deliberate and load-bearing. `luma-backlog` writes it, reads it,
and resolves wikilinks against it. Following the finding would break record
identity and every cross-reference.

**The rule is `bundles`, and it is scoping into `.luma/backlog/work-items/`** —
applying a bundle-container convention to records a different spec governs.
Nothing in any adopted bundle defines the reserved-name rule, so it is built into
foreman.

**The fix belongs in `luma-foreman`.** This repository only sees the symptom.
Either the `bundles` rule stops reaching into `.luma/backlog/`, or the reserved
name is not `INDEX.md` for a container the backlog owns.

## Out of scope

**Renaming the 49 files.** That is the one action this must not lead to.

---

## Notes from capture — `agent:claude-opus-5`, not part of the report

**Why it appeared now.** There were no work items in this repository before the
ideas migration, so the count went 0 → 49 in one commit. The rule was presumably
always going to fire; nothing had triggered it.

**The finding's own escape hatch covers this**: *"If the lowercase name was
deliberate, nothing is wrong and this is only a notice."* It was deliberate. The
open question is whether a MEDIUM finding should be the thing that says so, given
a reader's correct response is to ignore it 49 times.

### Overlaps

- **[[work-items/LEAD-0012-check-a-bundle-s-ring-where-the-bundle-is-written-not-only-where-it-is-adopted]]**
  — a foreman check **not running where it should**. This is a foreman check
  **running where it shouldn't**. The same rule-scoping question approached from
  opposite ends, and an answer to one probably shapes the other.

- **[[work-items/LEAD-0011-committed-derived-material-has-no-tier-and-the-cache-deferral-asked-the-wrong-question]]**
  — generated material sitting in `bundles/` "because there is nowhere else", and
  inheriting bundle rules it was never meant to answer to. This is the same
  category error one directory over: a non-bundle judged by bundle rules.

**Neither is a duplicate** and both should stay open. The seam worth naming is
that LEAD-0011 and LEAD-0012 are about what foreman's rules *should* cover, and
this is a concrete case where one demonstrably overreaches.
