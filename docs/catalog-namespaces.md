---
type: document
title: Catalog namespaces
description: The prefix a catalog publishes its bundles under — why it is a name rather than an identity, when two adoptions collide, and why catalogs do not survive to session time.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-23T00:00:00Z }
---

# Catalog namespaces

> **Partly settled since, on 2026-08-26 — read this first.** A catalog no
> longer *declares* its namespace. It **derives from where the catalog lives**,
> so `github.com/LumaStack/luma-catalog` publishes
> `lumastack/luma-catalog/<name>`, and a fork gets its own namespace without
> anybody arranging it; a `CATALOG.md` declaration still wins where one exists.
> **The first line of *The model, in four lines* below is superseded by that.**
> The rest — collisions, consumer override, why catalogs do not survive to
> session time — was not decided and still reads as draft.

**Draft. Nothing here is settled.** Sixth companion to
[bundle-dependencies.md](bundle-dependencies.md),
[bundle-versioning.md](bundle-versioning.md),
[shared-types.md](shared-types.md), [curator.md](curator.md) and
[adoption-use-cases.md](adoption-use-cases.md), which scores this against the
cases it has to handle. Kept out of [DECISIONS.md](DECISIONS.md) on purpose.

## Which namespace this is about

**The prefix a catalog publishes its bundles under**, and nothing else.
`lumastack/luma-catalog/decision-records` is *the `decision-records` published by
the `lumastack/luma-catalog` catalog* — so the namespace belongs to the
**catalog**, not to the bundle, and
the same bundle promoted into another organization's catalog is that
organization's to name.

**The estate has several other namespaces and none of them belong here.** Type
names like `luma/project` are [shared-types.md](shared-types.md) and §10.4 of
the knowledge format; `.luma/` as a vendor-owned directory is a settled decision
about layout; `luma-invoke:` is a reserved prose token; `<org>/<repo>` under
`~/.config/` is a configuration convention. **They share a technique and answer
different questions**, and collecting them because the word matches would
produce a document nobody can act on.

## The model, in four lines

| | |
| --- | --- |
| ~~**a catalog declares `namespace`**~~ **superseded — it derives from where the catalog lives** (RET-0005) | the **default local alias** for everything it publishes |
| **a consumer may override it** | the **actual local alias**. Deferred — see below |
| **`source` + `commit` in `adopted.toml`** | the **identity** |
| **collision is detected against `source`** | never against the name |

**The whole design is the separation between the middle two.** A name is what
you type and where the files go. An identity is what decides whether two things
are the same thing.

## The namespace is a claim, not an identity

**Nothing arbitrates it.** No registry issues one, no authority reviews one, and
two organizations may both publish `acme/` in perfect good faith. A catalog
saying `namespace: acme` is making a claim about what it would *like* to be
called.

So it can only honestly be **a suggested local name**. Treating it as an
identity means trusting an unverifiable self-declaration to answer *is this the
same bundle I already have*, which it cannot.

**The thing that is actually unique is the source URL**, and it is already
recorded. Adoption writes `source` and `commit` into `adopted.toml` for
forensics; those two fields turn out to be the identity as well, at no
additional cost.

**This is also the less registry-shaped answer**, which matters given *the
catalog is a catalog, not a registry*. Arbitrating global names is exactly what
a registry does, and declining to arbitrate is how this stays on the right side
of that line.

## Same address, same source is an upgrade. Different source is a collision

| what arrives at `.luma/bundles/<ns>/<name>/` | verdict |
| --- | --- |
| nothing there yet | **adopt** |
| same `source`, same version | **no-op** |
| same `source`, different version | **upgrade** |
| **different `source`** | **collision — fail** |

**This distinction is load-bearing rather than a refinement.** Without it,
re-adopting a bundle to pick up changes is indistinguishable from two catalogs
fighting over one address — the name is identical in both cases, and only the
source tells them apart. A design that checks names alone has to choose between
breaking upgrades and permitting silent overwrites.

## Check per address, never per catalog

**Two catalogs sharing a namespace collide on nothing** as long as their bundle
names differ. `acme/widgets` from one and `acme/gadgets` from the other land in
different directories and never meet.

So the rule is phrased about **an adoption**, not about a catalog: *does this
bundle's destination already hold something from somewhere else?* A rule phrased
about catalogs would refuse a combination that is entirely fine, and it would
have to be evaluated against a set of sources the project may not have
enumerated.

Cheaper to check, and precise about what actually goes wrong.

## The override is deferred, and the failure ships first

**An override is only ever needed once a collision has happened, and a collision
cannot happen yet.** It requires a project adopting from two sources that share
a namespace. There is one catalog. **The escape hatch is for a situation that
cannot currently arise.**

**So ship the error and not the override.** When somebody hits the collision,
the message is a better research instrument than a guess made now — it tells us
what they were actually trying to do, which is the thing nobody can currently
predict.

**And it is almost always the wrong thing to want.** An alias override makes the
forbidden thing possible: hold upstream `acme/git-secrets` and a fork's, renamed,
side by side. That is two contradictory policies in one context — **precisely
what the one-version rule exists to prevent.** The namespace question never
changes that rule; it only decides whether somebody can rename their way around
it.

**If it is ever built, three conditions.** It lives in committed config where a
reviewer sees it, not in a flag. It states a reason, the same move as *a narrow
constraint has to say why* and *an exemption is a sentence rather than a
pattern* — don't forbid it, make doing it quietly impossible. And it warns at
the point of use rather than only at the point of declaration.

**The bar for building it: two namespaces that are genuinely different and
non-competing.** That is an unlikely shape, and it does not deserve much
investment. Build it only if it stays low-effort and cheap to maintain.

## Two commits from one source: visible, not forbidden

Bundles adopted from one catalog weeks apart carry different commits, and that
is ordinary rather than broken. The layout bundle already says what the field is
for: *"Two bundles adopted from one commit came from an internally consistent
set; from different commits, that is visible and checkable."*

**Visible is the whole requirement, and nothing currently makes it visible.**
The data is in `adopted.toml` today and nothing reads it — a small honest gap
rather than a design question.

**Different sources at similar versions is the case that fails.** Some of
catalog A and some of A's sibling is not a mixed-commit problem; it is two
identities, and the rule above already refuses it at whichever adoption lands
second.

## Do two catalogs ever load into one session?

**The question dissolves, and how it dissolves is worth keeping.**

**Catalogs do not exist at session time.** A catalog is copied from, never
resolved against. Adoption happens once, writes files into `.luma/bundles/`, and
is finished — what a session sees is bundles in a repository, with no catalog
anywhere in the picture. *How many catalogs are loaded* has no answer because
zero always is.

Two different questions were wearing one sentence:

| | scope | when |
| --- | --- | --- |
| **adoption** | a repository | once, committed |
| **loading** | a session | every time, from what is already there |

### The video-versus-coding case is two projects

Doing video work and doing code work are two repositories, and each adopts what
it needs. **The repository boundary already does this job**, and it does it
better than a session concept would — the answer is committed, reviewable, and
the same for everybody who clones it.

Where that intuition *does* bite is one repository with two kinds of work in it:
writing code most days, running an incident occasionally. That is conditional
loading, it is a known gap, and it is not a catalog question.

### Core plus department sidecar is the `upstream` chain, already settled

A universal catalog with an organization's below it is the designed shape. A
project names only its organization's catalog and gets the chain.

**And the chain resolves at adopt time**, so it costs a session nothing. What
lands is bundles.

**One caution, and it is the gap this arrives at from a friendlier direction.**
The chain has an answer for *obligation* conflict — most-restrictive-wins, an
organization may raise a universal recommendation and may not lower a universal
mandate. It has no answer for **content** conflict. A department bundle that
contradicts a universal one is two rules in force, and nothing arbitrates. The
sidecar is safe about how strongly you must adopt and silent about what the
things you adopted say.

### But *"start a new session"* is a real mechanism, pointed at the wrong problem

**No harness has a hook for *conditions changed, drop these*.** Agent Skills
load names and descriptions at startup and bodies on match; there is no unload.
That is why conditional loading keeps being deferred.

**Which makes the session boundary the only reload point that exists.** If
nothing can change what is loaded mid-session, then selection has to happen at
session start — and selection at session start is buildable today with the
adapters that already exist. A named mode chosen when a session begins, with
the adapters written to match, needs no new harness capability at all.

**That is the cheap version of routing**, and it is worth separating from the
expensive version. It answers *which subset of what this project adopted is
relevant to what I am about to do* and gives up on *conditions changed
mid-flight*, which nothing can deliver anyway.

Recorded as an idea rather than a proposal. It has not been costed, it needs a
place for a mode to be declared, and *what happens when somebody picks the wrong
mode* has no answer yet — the failure would be silent absence, which is the one
failure mode that is currently well covered and would stop being.

## Open

- **Whether an ad-hoc adoption should be marked as such.** A configured chain
  carries curation and publication checks; a one-off `--from` at a stranger's
  URL carries none, and `adopted.toml` records a source without saying which it
  was.
- **What the collision error should say.** It is the research instrument for
  whether the override is ever needed, so it should ask what the person was
  trying to do rather than only refusing.
- **Whether `namespace` should have been `mandatory` rather than
  `recommended`.** It is recommended because an adopter can always name one
  explicitly, which is true and means a catalog can ship unaddressable by
  default.
- **Session modes.** Where a mode is declared, how `apply` knows which to
  write, and whether picking wrongly can be made noisy.
