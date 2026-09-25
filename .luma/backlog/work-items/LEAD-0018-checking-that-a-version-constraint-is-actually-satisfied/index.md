---
type: work-item
type_version: "0.0.2"
key: LEAD-0018
title: 'Checking that a version constraint is actually satisfied'
description: 'Archived on the day the code that did half of it was deleted, because version constraints are expected back and this should not be rediscovered from scratch.'
workflow_status: closed
rank: 070.0010.000
kind: idea
stage: archived
created: {by: 'agent:claude-opus-5', at: '2026-08-28T00:00:00Z'}
closed: {on: 2026-09-25, as: canceled, by: 'human:benlinton', reason: 'Arrived in the ideas corpus already archived, on 2026-08-28, when the code that did half of it was deleted. Closed canceled rather than completed: nothing was delivered — the work was withdrawn. Its own body states the re-open trigger (a catalog that actually declares a version constraint, then the project-side check first) and names where the deleted code lives: luma-catalog-curator 830e1c8.'}
modified: {by: 'human:benlinton', at: '2026-09-25T15:17:56Z'}
---

# Checking that a version constraint is actually satisfied

> **This idea was archived on 2026-08-28 in the corpus it came from**, and is closed here rather than left open. Its own body says what withdrew it and what would earn it back; that is the re-open trigger.

**Archived on the day the code that did half of it was deleted, because version
constraints are expected back and this should not be rediscovered from scratch.**

A `luma/catalog` may put a constraint on an obligation:

```yaml
requires:
  - bundle: acme/change-review
    obligation: mandatory
    version: ">= 2.0.0"
```

**Nothing anywhere checks it.** Not when the catalog is curated, not when a
project adopts, not when `inspect` runs.

## The two checks it would serve, in two different tools

**Catalog-side, in `luma-catalog-curator`.** A catalog that mandates `>= 2.0.0`
of a bundle it publishes at `1.5.0` is internally contradictory, and **every
consumer adopting from it is born failing a requirement it cannot satisfy.**
`luma/catalog` names this as the kind of defect no per-field validation reaches.

**Project-side, in `luma-foreman`.** A project holding `1.5.0` of a bundle its
catalog mandates at `>= 2.0.0` is out of compliance and does not know.
**`luma-foreman` does not read `requires` at all**, so every obligation a catalog
declares is currently unenforced — the constraint is only the sharpest edge of
that.

## What existed, and where it is

`_satisfies(version, constraint)` and `_parts(version)` in the curator —
comparison and parsing, about forty lines, well tested. Written for one caller:
a `starters` pin conflicting with the catalog's own mandate. When `starters` was
withdrawn the caller went and the pair was uncalled.

**The code is at `luma-catalog-curator` `830e1c8`**, with its tests in the same
commit. Nothing needs rewriting from memory.

## Why it was deleted rather than kept

**Because "we will want this later" is exactly what `starters` was.** A
mechanism that exists, is consumed by nothing, and gets cited as though it were
built. Uncalled code is the same defect as a declared field nobody reads, and
the whole of 2026-08-27 and 28 was spent removing that shape.

**And no catalog declares a version constraint today.** `luma-catalog`'s single
`requires` entry carries an obligation and no version, so both checks would have
run on zero inputs — passing green, forever, saying nothing.

## What would earn it back

**A catalog that actually declares one.** Not a plan to; a live entry.

**Then the project-side check first, not the catalog-side one.** The catalog-side
check catches an author's mistake and the project-side check is what makes
`requires` mean anything at all. Building the easier one first would produce a
tool that validates obligations nobody enforces.

**And a decision about what happens when a constraint is unmet.** An error at
`get` time refuses an adoption somebody may have good reason for; a finding at
`inspect` time reports it and lets them decide. That is the same shape as
`on_violation`, and probably has the same answer.

## Related work

- Born from the idea it replaces: [`checking-version-constraints`](../../ideas/archived/checking-version-constraints.md)
