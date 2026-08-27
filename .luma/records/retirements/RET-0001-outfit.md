---
type: retirement
retirement_id: RET-0001
retired_at: 2026-08-26T19:26:16Z
origin: project
scope: estate
scope_evidence: "decided in luma-foreman; the word appeared in the catalog, in luma-leader's drafts and in every bundle documenting the tool"
decided_in: "lumastack/luma-foreman#ADR-0003"
was: "the command `outfit`, for writing what a project adopted into what its harness reads"
now: "`apply`"
why: "the metaphor never appeared without a gloss beside it, and nobody's first guess at a prompt is `outfit`"
recognizers:
  - kind: term
    value: outfit
  - kind: shape
    value: "the module `outfit.py`, now `apply.py`"
  - kind: claim
    value: "any instruction to re-run the projection step that names it as a crew being outfitted"
except:
  - ".luma/records/"
lifecycle_status: stable
---

# RET-0001: the command `outfit`

## What it was

`luma-foreman outfit` wrote the adapters and the index. The tool is named for a
role, and the command set followed the metaphor: a foreman *outfits* a crew.

## What replaced it

`apply`. Idempotent, re-runnable, and the terraform reading of exactly what it
does — make derived state match declared state. `apply --check` also reads
correctly in a CI gate, which `outfit --check` never did.

## How to recognize it

The word is rare enough in ordinary English here that a term match is reliable.
The one that is not mechanical: prose describing the *act* in metaphor —
outfitting, kitting out, equipping a crew — without using the command name.

## Where a hit is correct

Version histories, which say what was true when written, and the section of
`luma-leader`'s `loading-mechanisms.md` that argues about which word should
replace it. That argument cannot be restated in the word it settled on.
