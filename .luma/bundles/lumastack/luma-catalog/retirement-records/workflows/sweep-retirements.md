---
type: workflow
title: Sweep retirements
description: Check this repository against every retirement it has adopted — including the ones no search can find — and file what turns up. Use after adopting retirements, after they move, or before publishing anything.
---

# Sweep retirements

**A sweep that only finds words has failed.** The retirements that survive are
the ones whose vocabulary is intact, so the reading is the work and the searching
is the warm-up.

## 1. Know what you are sweeping against

Every adopted `retirement`, plus any local overlay. **Note the version** — it
goes in the receipt, and *"swept against 0.3.0"* is worth nothing once 0.5.0 is
adopted.

## 2. Run the cheap recognizers first

**`term` and `shape`, mechanically.** They cost nothing and they narrow where you
have to read carefully.

**Every hit is a question, not a verdict.** A quotation, an example of what not
to write, a different sense of the same word, and a document arguing about the
retirement itself are all correct as they stand. **Resolve each one deliberately**
— that judgement is the whole reason these are notices rather than failures.

## 3. Read for the claims — this is the actual sweep

**For each `claim` recognizer, ask what a document asserting the old idea would
look like**, and then go and look. It will not contain the retired word; if it
did, step 2 would have found it.

Where to look, in order of what it costs to be wrong:

1. **`policy` and `workflow` documents** — they bind, so a stale one teaches
   somebody to do something that cannot work
2. **anything published** — a bundle reaches every future adopter
3. **drafts and background** — cheaper, and often where the old model survives
   longest because nobody re-reads them

**Distinguish an assertion from an argument.** *"A catalog declares its
namespace"* stated as instruction is a hit. The same sentence inside a section
arguing about how namespaces should work is the record of the change, and is
correct as it stands — mark it as history *at the point of use* rather than
trusting a linked record to do it, because an unmarked quotation is how the idea
gets picked up again.

## 4. Grade by the document, not by the idea

| carried in | file it as |
| --- | --- |
| `policy`, `workflow` | a **finding** |
| `document` | a **notice** |
| a `## Version` history, or a record | nothing. Exempt |

**If the retirement's `enforced` date has passed, a notice becomes a finding.**
One comparison, and it is what gives a deadline teeth.

## 5. File findings as an audit

**Use `audit-records`.** A sweep is an audit with a lens somebody else supplied,
and that machinery already gives one file per finding, a severity, a response
from whoever is accountable, and a verification that closes it. Do not invent a
second shape for the same thing.

**Scope the audit honestly.** It examined what these retirements describe and
nothing else — say so, because silence about everything else must not read as
*examined and clean*.

## 6. If the strategy itself is wrong, say so

**A recognizer that over-reports here, or an idea with a disguise nobody
anticipated, is a finding against the strategy** — not against this repository.
File it the same way; the respondent is whoever publishes the retirement.

**Two things you may do locally, immediately, without waiting:** add an `except`
for your own documents, and add a recognizer. **You may not remove anything the
strategy declared.** Add-only is the whole safety property — you can quiet your
own noise and widen your own net, and you cannot weaken a retirement for
yourself.

*A long-lived local overlay is evidence the upstream strategy is wrong.* Report
the count rather than letting it accumulate silently.

## 7. Write the receipt

`.luma/records/retirements/swept.toml` — what you swept against, at which
version, when, and the audit if you filed one.

**Without it nobody can tell a clean repository from an unswept one**, which are
the two states this whole thing exists to distinguish.
