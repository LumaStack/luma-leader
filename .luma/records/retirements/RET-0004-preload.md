---
type: retirement
retirement_id: RET-0004
retired_at: 2026-08-26T17:52:27Z
origin: project
scope: estate
scope_evidence: "released by the format; found in a published policy reaching every adopter of bundle-manager, and in six luma-leader documents"
decided_in: "lumastack/luma-knowledge-format#SPEC.md§13"
was: "`preload`, a Document declaring when it should be placed in front of a reader"
now: "delivery is derived from `matches`; `matches: always` is the only route to being loaded up front"
why: "consumption belongs to whatever distributes and loads Bundles, not to the format that defines them"
recognizers:
  - kind: term
    value: preload
  - kind: shape
    value: "a frontmatter key `preload:`, or the invented `preload_default`"
  - kind: claim
    value: "any document telling an author to declare when their own document loads, or treating loading as a tier the author files into"
except:
  - ".luma/records/"
lifecycle_status: stable
---

# RET-0004: `preload`

## What it was

A core field: `preload: mandatory | optional`, declaring whether a Document
should be loaded before work started. Authors filed documents into loading tiers.

## What replaced it

Nothing declares delivery. A consumer *derives* it from `matches`, and the
default reversed — a document saying nothing about what surfaces it is available
on request rather than loaded. `matches: always` is the one route to being in
front of a reader up front, and since the reversal it can only be chosen, never
fallen into.

## How to recognize it

The term is reliable. **The claim is the one that matters**: the old model was
*the author decides when this loads*, and that survives without the word — any
prose treating loading as something an author files into rather than something a
consumer derives is a hit.

`preload_default` deserves its own mention: it was never specified at all, and
appeared in a published policy as though it had shipped.

## Where a hit is correct

Version histories, records, and the documents that argued this change — including
`luma-leader`'s `conditional-preload` backlog idea, which is named for the field
and is about the field.
