---
type: retirement
retirement_id: RET-0003
retired_at: 2026-08-26T17:52:27Z
origin: project
scope: estate
scope_evidence: "released by the format, so it binds every consumer of it"
decided_in: "lumastack/luma-knowledge-format#SPEC.md§13"
was: "`applies_to`, the field naming what makes a Document surface"
now: "`matches` (§10.7)"
why: "the old name obliged an author to write a false sentence — `applies_to: everything` claims a rule governs everything, when it only says what surfaces it"
recognizers:
  - kind: term
    value: applies_to
  - kind: shape
    value: "a frontmatter key `applies_to:` on a policy or workflow"
except:
  - ".luma/records/"
lifecycle_status: stable
---

# RET-0003: `applies_to`

## What it was

The frontmatter field on a `policy` or `workflow` declaring what brings it into
play.

## What replaced it

`matches`, with the same three forms — `always`, a list of triggers, or nothing
at all.

## How to recognize it

Purely mechanical. The name is not ordinary English and never appears by
accident, which makes this the cheapest entry on the list and the least
interesting.

## Where a hit is correct

Version histories, and documents recording the migration itself — including any
that describe the intermediate step where `preload` briefly became `compliance`
plus `applies_to`.
