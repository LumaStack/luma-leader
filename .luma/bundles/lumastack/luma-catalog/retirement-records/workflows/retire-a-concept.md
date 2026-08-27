---
type: workflow
title: Retire a concept
description: Settle how far a retired idea reaches, record the decision where it was made, and publish the strategy that finds it everywhere else. Use when an idea, field, concept or word stops being how we do things.
---

# Retire a concept

**Two artifacts in two places, and conflating them is the mistake this workflow
prevents.** The *decision* stays where it was made. The *strategy* travels.

## 1. Settle the scope, before writing anything

**Which of these is it?** Answer from evidence, not instinct.

| | |
| --- | --- |
| `project` | it dies here; nothing else ever held the idea |
| `peer` | a few named projects hold it, the organization is not involved |
| `organization` | everything under one org |
| `estate` | every project, every relevant organization |
| `unknown` | **nobody has looked. Probe before choosing** |

**Probing is cheap and you should do it.** Run the term and shape recognizers
across candidate repositories — mechanical, no judgement — to answer *does this
appear anywhere else*. Put the answer in `scope_evidence`.

**Scope only widens.** Starting narrow and promoting later is supported;
withdrawing from everyone who adopted it is not. See [[retiring-a-concept]] for
which direction to err in, and it is not the same answer for a word and for a
concept.

**If it is `project` scope, stop after step 3.** Nothing is published, the record
stays in this repository, and there is no strategy to distribute.

## 2. Record the decision where it was made

**In the repository that decided**, as an ordinary `decision`. Why the old way
stopped being right, what was considered, what would re-open it.

**Do not move it here.** A decision belongs to whoever made it, and the estate
spans organizations — an id alone is ambiguous, so what travels is a citation of
the form `<org>/<repo>#<id>`.

**Not every retirement has a decision record**, and that is fine. A specification
section or a pull request is sometimes the whole account. Cite what exists.

## 3. Write the strategy

[The retirement template](../templates/retirement.md).

**It goes in this project's records** — `.luma/records/retirements/`, or wherever
records are configured to live. **Not in a bundle**: a vendored copy cannot be
edited by the project holding it, so a record placed there can never be corrected
and is reverted on the next adoption.

The parts that take thought:

**`was` and `now`, one line each.** These are the *distributed payload* — they
land in [[what-we-retired]] and are loaded into every session. A paragraph here
is a paragraph everybody pays for forever.

**`recognizers`, and at least one that is not a term.** A `term` catches the
spelling. A `claim` catches the idea, which is the thing that actually leaks. **A
retirement carrying only a `term` is usually one nobody finished thinking about**
— ask what the old idea would look like written in today's vocabulary, and record
that as the claim.

**`except`, for where a hit is correct.** Records say what was true when filed. A
`## Version` history is a changelog. And a document *arguing about* which word
should replace another cannot be restated in the word it settled on.

## 4. Publish it, and say what adopters must do

Follow `publish-to-the-catalog`. The bundle's `## Version` entry states **what an
adopter has to do**, not only what changed.

**Regenerate [[what-we-retired]]** — it is derived from the records and must
never be hand-edited.

## 5. Sweep your own repository first

Run [[sweep-retirements]] here before expecting anybody else to. **A retirement
whose own repository fails it is not ready to distribute**, and finding that out
from somebody else's audit is the expensive way.
