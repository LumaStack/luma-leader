---
type: document
title: Bundle dependencies
description: How a bundle depending on another bundle should work — flat resolution, one version per project, and why context rather than convenience decides it.
stage: draft
created: { by: human:benlinton, at: 2026-08-21T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Bundle dependencies

**Draft. Nothing here is settled.** This is a design worked out in one sitting
and written down so it can be argued with rather than re-derived. It is not in
`DECISIONS.md` on purpose: if it were, its claims would read as positions the
organization has taken, and they are not yet.

**What it would change if adopted.** Bundles are currently self-contained and
depend on nothing, and that is the stated reason there is no solver, no version
negotiation, and no dependency graph. This proposes that bundles may depend on
other bundles.

**The reason is narrow and does not generalize.** For **prose**, the alternative
to a dependency is duplication — and two copies of a policy drift, then
contradict each other, with nothing recording which is current. That is worse
than the resolution problem. For code the same trade goes the other way, which is
why this is not an argument that bundles should behave like packages.

## Flat, one version, no nesting

**Context is a single namespace with no scoping mechanism, and that decides the
whole design.** A package manager can nest dependencies because two versions of a library are
two objects with separate call sites. Two versions of a policy in one context
window are contradictory instructions to one reader. There is no `require` scope
for prose, so nesting is not merely undesirable — it has no meaning.

So: **one version of any bundle in a project, ever.** Dependencies sit side by
side. Nothing contains anything.

**Type definitions make this sharper rather than softer** — *for records, and
only within one bundle.* If two bundles want different field sets for the same
type, records have already been written to disk against one of them. Prose read
under the wrong version can at least be reasoned about; a record written against
a schema that no longer exists is silently wrong and cannot be failed out of
after the fact.

> *Corrected 2026-08-23.* This originally read as an argument that **types make
> the one-version rule apply more strongly than prose does**, and that is wrong.
> **The bundle is the resolution scope**: a contract is found in *this* bundle's
> `_types/`, so two bundles may hold different versions of one type and each
> one's documents are checked against the copy that travelled with them — which
> is the `require` scope prose does not have. Bundle B moving to a new version
> does not invalidate records inside bundle A. The danger above is real and it is
> scoped to the same place the resolution is.
>
> **The exception is a document that lives outside every bundle** — it has no
> scope, so nothing decides between two contracts claiming it. See
> [shared-types.md](shared-types.md).

## A dependency is transitive adoption, and nothing more

With no nesting, no scoping and one version, *A depends on B* is operationally
identical to **adopting A also adopts B**. There is no second mechanism, no
separate dependency store, no link step.

The manifest records, per entry, whether it is there because someone asked for it
or because something they asked for required it. That flag exists for one
purpose: **removal**. Drop A and B goes with it, unless B was asked for directly
or something else still requires it.

## Resolution takes the current version, and fails only across majors

**Current, always.** An old policy is a superseded policy — somebody edited it
because the previous rules were wrong, so deliberately resolving backwards means
running rules that were replaced on purpose. The conservatism that makes
minimal-version selection right for code is wrong here, because for code an old
version still works and for policy it does not.

**A big difference is the one hard failure.** A bundle typically states the major it is
compatible with; two requirements naming different majors cannot both be
satisfied, and no arrangement rescues it, because nothing can nest. There is no
resolution step beyond this: no candidate selection, therefore no backtracking,
therefore no solver.

**What earns a major is defined separately** — see
[bundle-versioning.md](bundle-versioning.md). Resolution keys on the major line, so
that document decides where this one's only hard failure falls.

## A constraint may be as tight as its author wants; the norm is loose (scoped to major version)

**Anyone may express a dependency however they need to** — from "any version of
this major" down to an exact patch. What differs is the **default**, and what a
tight constraint costs whoever else is nearby.

**The norm is the whole major line, and defaults to it.** Depending on `2` means
accepting every `2.x.x` — every minor and every patch, without review.

**Not because minor and patch carry no information.** They carry a great deal: a
minor addition to a policy can change what an agent does more than a restructure
would. The reason to take them unread is simpler. **For context, skills and policy
we want the newer thing.** A newer policy is the correct policy, because somebody
replaced the old one deliberately.

**This is not dependency management protecting software from breaking itself. It
is protecting a system from running on old ideas.** Those two want opposite
defaults. A package manager's caution is right when an old version still works,
and wrong when an old version is a rule somebody has already decided against.

**It breaks down in exactly one place: where rules are rigid and changing them
needs oversight.** Compliance, legal, anything governed. There *the newest is the
best* is false by construction — what is correct is what was reviewed and
approved, and an unreviewed improvement is still unapproved. That is what a
tighter constraint exists for.

**Two different needs wear the same syntax, and they must not be confused.**

| | what it claims | who is affected |
|---|---|---|
| **a bundle constraining its dependency** | *I am compatible with this* — a correctness claim | everyone who adopts it |
| **a project pinning what it adopted** | *I do not move without review* — a change-control policy | only that project |

**The second is the compliance case, and it is already largely satisfied by
vendoring.** Content is committed, so nothing moves until somebody runs an update
and commits the result. A compliance team is protected by the architecture before
any pin is written. What a pin adds is refusal — *do not even offer me a newer
one, and fail if something tries to pull one in* — which is a legitimate thing to
want and costs nobody else anything, because it lives in that project's own
manifest.

**The first is where tightness has a price.** A bundle pinning its dependency
narrowly constrains every other bundle that shares that dependency. This would be
permitted, with the catalog entitled to reject a bundle whose constraint cannot
coexist with what it already holds. **A tight constraint is a claim its author has
to be willing to defend**, because the cost of it falls on people who never chose
it.

**Different teams should reach different answers, and that is correct rather than
a failure of standardization.** An engineering team wants the newest policy,
because for prose the newest is usually the best and often the only one worth
caring about. A legal or compliance team wants nothing to change without review.
Both are right about their own situation, and the constraint syntax is where that
difference gets stated instead of argued.

### A narrow constraint has to say why

**A bundle constraining more narrowly than a major line states a reason, and
publication rejects it otherwise.** One field, one sentence, and only on the
narrow case.

The rule exists because **the cost of a tight constraint falls on strangers.** An
author pinning to a patch is spending other people's flexibility: every bundle
sharing that dependency is now constrained by a decision none of them made, and
the person who eventually hits the publication failure has to work out whose
caution caused it. Nothing about a version expression announces that it was
deliberate rather than copied from an example, and a default nobody notices
violating is not a default.

This is the same move as **reporting what a tag change costs**, and as **making an
exemption a sentence rather than a pattern**: it does not forbid the thing, it
makes doing it quietly impossible. *"Pinned to `2.1.4`: our regulator requires
policy changes to be reviewed before adoption"* is a sentence somebody stands
behind. `2.1.4` on its own is indistinguishable from carelessness.

**It is asked of bundles, not of projects.** A project pinning its own adoption
affects nobody else and owes nobody an explanation; the asymmetry follows the
table above and is the whole point of separating the two cases.

**The catalog would report its own tightness.** A catalog doctor listing every
constraint narrower than a major line with its stated reason. Individual pins are
defensible and a catalog drifting toward tightness is a problem nobody would
otherwise see, because each one was reasonable on its own.

**What this does not solve:** a required reason catches carelessness, not bad
judgement. Somebody who believes their pin is necessary writes a sentence and
publishes. What stops that is a reviewer disagreeing with it, which is a
catalog-editor job rather than a mechanism.

## The conflict check runs at publication *and* at installation

**Both, and they are not the same check.**

**At publication**, because that is where the only person who can fix it is
standing. A catalog is curated rather than open, so it can refuse a bundle whose
requirements conflict with what it already holds. The author is present, owns the
bundle, and can change it. Letting the conflict through means it surfaces later in
front of an adopter who owns neither bundle and whose only recourse is forking —
which is a worse position than a package-manager user is in, since they at least
can nest in a typical package manager tool (not here).

**At installation**, because publication-time consistency is a property of *one
catalog at one moment*, and a project is not that. A project adopts across an
organization's catalog and the universal one, from forks, and from its own local
bundles — combinations no single publisher ever saw. Catalogs are forkable and
vendored copies are editable. **A check that runs only where you control both ends
stops running the moment somebody forks**, which is the same fail-open shape this
design rejects everywhere else.

So publication catches it early and cheaply; installation catches what
publication structurally cannot see. Neither makes the other redundant.

## Recommended: keep a bundle in the project until it earns a catalog

**A recommendation, not a rule**, because the cases where it is wrong are real and
the author usually knows which one they are in.

**A bundle starts life in the project that needed it.** That is where the work is,
where it will be edited most, and where being wrong about its shape costs one team
an afternoon. A project's own content becoming a small local bundle already costs
about four lines of manifest, and promotion is a directory copy — so starting local
forecloses nothing.

**Promote it when one of two things becomes true:**

- **several projects are using it** — the copies are real, not anticipated
- **it is foundational** — core, high-traffic, the thing other content leans on

**Dependencies make this matter more than it used to.** Publishing early does not
merely make a bundle available; it creates a surface other bundles can depend on.
A catalog entry is a commitment. A project bundle is a draft. Once something
depends on you, changing shape stops being free and starts breaking strangers —
and under flat resolution those strangers cannot pin their way around it without
spending everyone else's flexibility.

**The cost of being wrong is asymmetric, which is why the recommendation leans
late.** Promoting too late costs a duplicated copy or two, which is visible,
annoying and fixable. Promoting too early costs every adopter who built on a shape
that had not settled, which is none of those things.

**Two copies are a signal, not yet a problem.** The same content appearing in a
second project is evidence worth noticing; it is usually the third that says the
thing is genuinely shared rather than coincidentally similar.

**When to ignore this.** Something built explicitly to be shared, or a standard an
organization has already decided to adopt everywhere, does not need to prove
itself through three projects first. The recommendation exists to stop *incidental*
content becoming a public commitment by accident — not to make deliberate
standards wait.

## What enters context is unchanged by who pulled it in

**A depending bundle does not decide what loads.** *Preload is declared by whoever
holds the knowledge* — a settled decision in [DECISIONS.md](DECISIONS.md), whose
title keeps a field the format has since released; the position is about delivery
and still holds — already covers this and needs no amendment: the bundle author declares each document's
`matches`, and the adopter decides a bundle-level default outright. A
dependency changes what is *available*; it does not change what is *present*.

**Adoption decides availability. Routing decides presence.** That is what keeps
dependencies affordable — a bundle adopted but not relevant to the work in hand
costs disk rather than context. The fixed cost of an adoption is only its
genuinely unconditional content, and a bundle with a large unconditional
footprint has a defect that is visible as a number.

**Adoption would report its transitive context cost before it is accepted.**
*"Adopting A also brings B and C; four documents load up front."* No package
manager reports this because no package manager has a budget to spend. Without it,
a dependency chain is a permanent context tax that nobody chose and nobody sees.
`matches` is declarative, so the number is computable before anything is loaded.

## Compatibility is recorded, not predicted

A version range is a claim about content that does not exist yet, made by
somebody who cannot know it. Instead, a bundle records what it was **verified
against** — a fact about work someone did. Running a different version than the
recorded one produces a notice rather than a failure, silenceable permanently by
anyone who checks the combination and records it.

**A version number says how much changed. It cannot say whether you still fit.**
Those are different questions, and for code the first stands in for the second
because a library has an interface — if the signature held, the caller still
works. A policy has no interface; its content is its effect. So a version tells
you a real thing and still cannot answer the only question that matters between
two bundles.

That is what makes recording better than predicting here. Compatibility
accumulates because somebody confirmed it, not because a number implied something
it was never able to say.

**Cross-bundle links would be checked at resolve time.** If a document in A links
to a document in B that is not present at the resolved version, that is a finding.
For code a broken assumption fails a test; for prose nothing happens at all — an
agent follows a dangling link, finds nothing, and proceeds confidently. This check
is the substitute for a compile step, and without it a recorded compatibility
claim is a promise nobody verifies.

## If this were built

*The whole of this section assumes per-bundle constraint resolution. A proposal below — **resolve at a catalog snapshot** — would replace most of it with a lookup, and is worth reading before building any of this.*

- Resolution order: collect requirements transitively; fail if constraints cannot
  be satisfied jointly, naming every requirer and what each asked for; otherwise
  take the current version satisfying all of them. In the ordinary case every
  constraint is a major line and this is one lookup.
- Default a dependency to the major line. A bundle constraining more narrowly
  carries a stated reason, and publication rejects it without one. Projects owe no
  reason for their own pins.
- Run that check at publication and again at installation, with the same code and
  different inputs.
- Report the transitive set and its unconditional context cost before writing
  anything.
- Record in the manifest, per entry: version, and asked-for versus required-by.
- `stage` already carries readiness. Experimental is not a version
  question and must not become one.

## Proposal for consideration: resolve at a catalog snapshot

**Raised 2026-08-23. Not well thought out yet, and recorded so it can be argued
with rather than re-derived.** It is an alternative to the whole resolution
scheme above, not an adjustment to it.

### The proposal, as raised

**There is nothing that says you need the entire catalog.** You could say *I want
`git-secrets`*, and it pulls everything in the catalog **at version 1.2.0** that
is referenced by `git-secrets`.

So a catalog version becomes a **coordinate for resolution** rather than a unit
of adoption. You still take only what you need; the version says *as of when*.

**The weakness, as raised:** *"you might get unintended changes that you didn't
want because you're at a new catalog version. But I think that could happen with
bundles as well. It's just that the chaining could be more aggressive. I don't
know — maybe I'm overthinking this."*

### What it would solve

- **The solver disappears.** Resolution becomes a lookup rather than a
  constraint problem. No candidate selection, no joint-satisfiability check, no
  backtracking — the publisher already made the snapshot consistent, so **any
  subset of one snapshot is consistent by construction.**
- **Most of the constraint machinery becomes unnecessary.** Version expressions,
  the required reason on a narrow constraint, the catalog doctor reporting
  tightness — much of that exists to reconstruct at adoption time a guarantee the
  catalog could have made once, at publication.
- **The publication check gains a unit.** *This snapshot was verified* is a
  clearer claim than *these entries were pairwise satisfiable when each was
  published.*
- **Reproducibility is one coordinate**, not a set of version numbers to compare.
- **Vendored copies inside the catalog** — a bundle carrying a type whose
  canonical form lives in another bundle — are checked once per snapshot rather
  than trusted at every adoption.

### What it would introduce

- **Unintended changes, which is the weakness raised above.** Moving to a newer
  snapshot to get one thing brings everything else forward with it. **The same
  hazard exists with per-bundle versions and the degree is different**: there you
  move one thing deliberately, here the blast radius is whatever the closure
  contains.
- **More aggressive chaining.** A transitive closure resolved at a snapshot may
  pull in more than a per-bundle resolution would, and all of it moves together.
- **Upgrades stop being local.** If two closures share a bundle, upgrading either
  means re-resolving both at one newer snapshot — otherwise the project holds two
  versions of one bundle, which flat resolution forbids. That is ordinary for a
  lockfile and it is still a cost.
- **No answer across catalogs, and a misleading appearance of one.** Two catalogs
  means two snapshots and no joint guarantee. Unsolved either way, but a
  coordinate makes it *look* solved.
- **It needs snapshots to exist.** Somebody has to publish them deliberately, and
  today nothing does. Publication is an event now — merging to a catalog's
  `main`, see [curator.md](curator.md) — but it produces no snapshot, so this
  still has nothing to resolve against.
- **It is coarse.** There is no way to say *this bundle at 2.1, everything else
  current*, which a per-bundle scheme gives for free.
- **It edges toward the thing the naming decision warned about.** Resolving
  against a coordinate starts to resemble a resolver, and *the catalog is a
  catalog, not a registry* tracks the mechanism rather than the word.

### The objection this raises first: does the catalog become the real bundle?

**Raised as:** *it feels like in this approach, catalogs are the true bundles,
and bundles are submodules used for grouping knowledge.*

**Unit of distribution and unit of resolution do not have to be the same
thing.** That is the whole answer, and it is worth stating before the objection
gets made a second time.

**What lands on disk is unchanged.** Individual bundle directories in
`.luma/bundles/`, each self-contained, each moveable, each readable offline with
nothing fetched. That is precisely what §2 of the knowledge format means by *unit
of distribution*, and a snapshot coordinate does not touch it. What changes is
only **how you decided which bundles and which versions** — which is resolution.

The same split exists elsewhere and nobody finds it strange: a package is the
unit of distribution and a lockfile is the unit of resolution. Packages did not
become submodules when lockfiles were invented.

**And the version half of the worry is already answered here.** The instinct
behind *bundles become mere groupings* is that the compatibility guarantee moves
up to the catalog, leaving bundle versions as labels. But
[bundle-versioning.md](bundle-versioning.md) already assigns them exactly that
job: **"the version ladder is how a change is communicated, not how compatibility
is guaranteed."** Compatibility rests on a recorded verification rather than on
the number, with or without this proposal. Nothing is demoted.

**Where the objection lands, and it is the honest cost.** Today a catalog is a
*place you copy from*, and a bundle you have copied has no further relationship
to it — hand somebody the directory and they are finished. Under this proposal
the directory still works standalone, but **re-resolving requires the catalog and
a coordinate into it.** The catalog stops being purely a place and acquires
states.

That is a real increase in coupling and it is the thing *the catalog is a
catalog, not a registry* watches for. Its re-open trigger is remote resolution —
*"an index tools query at apply time"* — and this stays the right side of that
line, because you resolve once, vendor, and never look again. **But it is closer
to the line than today**, and that is worth knowing rather than discovering.

### It may already exist, and be called a tag

*Catalog at 1.2.0* and *catalog at commit `abc123`* are the same coordinate; a
version is the human-readable alias. **The settled decision already endorses
this** — *"tag it — a tag labels a snapshot without implying compatibility, which
is the weaker and more honest claim"* — and the section below adds that under a
publication check **a tag does imply mutual satisfiability**.

So this may need no version scheme at all: an `adopt --at <tag>` and a commit
recorded in the project's manifest, which records nothing of the kind today.

### What would settle it

**Whether bundles gain dependencies at all.** Today nothing references anything,
so the closure of `git-secrets` is `{git-secrets}` and the coordinate carries no
information. **This is worth nothing until that draft is adopted, and worth
comparing carefully against the resolution above if it ever is.**

## Alternatives considered

**Forbidding constraints narrower than a major.** It would make the deadlock
between two tightly-pinned bundles structurally impossible, at the cost of the
compliance case — an organization that must not accept an unreviewed policy change
would have no way to say so. The deadlock it prevents is caught at publication
anyway, where somebody can act on it. *Revisit if tight constraints in published
bundles become common enough that publication failures block ordinary work.*

**A second kind of dependency** distinguishing *I need its content* from *I accept
its rules*. It dissolves on inspection: obligation is declared by a catalog's
mandate, not by a dependency, and what loads is derived from `matches`. A dependency
that also bound you would be an obligation wearing a different name. *Revisit if a
real bundle pair needs a distinction these two mechanisms cannot express.*

**A depending bundle raising a specific document of its dependency into context.**
It reaches into another bundle's internals by path, which breaks silently on
rename. When it bites, the honest diagnosis is usually that the dependency's own
`matches` is wrong — a fix that helps every adopter rather than one. *Revisit when a
real pair demonstrates otherwise.*

## The catalog still does not need a version of its own

**Answered here because this design is what raises the question.** *Bundles are
versioned; the catalog is not* carries a re-open trigger: *"if bundles ever gain
dependencies on each other, entries do need co-guarantees and a catalog version
starts meaning something."* This proposal gives entries exactly that co-guarantee,
so the trigger fires. The conclusion survives anyway, on a different argument than
the one it was originally given.

**The original reason is now void.** It said a catalog version would convey
nothing because entries had no dependencies and therefore no guarantee to make
about each other. Under this design they do.

**But every commit of the catalog is already a consistent set.** If the check runs
at publication, any commit that exists is one where every bundle's constraints were
jointly satisfiable. A version number would be a second and coarser name for
something a commit identifies exactly.

**And nothing takes a catalog version as an input.** Adoption is per bundle, taking
current. There is no operation whose argument is *the catalog at v5*, so the number
would exist to be read rather than used.

**Reproducibility is already held closer to the work.** A project's manifest
records the full resolved set including transitive entries — which is better than a
catalog version, because it records what was actually taken rather than what
happened to be available.

**The existing answer gets stronger rather than weaker.** That decision says to
*tag* a snapshot where a mandate needs a stable referent, calling a tag "the weaker
and more honest claim" because it labels a snapshot without implying compatibility.
Under a publication consistency check, **a tag now does imply mutual
satisfiability**. The mechanism chosen as the humbler option turns out to be the
right one at full strength, with nothing to change.

**Why the distribution analogy does not carry.** Debian versions its releases
because packages have deep dependency graphs and must coexist at runtime on one
machine, where a bad combination is found by a crash. This is flat, shallow,
vendored, and current-is-best, and a bad combination is found at adoption with a
person present. The conditions that make a release number valuable are absent.

## Open, and unresolved here

- **Which word a dependency uses.** `requires:` is taken — on a catalog it means
  *obligation*, which bundles a project must adopt and how strongly. A
  bundle-to-bundle dependency is a different relationship and needs a different
  name, or the two get confused in exactly the place confusing them is most
  expensive. `depends` is free.
- **What the publication check was validated against.** The rules that check
  enforces will change — requiring a reason on a narrow constraint is already one
  such rule. A catalog published before a rule existed was never held to it, and
  nothing records which rules it passed. That is a **conformance** version rather
  than a content version, and versioning the catalog would not answer it.
- **Cycles.** A depends on B depends on A. Harmless for content, pathological for
  anyone reasoning about it, and presumably detected and rejected. Unspecified
  because none can exist yet.
- **Cross-catalog conflict.** Curation makes a catalog internally consistent, but a
  project adopting from two catalogs can still reach a combination neither
  publisher saw. No escape hatch is proposed. If it turns out to be common, the
  answer is arbitration between catalogs, which does not exist.
- **Budget enforcement.** Reporting the context cost is proposed; refusing an
  adoption that exceeds a ceiling is not. That becomes worth having only once
  bundles routinely carry large unconditional content.

## Where the reasoning behind this lives

Much of the argument that produced this was worked out in conversation and
survives only in commit messages on `luma-leader`, `luma-catalog`,
`luma-knowledge-format` and `luma-foreman` dated 2026-08-21 and 2026-08-22 — in
particular why minimal-version selection is wrong for policy, why semantic
versioning carries little signal for prose, and the route from *do we need
versions at all* to *versions are good enough*. **That is a known weakness of this
document**, recorded so it can be fixed rather than rediscovered.
