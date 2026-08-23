---
type: document
title: Shared types
description: Where a type used by several tools lives, why it is not a knowledge-format built-in, and how one changes without making every tool upgrade at once.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Shared types

**Draft. Nothing here is settled.** Fourth companion to
[bundle-dependencies.md](bundle-dependencies.md),
[bundle-versioning.md](bundle-versioning.md) and [cataloger.md](cataloger.md).
Kept out of [DECISIONS.md](DECISIONS.md) on purpose.

**This is the reasoning. The operating instructions are in the `luma/luma-types`
bundle itself** — how to vendor, what to record, what breaks. This document says
why the design is shaped that way, so the arguments survive somewhere other than
commit messages.

## It mostly confirms a decision that was already made

*Shared types travel inside bundles, and a catalog publishes them* settled this
on 2026-08-17, and it holds. It rejected the same three homes on the same
grounds — **not the applier** (welds two version numbers that must move
independently), **not governance** (the layer organizations replace wholesale),
**not the format** (*"the moment the specification ships a `person` type it is
making domain claims"*). It named the residual case — cross-cutting types with
no natural owner — and said those *"earn a foundation bundle."*

`luma/luma-types` is that foundation bundle. **This document adds what that
decision could not have known**, because it was reasoning about `person` and
`decision` rather than about the types that describe the distribution machinery
itself.

## Why not a built-in, in the format's own terms

Settled in the knowledge format at `v0.0.10` and not restated here. The short
version, because it is the question that keeps returning:

- **A consumer that ignores `luma/project` is not broken.** It reads
  `.luma/project.md` as a plain `document`, which is correct and complete. *My
  tooling would break* is explicitly the wrong kind of broken — true of every
  domain type ever written.
- **A built-in's contract is versioned with the format.** These change at our
  rate, so they would drag the format's releases behind our roadmap.
- **A built-in takes a word from everyone, permanently.** `project` is claimed
  across the industry. **Importance is what a namespace is for.**

**The namespace is the whole collision story.** `luma/project` cannot be mistaken
for anybody else's `project`, so the format never has to reserve the word — which
is also why the earlier idea of reserving names without defining them was worse
than useless. It would have stopped collisions and given tooling no contract.

## The property that makes vendoring safe

**The bundle is the resolution scope.** A contract is found in *this* bundle's
`_types/`, so two bundles may hold different versions of one type and each one's
documents are checked against the copy that travelled with them.

That is the `require` scope prose does not have, and it is the reason
[bundle-dependencies.md](bundle-dependencies.md)'s one-version rule does **not**
extend to types. Two versions of a policy in one context window are contradictory
instructions to one reader; two versions of a type are two contracts in two
scopes.

**bundle-dependencies overreaches on this point and is worth correcting.** It
argues *"type definitions make this sharper rather than softer… a record written
against a schema that no longer exists is silently wrong."* True about records —
and scoped to the same place the resolution is. Bundle B moving to v2 does not
invalidate records inside bundle A, because A's records were never checked
against B's copy.

### Except for documents that live outside every bundle

**And those are exactly the two types worth sharing.** `.luma/project.md` is one
file declaring one `type: luma/project`, and it is inside no bundle — so there is
no scope, and nothing decides between two contracts claiming it.

The types most likely to be shared describe **the containers bundles live in**
rather than living in one. That is the hard case, and it is not a coincidence.

**Those need one answer per project**, and where that answer lives is the
`.luma/_types/` question below.

## Changing a shared type

**A breaking change is never one release. It is three**, and the property that
matters is that every intermediate state is valid for both old and new readers:

| | ships | who must have upgraded |
| --- | --- | --- |
| **expand** | the new field is added; the old one stays. Both valid | nobody |
| **migrate** | documents move to the new field | tools reading the new field |
| **contract** | the old field is removed. **The breaking release** | everyone, and by now everyone has |

**There is never a moment when two tools must ship together.** That is the whole
point, and it is what answers *do we force all the tools to upgrade at once* —
no, and needing to would mean somebody skipped *expand*.

**Raising an obligation is a contract step wearing an expand step's clothes.**
Strengthening `optional` → `mandatory` makes every document lacking the field
non-compliant the moment it ships. Migrate first, strengthen last.

**Tools are field-tolerant, not version-aware — and have no choice.** A document
never records which type version it was written against, so version dispatch is
impossible. *Read the new field, fall back to the old one* is the entire
technique, and §4's tolerance makes it free.

**Two hops, one of them cheap.** Updating a vendored copy is mechanical, per
bundle. Migrating records already written happens **once per project**, because
the records are the project's — however many bundles vendored the type.

## What it costs across projects

Documents inside bundles are safe. **The exposure is aggregation of out-of-bundle
documents**: a repository index reading every project's descriptor while half
declare the old field and half the new. Every file parses, nothing errors, and
**the aggregate is silently incomplete**.

The `luma/project` type already says a collector is *"combining, not authoring."*
Combining heterogeneous records is where that stops being safe, so a collector
should read the version each project declares and say so rather than presenting a
mixed set as uniform.

## What earns a place in the foundation bundle

**Three consumers, or a deliberate decision to share.** A type starts in the
project that needed it, where being wrong about its shape costs one team an
afternoon.

**The exemption is the one the promotion recommendation already grants:**
something built explicitly to be shared does not have to prove itself through
three projects first. `luma/catalog` was promoted on that basis rather than on a
count — waiting for a first outside user before centralising the type that
*defines a catalog* would produce copies to reconcile, not evidence.

**Importance is not the bar.** A type serving one tool belongs to that tool,
however load-bearing it is there.

## Open

- **Per-type versions.** A Type Definition has no version of its own, so changing
  `luma/project` bumps `luma/catalog` too. This is the pressure that makes a
  separate repository look attractive, and a separate repository would not fix
  it either. Already an open item in the format.
- **`.luma/_types/`.** Where a project keeps the single contract governing its
  out-of-bundle documents. Needed twice over — once so resolution has an answer,
  and again so an aggregator can ask which version a project is on. A layout
  question rather than a format one.
- **Whether a bare type name should ever be legal for a shared type.** Today
  namespacing is `SHOULD`. If a project declares `type: project` meaning ours,
  nothing objects and nothing resolves.
- **What a cataloger does about mixed versions**, given the check is *report
  always, fail only for the out-of-bundle set* — narrower than a blanket conflict
  rule, and unimplemented.
