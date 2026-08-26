---
type: luma/idea
title: Changing many moving parts at once, without leaving the files competing
created: { by: human:benlinton, at: 2026-08-26T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle_status: draft
---

# Changing many moving parts at once

## The idea, as raised

**When making big changes that touch many files — code, config, policies,
workflows, and/or docs — we need to design a process for changing many moving
parts in a way that doesn't leave files competing with each other.**

The failure, as observed:

- **Old docs start trying to tell the new docs how they are wrong.**
- **Old policy tries to combat new policy.**

What is wanted:

- **Give agents a way to resolve old from new.**
- **Track what needs to change and what's remaining.**
- **Ignore the old stuff somehow**, so it doesn't leak into the new way of
  things or reverse it.

**Maybe the backlog will do all of this work, but maybe there is more to it** —
so the idea is created.

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it. Evaluation is welcome here — it just does
not get to edit what was raised.*

### The worked example is the twenty-six hours that produced this idea

`preload` became `compliance` + `applies_to`, then `compliance` was removed
again, across five repositories between 2026-08-25 morning and 2026-08-26. What
that left behind, found by walking the estate rather than by looking for
trouble:

| where | what it still says |
| --- | --- |
| `SPEC.md`, and foreman's `TRIGGER_KINDS` | `always` is one of six trigger kinds |
| `loading-mechanisms.md`, final scoping | there are five, and `always` is not among them |
| curator `catalog.py` / `report.py` | the headline cost column counts `preload: mandatory` |
| foreman `outfit.py` | a `legacy_preload` path nothing can set any more |
| `loading-mechanisms.md`, what it owes | asks whether seven policies are misfiled; PR #80 answered it |
| `create-bundle.md` | tells a bundle author to write `compliance: recommended` |

**Every one of those speaks with exactly the authority it had before it went
out of date.** Nothing in any of them is marked as belonging to a regime that
ended.

### The mechanism underneath: authority does not decay

A Document can say how mature it is — `lifecycle_status: draft | provisional |
stable | archived | unknown` — and a `decision` can name what replaced it via
`superseded_by`. **Neither says what this idea needs**, for two reasons:

- **They are per-document, and a sweeping change lands per-sentence.**
  `create-bundle.md` is not archived. It is a current, correct workflow with one
  stale paragraph in the middle of it.
- **They describe a document's own state, not a change's.** Nothing anywhere
  holds the fact *the vocabulary moved from X to Y on this date, and here is
  every site*. That fact currently lives in a commit message and in whoever was
  in the room.

### Three jobs are hiding in here, and they should not be built as one

| | the job | plausibly whose |
| --- | --- | --- |
| **the ledger** | what must change, where, and what remains | **`luma-backlog`** — this is the half the raiser already suspected |
| **the verdict** | which of two contradicting statements is current | nothing owns this. It needs a record of the change itself, not of either document |
| **the quarantine** | superseded content never reaches an agent at all | **the loading design**, and it may be nearly free |

**The third is the one worth noticing.** The loading design already decides what
reaches a reader and when. Content that never loads cannot argue with anything —
so *quarantine* may be a consequence of triggers rather than a new mechanism:
a document belonging to an ended regime is one nothing should fire on. That is a
much smaller ask than a new subsystem, and it is testable today.

**The second is the genuinely new thing.** A ledger tracks work; a verdict
resolves a contradiction an agent is standing in front of right now. *These two
sentences disagree, and this one is the current one* is not derivable from
either document — it needs the change to be a thing with a record, which is
close to how a `decision` already works and might simply be one.

### What already exists, so it is not built twice

**`change-a-shared-type` is the narrow version, already solved.** Expand,
migrate, contract — never require two things to ship together. Its discipline
would have caught the `always` disagreement outright, because the spec and the
design doc did have to agree simultaneously and nothing made that visible. **The
general version — vocabulary, prose, policy, and code at once — is what is being
asked for, and the three-step shape is probably the thing to generalise from.**

**`bundle-migrations`** (`luma-catalog/.luma/backlog/ideas/`) is adjacent and
different: it walks an *adopter* from version X to Y. This idea is about the
estate's own sweep, before anything is published.

**"Everything is committed, and deletion is not the tool"** (`the-estate`) is
why superseded content lingers by design, and that rule is right. So the answer
cannot be *delete the old* — it has to be *mark it, and stop delivering it*.

### The failure mode to design against, stated once

**A half-finished sweep reads exactly like a finished one.** Every file parses,
nothing errors, and the aggregate is silently wrong.

That is the third sighting of this shape: `change-a-shared-type` warns about it
for aggregation across projects, `loading-mechanisms` names it for a trigger
that matches nothing, and this is it again for a vocabulary migration. **It may
be the estate's characteristic failure, and worth naming as one** — the answer
each time has been to make the silent thing produce a number or a check.

### Scoping caution

**The trigger for this process should be narrow**, or it becomes change
management for every edit. The version worth having fires on a change that
**renames or removes shared vocabulary** — a field, a type, a reserved name, a
value in an enum — which is exactly the set that produced every row in the table
above, and is rare enough that a real ceremony is affordable.

## Notes

Raised immediately after `luma-foreman`, `luma-leader` and `luma-catalog-curator`
were re-adopted onto the post-`compliance` bundles, which is when the
disagreements above became visible in one sitting. The immediate evidence is
that the sweep looked finished from every individual repository.
