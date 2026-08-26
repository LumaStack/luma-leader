---
type: finding
title: Nothing in the estate records whether a human or an agent wrote it
finding_id: F-001
severity: high
location: every repository — frontmatter and git metadata alike
---

# F-001: Nothing in the estate records whether a human or an agent wrote it

## Condition

**Two independent places could carry authorship, and neither does.**

**Document frontmatter.** Of 160 documents in the universal catalog, 9 carry
`created`, 6 carry `modified`, and 5 carry `verified` — and all five of those
are templates and Type Definitions demonstrating the field, not uses of it.
**Not one published document records who wrote it or that anybody confirmed it.**

**Git.** Every commit in the estate is authored as a person:

| repository | authors |
| --- | --- |
| `luma-knowledge-format` | 97 Benjamin Linton, 35 Ben Linton |
| `luma-catalog` | 176 Benjamin Linton, 82 Ben Linton |
| `luma-foreman` | 96 Benjamin Linton, 39 Ben Linton |
| `luma-leader` | 130 Benjamin Linton, 44 Ben Linton |

Four commits across the estate carry any agent or co-author trailer. **Agent
work and human work are indistinguishable in the history**, including every
commit made during this session, which an agent wrote and which git attributes
to a person.

## Criteria

**The format specifies all three fields.** §7.1 defines `created` and `modified`
as actor events; §7.2 defines `verified` as independent confirmation, with trust
tiers derived from it — *no `verified` ⇒ unverified*, *verified by any
`human:<id>` ⇒ human-reviewed* — and states that trust tier is orthogonal to
`lifecycle_status`.

**The actor convention exists for exactly this.** §7.4 defines
`<kind>:<producer>` with `human:` and `agent:` as distinct kinds. The vocabulary
was designed to make this distinction and nothing uses it.

## Cause

**The fields are optional and nothing consumes them.** No tool reads `created`,
`modified` or `verified`; `inspect` does not report their absence; the curator
does not count them. A field with no reader is a field nobody fills in, which is
the same mechanism that emptied `compliance` before it was removed.

**It is the shared cause of F-002 and F-003**, and of most of Lens B. Five
findings with one cause are one problem.

## Effect

**There is no way — mechanical or otherwise — to separate what the maintainer
decided from what an agent wrote well and nobody read.** That is not a
hypothetical: F-002 records three documents asserting a decision nobody made,
and the maintainer has stated that agent-authored content "takes a lot of
liberties" without any means of finding it.

**The cost compounds rather than accumulating.** An unmarked agent assertion is
read by the next agent as estate convention, cited, and thereby strengthened —
so the population of unearned claims grows without anybody adding to it
deliberately.

**And it gets worse with time, not better.** Every day of unattributed history
is a day that cannot be reconstructed later, because the signal was never
captured.

## Recommendation

**Make one of the two places carry it, and prefer git.** A commit trailer costs
nothing per document, cannot go stale, and covers content that is not a Document
at all — code, configuration, workflows. Frontmatter `created`/`modified` is the
format's own answer and is better for anything a tool will query.

**`verified` is the higher-value half and should not wait for the other.** It
answers *did a person stand behind this*, which is the question actually being
asked, and it is orthogonal to who typed it. A backfill is not required: an
empty `verified` correctly reads as *nobody has confirmed this*, which is true
today of all 160 documents.

**Give it a consumer in the same change or it will empty out again.** The
minimum is `inspect` reporting the count of unconfirmed documents in a bundle,
which makes the number visible without demanding anybody act on it.
