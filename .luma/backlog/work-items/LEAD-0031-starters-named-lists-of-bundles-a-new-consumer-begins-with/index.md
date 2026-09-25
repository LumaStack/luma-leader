---
type: work-item
type_version: "0.0.2"
key: LEAD-0031
title: 'Starters — named lists of bundles a new consumer begins with'
description: 'Withdrawn from luma/catalog and from the live CATALOG.md on 2026-08-27, and kept here rather than deleted so that whoever proposes it next argues against what it was rather than reinventing it.'
workflow_status: closed
rank: 070.0020.000
kind: idea
stage: archived
created: {by: 'human:benlinton', at: '2026-08-23T00:00:00Z'}
closed: {on: 2026-09-25, as: canceled, by: 'human:benlinton', reason: 'Arrived in the ideas corpus already archived, on 2026-08-27, when starters was withdrawn from luma/catalog 0.3.0 and from the live CATALOG.md. Closed canceled rather than completed: it was withdrawn, not shipped. Its own body states the re-open trigger — the consumer-kind declaration first, then a use case beyond symmetry with requires, then a decision about who chooses.'}
modified: {by: 'human:benlinton', at: '2026-09-25T15:17:57Z'}
---

# Starters — named lists of bundles a new consumer begins with

> **This idea was archived on 2026-08-27 in the corpus it came from**, and is closed here rather than left open. Its own body says what withdrew it and what would earn it back; that is the re-open trigger.

**Withdrawn from `luma/catalog` and from the live `CATALOG.md` on 2026-08-27**,
and kept here rather than deleted so that whoever proposes it next argues
against what it was rather than reinventing it.

## What it was

A catalog declares named lists — conventionally one per kind of consumer it
serves — and a new consumer begins with the list matching what it is.

```yaml
starters:
  project:
    extends: upstream/project
    adds:
      - bundle: acme/deploy-checks
        version: "0.2.3"
      - bundle: acme/incident-response
    excludes:
      - upstream/adr-workflow
```

**Three properties it had, all of which survive the withdrawal and are worth
keeping if it returns:**

- **Never retroactive.** Changing a starter changes what the *next* thing begins
  with and touches nothing existing. That is what let an organization evolve its
  defaults freely, and it is why anything meant to reach existing consumers
  belonged in `requires` instead.
- **Called starters rather than defaults for that reason.** A default is an
  ongoing fallback consulted every time; this fired once.
- **Pins optional, unpinned the common case.** An entry with no version took the
  latest at bootstrap, and the adopting consumer recorded what it got.

It was also the only list where **subtracting** was legitimate, which is why it
needed explicit `extends` / `adds` / `excludes` where `tags` and `requires`
merge additively. That asymmetry is gone with it.

## Why it was withdrawn

**It was written before anything could use it.** A starter keys on what kind of
consumer is adopting, and **no consumer declares its kind** — see
[[work-items/LEAD-0010-a-repository-cannot-say-what-kind-of-consumer-it-is-so-nothing-keys-on-it]]. So the lists sat in a published
catalog describing a bootstrap nothing performed, and every field they declared
read as settled while being unreachable.

**Nothing read them.** Not `luma-foreman`, not the curator, not any workflow. A
declared mechanism nobody consumes is worse than a missing one: it looks like an
answer, so nobody builds the thing that would actually answer it.

**And it kept being cited as though it were built.** In the design work of
2026-08-27 it was reached for three separate times as *the* answer to *what does
a new consumer begin with* — including as an argument against a competing
proposal. An unbuilt sketch that wins arguments is doing damage rather than
waiting quietly.

## What would earn it back

**The consumer-kind declaration first.** Until a repository can say what it is,
nothing can key on it and this cannot work in any spelling.

**Then a reason beyond symmetry with `requires`.** The two look like a pair —
*what you start with* and *what you owe* — and that symmetry is most of why this
existed. Symmetry is not a use case. What is needed is somebody actually blocked
by not having it, having already declared a kind.

**And a decision about who chooses.** `the-estate` records that *the adopter's
own selection* is missing and that nothing in the format provides it. A starter
answers a neighbouring question — what the **catalog** suggests — and shipping
the catalog's answer while the adopter's is missing may be the wrong half to
build first.

## Notes

`requires`, `tags` and `upstream` are untouched and remain live. Only `starters`
went.

**The withdrawal is `luma/catalog` `0.3.0`**, minor with the removal stated
outright: a catalog still declaring `starters` is declaring a field the type no
longer defines, which §4 says a consumer tolerates and must not reject.

## Related work

- Born from the idea it replaces: [`starters`](../../ideas/archived/starters.md)
