---
type: luma/idea
title: Changing many moving parts at once, without leaving the files competing
created: { by: human:benlinton, at: 2026-08-26T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
stage: draft
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
| foreman `apply.py` | a `legacy_preload` path nothing can set any more |
| `loading-mechanisms.md`, what it owes | asks whether seven policies are misfiled; PR #80 answered it |
| `create-bundle.md` | tells a bundle author to write `compliance: recommended` |

**Every one of those speaks with exactly the authority it had before it went
out of date.** Nothing in any of them is marked as belonging to a regime that
ended.

### The mechanism underneath: authority does not decay

A Document can say how mature it is — `stage: draft | provisional |
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

### The second axis, as raised: what it claims, and whether anyone still stands behind it

**Raised as an addition to the idea, and it answers all three jobs above with
one field.** Two axes, where the estate currently reasons on one:

| axis | the question | where it lives today |
| --- | --- | --- |
| **the claim** | how does this bind, *taken at face value* | `type`, `matches`, `on_violation` — every document states it |
| **the confirmation** | is it trusted right now, is it ready, is it current with how we now think | **`verified`, and nothing reads it** |

**A document written perfectly for the old world still states its claim with
full confidence.** Sincerity does not expire, which is why the first axis alone
cannot resolve a contradiction.

**The format already has the second axis and does not know it.** §7.2:

```yaml
verified:
  - { by: human:benlinton, at: 2026-08-26T09:00:00Z }
```

A list of independent confirmation events, **explicitly independent of
`modified`** — *"content can change without re-confirmation, and be re-confirmed
without changing"* — and trust tier is **orthogonal to `stage`**. That
orthogonality is exactly the one this idea needs, written down before anybody
needed it.

**What is missing is what a confirmation was made *against*.** `{by, at}` says
somebody confirmed this at a moment; it does not say under which worldview.

### Two ways to close it, and they layer rather than compete

**Derive it, by date.** Give a change a date, and any document whose newest
`verified.at` predates it has not been confirmed under the new regime. No new
field: the change record supplies the date, `verified` supplies the timestamp,
and the rest is subtraction. **The honest limit is that it proves *not* current
and cannot prove current** — a document confirmed last Tuesday for unrelated
reasons passes without anybody having thought about the change.

**Stamp it, on the document.** A verification event gains an end:

```yaml
verified:
  - { by: human:benlinton, at: 2026-08-26T09:00:00Z, invalid_at: 2026-08-27T14:00:00Z }
```

**This is push where the date test is pull.** A reader holding only the file —
a bare clone, no tooling, no change record — can see that the confirmation is
dead. Every failure this idea is about is silent, so a form that speaks without
being interrogated is worth more than its cost.

**They are the same layering the loading design already settled**, arriving a
third time: the change record is the declaration and the source of truth,
stamping is what `apply` writes, and the floor when no tool is present is whatever
was committed. Derive first, stamp when the document should carry its own truth.

### What `invalid_at` changes, precisely

**It separates three states that currently collapse into two.**

| | means |
| --- | --- |
| no `verified` | **nobody ever checked** |
| `verified`, live | somebody checked, and it stands |
| `verified`, `invalid_at` set | **somebody checked, and it is known not to stand any more** |

The third is strictly more informative than the first, and today they are
indistinguishable. *Never examined* and *examined, now known stale* are
different facts about a document, and only one of them tells you a person has
already read it once.

**It is append-only, which is what the estate requires.** Ending an event rather
than deleting it keeps the history: that Benjamin confirmed this on the 26th
remains true and visible after the confirmation stops counting. Deleting the
entry would lose the only evidence anybody ever looked.

**The name is open. Three candidates, as raised:**

| candidate | what it says | against it |
| --- | --- | --- |
| **`invalid_at`** | the confirmation no longer holds, from this moment. Neutral about why | says nothing about cause, so *aged out* and *actively withdrawn* read alike |
| **`compromised_at`** | the confirmation was **undermined** — something happened that makes it untrustworthy, rather than it merely ageing out | **`compromised` is the security word for a leaked credential**, and this estate publishes `git-secrets` and `never-commit-credentials`. A reader can take it as *this document was breached* |
| **`invalidated`** | same as the first, in the verb form | the composite form `{by, at}` follows naturally from it, which is heavier than the bare-date family it would join |

**`invalid_at` is the safest of the three** on the estate's own naming
rules — consistent with `archived` and `stale_after`, both bare dates on a
thing rather than composites, and it takes no word that is already spoken for.
If *who revoked it* turns out to matter it can grow into a `{by, at}` later,
which is additive and breaks nothing.

**`compromised_at` is the more expressive one and the question underneath it is
real:** is *this confirmation expired because the world moved* the same fact as
*this confirmation was actively undermined*? If they are two facts, the answer
is a cause on one field rather than two fields — and if they are one fact,
`invalid_at` says it without borrowing a word from security.

**It must not be confused with `stale_after`, and the two are easy to conflate.**

| | subject | direction | says |
| --- | --- | --- | --- |
| `stale_after` | the **document** | forward, set in advance | recheck after this date; the content may well still be right |
| `invalid_at` | one **confirmation** | backward, set when it happens | this confirmation stopped counting at this moment |

**The normative edit it forces is small and is the real change.** §7.2 derives
trust tiers from `verified`; the derivation has to ignore invalidated events, so
a document whose only human confirmation has been invalidated falls back to
*unverified*. Without that, the tier keeps reporting *human-reviewed* for a
review that has been withdrawn, which is the same silent lie in a new place.

**The cost is zero right now, and will not be later.** Nothing reads `verified`
and no real document writes it — so changing its shape today needs no expand,
no migrate, no contract. **This is the cheapest moment this change will ever
have**, and that is an argument for settling the shape now even if nothing
builds on it for months.

### The test this has to pass, from the `compliance` removal

**Does anything *do* something differently?** That is the test that killed
`compliance` yesterday — a field nothing acts on. Applying it honestly:

- **The ledger becomes a query**, not a hand-kept checklist: every document in
  scope with no live confirmation since the change. That satisfies *hold only
  what cannot be derived*.
- **The verdict becomes a rule** needing no judgement: when two documents
  disagree, prefer the confirmed one and say plainly that the other is
  unconfirmed.
- **The quarantine becomes demotion**, not deletion — which is what
  `the-estate` requires anyway, since deletion is not the tool here.

Three consumers, each doing something concrete. **The warning is the other
direction:** `verified` today has zero readers and zero writers, so if this axis
does not land, `verified` is the next `compliance` — a specified field that
means nothing because nothing acts on it.

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
