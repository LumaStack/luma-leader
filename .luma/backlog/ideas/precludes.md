---
type: luma/idea
title: A bundle may declare that another bundle cannot be loaded beside it
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
stage: draft
---

# `precludes:`

## The idea, as raised

**Catalog and bundle resolution should possibly support `precludes:`** — which
says that when loading this bundle, another bundle cannot be loaded. **They will
break each other, this is known, and we are letting you know.**

**Bundles preclude bundles, and catalogs preclude catalogs.** The two scopes
match; a bundle does not get to preclude a whole catalog.

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it. Evaluation is welcome here — it just does
not get to edit what was raised.*

### It is the first mechanical answer to a gap that was recorded as having none

`adoption-use-cases.md` names two cases nothing addresses: two catalogs solving
one problem differently, and two catalogs where one's guidance is the other's
anti-pattern. Both end *both are in force, the agent picks, nothing arbitrates.*
`bundle-dependencies.md` reaches the same place in one line under *Open* and
calls the answer *"arbitration between catalogs, which does not exist."*

**This is that arbitration, for the subset where somebody already knows.**

### It does not compete with `governs`, it covers the opposite half

The only other proposal for this gap is a declarative `governs:` — a bundle
naming the subject it claims authority over, so two bundles claiming
`git-workflow` collide mechanically.

| | catches | needs |
| --- | --- | --- |
| **`precludes`** | **known** enemies, exactly | knowing the other exists |
| **`governs`** | **unknown** collisions between strangers | a controlled vocabulary, which drifts |

**`precludes` is much cheaper and makes no claim it cannot back.** It is a list
of identifiers. `governs` needs a published vocabulary and fails silently when
one author writes `git-workflow` and another writes `git-workflows` — the exact
hazard the tag vocabulary already carries.

**Build `precludes` first if either is built.** It is honest about being partial
where `governs` is partial and looks total.

### It does not break *bundles depend on nothing*, and that will be the first objection

**A dependency changes what you acquire. A preclusion changes nothing you
acquire.** It is a constraint on the set rather than an addition to it.

A `precludes` entry naming a bundle nobody has is **inert** — no lookup, no
fetch, no resolution. §2 of the format defines self-containment as *nothing is
fetched in order to read it*, and this fetches nothing. Hand somebody the
directory and it still works exactly as before.

**A dangling preclusion is silence, not an error**, the same as a wikilink to a
document nobody has written. §4 forbids rejecting a document over something the
consumer cannot resolve.

### Asymmetric declaration, symmetric effect

**Whoever knows about the conflict declares it, and the other side never has
to.** The check is a union over everything adopted: does any adopted bundle name
any other adopted bundle?

That is what makes the fork case work. **A fork declares that it precludes its
upstream, and upstream never learns the fork exists** — which it could not, and
should not have to.

### Refuse rather than warn

The idea is phrased as a warning — *we are letting you know*. **Refusing is
better, and it is the same shape already chosen twice**: fail by default, allow
an override, require the override to be written into committed config.

**The difference from an accidental collision is what decides it.** A namespace
collision is two catalogs that happened to pick one word. A preclusion was
*declared by somebody who thought about it*. Refusing honours their judgement;
warning second-guesses it while still letting the contradiction into context,
which is the outcome the whole thing exists to prevent.

The override case is real and narrow — migrating from one to the other, and
briefly holding both.

### Catalog-level is not the same mechanism at a larger size

**It buys one thing a bundle list cannot: coverage of content that does not
exist yet.** A bundle-level list is a snapshot and goes stale the moment either
side adds a bundle. A catalog precluding a catalog is a standing statement that
covers everything published there afterwards — which is exactly right for a
fork, where the divergence is wholesale and enumerating bundles is both noise
and a losing race.

**And it costs the thing coarse mechanisms always cost.** It blocks harmless
bundles along with conflicting ones. That is the same objection that sank the
catalog snapshot on the pinning axis, and it is weaker here only because a fork
genuinely is all-or-nothing.

**It merges rather than inherits, and that falls out of an existing decision.**
*Catalogs do not inherit; only starters do* sets the rule: merge additively
where more is safe, require explicit inheritance where subtracting is
legitimate. A preclusion is more restrictive, so the union of every preclusion
in a chain applies and no `extends` is needed. It joins `requires` and `tags` on
the merging side.

**Preclusion is not transitive.** If C precludes the universal catalog, and
Acme's catalog names the universal one as `upstream`, C does not thereby
preclude Acme's. Inheriting a preclusion would let one declaration take out an
arbitrary subtree.

**The scopes match deliberately, and the reason is worth keeping.** A bundle
precluding an entire catalog would let one document block a whole source — the
blast radius of a claim should match the standing of whoever makes it.

### Two risks, and one of them has a known remedy

**A preclusion is a claim about something you do not control, which makes it
usable as a weapon.** Nothing distinguishes *these genuinely contradict* from *I
would rather you did not use theirs*.

**The remedy already exists in the estate and should be copied verbatim: state a
reason.** `bundle-dependencies.md` requires one on a narrow version constraint
because *the cost of a tight constraint falls on strangers*, and refuses
publication without it. Identical argument. `precludes: [acme/other]` is
indistinguishable from territorial behaviour; *"precludes `acme/rebase-workflow`:
both define the merge strategy, and an agent holding each gets contradictory
instructions"* is a sentence somebody stands behind.

**Stale preclusions rot invisibly, and this one has no remedy yet.** A claims B
breaks it, B fixes the incompatibility, and nothing ever re-checks. A now blocks
something harmless and nobody finds out, because the failure mode of a
preclusion is *an adoption that did not happen* — which leaves no trace anywhere.
It is a rot class with no detector, and worse than a stale version pin, which at
least surfaces as an outdated number.

### Bundle-level only, never document-level

Precluding a specific document inside another bundle reaches into its internals
by path, so the author renames a file and the preclusion silently stops
applying. **That failure has now been the reason to reject three separate
proposals** — per-document overrides of what loads, a depending bundle raising a
document of its dependency, and this. Worth noticing it is the same shape every
time.

### What would settle whether to build it

**A real pair.** The motivating example that produced this idea —
`lumastack/luma-catalog/luma-tools` and `lumastack/luma-catalog/luma-maintainers` — turns out **not** to need it: a
maintainer is also a consumer, and the maintainer content is additive rather
than contradictory. That is useful evidence rather than a disappointment.

The same test that is holding bundle dependencies back applies here: **name two
bundles that genuinely break each other.** The candidates are two competing
git-workflow bundles, and a fork held beside its upstream. Neither exists today.

## Related

[[bundle-routines]] — the other proposal about what a bundle declares about
itself, and the same question of what belongs in a manifest.
`adoption-use-cases.md` UC13 and UC14 — the cases this answers.
`catalog-namespaces.md` — where fail-by-default-with-a-written-override was
first chosen, and the pattern this reuses.
