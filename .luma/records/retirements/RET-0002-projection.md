---
type: retirement
retirement_id: RET-0002
retired_at: 2026-08-26T19:26:16Z
origin: project
scope: estate
scope_evidence: "decided in luma-foreman; found in luma-leader's drafts, its backlog and its decision log"
decided_in: "lumastack/luma-foreman#ADR-0003"
was: "`projection`, the noun for what `apply` writes"
now: "the **adapters**, or `apply` itself"
why: "it collided with `project`, this ecosystem's most load-bearing noun — `apply — project what this project adopted` is one word twice, two meanings"
recognizers:
  - kind: term
    value: projection
  - kind: claim
    value: "naming the generated output with a noun of its own rather than describing it as what `apply` writes"
except:
  - ".luma/records/"
lifecycle_status: stable
---

# RET-0002: `projection`

## What it was

The noun for the artifacts `apply` produces — the skills and the index.

## What replaced it

**The adapters**, or simply *what `apply` writes*. The verb *project* survives;
it is the noun that collided.

## How to recognize it

The term catches most of it. The claim catches the rest: an author who needs a
name for the generated output will invent one, and `projection` is the obvious
invention. **This is the entry that demonstrates the whole problem** — it was
reinvented within minutes of the sweep that removed it.

## Where a hit is correct

Version histories, and `luma-leader`'s `loading-mechanisms.md`, whose naming
section is the record of this retirement.
