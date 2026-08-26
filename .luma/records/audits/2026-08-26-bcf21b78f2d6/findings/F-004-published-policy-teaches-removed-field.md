---
type: finding
title: A published policy teaches a field the format removed two releases ago
finding_id: F-004
severity: high
location: luma-catalog/catalog/bundles/bundle-manager/policy/organizing-a-bundle.md, and four design drafts
---

# F-004: A published policy teaches a field the format removed two releases ago

## Condition

**`organizing-a-bundle` is a policy in the universal catalog and names `preload`
ten times**, including a section built entirely on it:

```
### `preload` and `type` answer different questions
…
**Filing by tier is still a cost decision.** A `preload: mandatory` policy is …
```

**`preload` was removed from the format in `v0.0.12`** and its name released.
Two further releases have shipped since. Nothing reads the field; a document
declaring it gets whatever the default is and no warning.

**Four design drafts in `luma-leader` carry the same problem** at lower stakes,
because nobody adopts them: `adoption-use-cases.md` (5), `bundle-dependencies.md`
(2), `curator.md` (1), and `loading-mechanisms.md`, which is partly deliberate —
it documents the removal — and partly not.

## Criteria

**A policy binds by being a policy.** That is the estate's own position, argued
when `compliance` was removed: *"A policy binds because it is a policy — that is
what the type means."* A binding rule that instructs an author to declare a
non-existent field is instructing them to do something that cannot work.

**`bundle-manager` owns the authoring guidance**, and its sibling `create-bundle`
was corrected on 2026-08-26 for exactly this — it was telling authors to write
`compliance: recommended`, a field deleted the previous day. **The same sweep
did not reach `organizing-a-bundle`**, so this is a known class of error with a
known instance left in place.

## Cause

**The field rename swept frontmatter and missed prose.** Migration searched for
`^applies_to:` and `^preload:` — declarations — and prose naming a field in
backticks does not match. The catalog reports zero documents declaring the old
fields and is correct; the check does not look at what documents *say*.

**Nothing can catch this class.** `inspect` validates declarations; the curator
validates structure and versions. **Neither reads prose for names the format has
retired**, and there is no reason either should — which is why this needs a
person or a sweep rather than a rule.

## Effect

**An adopter following a binding policy writes a field that does nothing**, and
finds out only if they notice their document is not being delivered as they
expected. **The failure is silent in the direction the whole loading design
exists to eliminate.**

**It also misinforms about the current design in a way that is hard to unlearn.**
The section says `preload` and `type` answer different questions — a true and
useful distinction whose vocabulary no longer exists. A reader who absorbs it
carries a model of the system that stopped being real two releases ago.

**Severity is high on consequence, not on effort.** It is perhaps an hour of
prose work. What it costs unfixed is every adopter of `bundle-manager` being
taught a dead mechanism by a rule that binds them.

## Recommendation

**Rewrite the `preload` sections of `organizing-a-bundle` against the current
design** — `matches`, the three delivery outcomes, and `matches: always` as the
one route to being loaded up front. Bump the bundle.

**Sweep the four drafts in the same pass**, but rate them separately: they are
drafts, nobody adopts them, and `loading-mechanisms.md` legitimately discusses
`preload` as history.

**Then add the missing check, because this will recur on the next rename.** A
grep for retired field names in prose is a five-line check and the format
already publishes the list — `preload` and `compliance` are both recorded as
*released* in `SPEC.md`. **A name the specification says is released should not
appear in a published policy except as history.**
