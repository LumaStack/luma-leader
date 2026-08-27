---
type: bundle
version: 0.1.0
published: 2026-08-27
consumers: [project, organization]
entry_point: policy/retiring-a-concept
description: Retiring an idea across many projects — the decision that stays home, the strategy that travels, and the recognizers that find a concept whose vocabulary survived.
---

# Retirement records

A retired idea comes back by being **reinvented, not remembered**. The ideas
worth retiring are the obvious ones for the job, so removing every mention is a
weak defence — an author reaches for one again and it reads as a fresh choice.
That has been observed happening within minutes of a sweep that removed one.

This bundle exists because the defence has to run in two directions at once.

## What is here

- [[retiring-a-concept]] — the policy. What may be retired, how far it reaches,
  and why a word is the cheapest recognizer rather than the important one.
- [[what-we-retired]] — **always loaded.** Every retired idea, one line each.
  Generated from the records; never edited by hand.
- [[retire-a-concept]] — settle the scope, record the decision at home, publish
  the strategy.
- [[sweep-retirements]] — check this repository against everything it adopted,
  including what no search can find, and file what turns up.
- [[release-a-retirement]] — decide an idea no longer needs watching.
- **`retirements/`** — the estate's six retirements, as records. This is the
  content the bundle exists to distribute; everything above is machinery.
- [the retirement template](templates/retirement.md)

**A bundle carrying instances is unusual here** — `audit-records` and
`decision-records` ship the machinery and leave the records in each project. The
difference is that a retirement has to *travel*: a strategy nobody else holds
cannot be swept against. Directories group and the `type` identifies, so
`retirements/` needs no new mechanism.

## The four ideas worth knowing before reading further

**A word is the floor, not the product.** Three recognizer tiers — `term`,
`shape`, `claim`. The first two are deterministic and cost nothing. **The third
needs a reader**, and it is the one that matters: *a catalog **declares** its
namespace* became *it **derives** from where it lives*, and the word "namespace"
is in both. No search finds that.

**The decision stays home; the strategy travels.** A `decision` says why we
changed our mind and is heading toward being locked. A `retirement` says what is
dead and how to spot it, and **must stay revisable for as long as it binds** —
new disguises turn up for years. Fusing them would force an amendment to a
settled position every time a sweep taught you something. The join is
`decided_in`, a citation of the form `<org>/<repo>#<id>`, because an id alone is
ambiguous once the estate spans organizations.

**Detection runs after the fact and cannot be the whole defence.** A config file
is read by a checker and shown to nobody; the author has never seen it. That is
why [[what-we-retired]] declares `matches: always` and is the only document here
loaded before work starts — **it is the half that meets an author before they
write, rather than after.**

**Scope is decided first, from evidence, and only ever widens.** `unknown` is a
real state: probe with the cheap recognizers rather than guessing. Err wide on a
concept and narrow on a common word — the two mistakes are not symmetric, and
retiring ordinary English produces noise that teaches readers to skim past
notices, which disables the check everywhere.

## Consumers

Both levels. A project retires its own ideas and sweeps against what it adopted;
an organization hands retirements down and needs to know which projects complied.

## Version

`0.1.0` — first release.

**It carries this catalog's first `matches: always` document**, and that is worth
stating rather than slipping in. Nineteen bundles have declared none, and the
argument for this one is specific: the failure it addresses happens *before* an
author writes, so nothing reachable only on demand can address it.
[[what-we-retired]] is capped at one line per retirement for that reason — it is
a list, and the reasoning lives elsewhere.

**`field_type: timestamp` is a new name in an open vocabulary** (§10.2), declared
on `retired_at`. The estate's date fields cannot order two renames that landed in
the same afternoon, and this week they did. `enforced` stays a plain date,
because nobody complies at 14:32.

**What an adopter has to do:** run [[sweep-retirements]] once after adopting, and
write the receipt. Until that exists there is no way to tell a clean repository
from an unswept one, which are the two states this bundle exists to distinguish.
