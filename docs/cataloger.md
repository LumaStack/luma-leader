---
type: document
title: The cataloger
description: What a tool that tends a catalog would do — the checks only a catalog can run, why it is not foreman, and the fact that nothing currently publishes anything.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# The cataloger

**Draft. Nothing here is settled.** Companion to
[bundle-dependencies.md](bundle-dependencies.md) and
[bundle-versioning.md](bundle-versioning.md), which between them describe a
substantial amount of work that has to happen *where a catalog is written*. This
names the thing that would do it. Kept out of [DECISIONS.md](DECISIONS.md) on
purpose: its claims would otherwise read as positions the organization has taken.

**A cataloger tends a catalog.** It validates what a catalog holds, refuses what
it cannot hold consistently, and reports what the catalog is becoming.

**Anyone runs one.** It is not a luma maintainers' script. An organization with
its own catalog needs exactly the same checks over exactly the same shapes, and a
tool that only worked on the universal catalog would be a private convenience
published as though it were a product.

***Cataloger* is the working name and is not settled** — `curator` currently has
the better argument. See *On the name* at the end; the title of this document is
a placeholder, not a decision.

## Why it is not foreman

**Runtime location, which is the rule the estate already splits on.** *Projects
split on runtime location, not on subject matter* is why `luma-foreman` is
separate from `luma-leader`, and it applies again here.

| | runs in | on behalf of |
|---|---|---|
| **cataloger** | a repository that **publishes** bundles | the author, who can still fix it |
| **foreman** | a repository that **adopts** them | the project, which owns neither bundle |
| **leader** | nothing. It is a place you visit | the organization |

Those are different repositories, different people and different release
cadences. The split is not a guess about future size — it is where the work
already falls in the two design documents above.

**This is also the answer to whether adoption belongs in foreman.** It does. The
boundary worth cutting was never *bundler versus foreman*; it was publish versus
consume, and publication was never inside foreman to begin with.

## Where it lives, and why not in a catalog

**Not in `luma-catalog`, and that is already declared rather than decided here.**
Both candidate homes rule themselves out in their own project descriptors:

```yaml
# luma-catalog
must_not_own:
  - tooling that adopts, vendors or checks bundles

# luma-knowledge-format
must_not_own:
  - tooling that reads or writes the format
```

**The anti-pattern is real independently of those lines.** Shipping the tool
inside the universal catalog would mean an organization has to clone *our
content* in order to create *their own catalog* — content and tool in one
artifact, where only one of them is wanted. `luma-leader` is out for a different
reason: it is never installed anywhere, and a cataloger must be installed
wherever a catalog is written.

**So: its own repository, or a command inside foreman.** The one-verb test
favours its own — a tool whose jobs cannot be named together is more than one
tool, and the audience publishing a catalog is not the audience running checks on
a project.

**What actually blocks it is distribution, not the boundary.** Foreman has no
tags, no packaging, and installs by clone and symlink; its own backlog records
that there is *no released artifact at all*. A second repository doubles an
unsolved problem rather than adding a solved one. **Distribution is owed to
foreman already, and solving it once makes this repository cheap.**

### A catalog stays inert

**Nothing in a catalog executes as a side effect of adoption.** This is the
sharper form of *the catalog should hold no executable code*, and it is the form
that survives contact with the bundles that exist — every workflow in the catalog
is full of `grep`, `git log` and `find` in fenced blocks, and banning code
outright would gut them.

The line is about **when**, not **what**:

| | |
|---|---|
| a bundle carrying a script | **fine.** Code invoked deliberately, by somebody who chose to |
| `adopt` running one | **never.** Code executing as a side effect of fetching |

**The reason is the blast radius, which is worse here than for a package
manager.** Adoption copies a bundle into `.luma/bundles/`, which is committed and
which agents read as authoritative — so a compromised or careless catalog reaches
every adopter's tree with no build step, no review gate and no isolation
anywhere in the path. A package at least lands somewhere nobody mistakes for the
rules.

This is already the stated position in `luma-catalog`'s `bundle-migrations` idea
— *"deliberate invocation only, always"* — reached there from migrations rather
than from catalogs. **It is checkable rather than a judgement call:** `adopt`
copies and hashes and has no execution path at all.

## The line: foreman checks a bundle, the cataloger checks a catalog

**One is about an artifact's internal health. The other is about a set's mutual
consistency.** That distinction should decide every check's home.

`foreman inspect --rule bundles` already exists and checks a single bundle for
defects the knowledge format tolerates — a dangling link, an unquoted frontmatter
wikilink, a template carrying live frontmatter. **Those are properties of one
bundle and need no catalog to find.**

A cataloger's checks are the ones that are *meaningless* about a bundle alone:
whether two bundles' constraints can be satisfied together, whether a release's
version tier is honest about what it removed, whether the catalog as a whole is
drifting toward tightness.

**Neither should reimplement the other**, and the open question is what happens
when a catalog wants both — see below.

## What it would do

### Refuse what only the catalog can see

Two contradictions are already recorded in `DECISIONS.md` under *Obligations
resolve most-restrictive-wins*, described there as **the one error class the
catalog can commit that no individual project could ever detect**:

- a bundle both **mandated and deprecated** — not a precedence puzzle, a broken
  catalog
- a **starter pinning a version the catalog's own mandate forbids**, which would
  make every new repository born failing

To which [bundle-dependencies.md](bundle-dependencies.md) adds the dependency
case: **a bundle whose requirements cannot be jointly satisfied with what the
catalog already holds.** Rejected at publication because that is where the only
person who can fix it is standing — letting it through means it surfaces in front
of an adopter who owns neither bundle and whose only recourse is forking.

### Require a reason for a narrow constraint

A bundle constraining a dependency more tightly than a major line **states why,
and publication rejects it otherwise.** One field, one sentence, and only on the
narrow case.

**The cost of a tight constraint falls on strangers** — every bundle sharing that
dependency is constrained by a decision none of them made. `2.1.4` on its own is
indistinguishable from carelessness; *"pinned to 2.1.4: our regulator requires
policy changes to be reviewed before adoption"* is a sentence somebody stands
behind.

**Asked of bundles, never of projects.** A project pinning its own adoption
affects nobody else and owes nobody an explanation.

### Judge what a release calls itself, where that is mechanical

[bundle-versioning.md](bundle-versioning.md) is explicit that the tiers are the
author's judgment and only one part is enforceable. That part is **references**,
because a reference is structural and countable where a claim is neither.

**Reject outright, when the release does not call itself major:**

- removing a document other documents link to
- removing a field from a type definition when records exist against it
- removing a heading or workflow step other content references

**Surface, never refuse:** every other removal. *"This release removes X — is it
still minor?"* points the author at the signal most likely to mean they got the
tier wrong, without deciding for them in the cases where subtracting was the
improvement.

**Flag, on a patch: any edit touching a normative sentence.** A `must`, a
`never`, an `always`, a threshold, a condition. `must not` → `must` is two
characters, the diff of a typo, and a complete reversal — and *"patch: fixed
wording"* gets approved in seconds. The check cannot know whether the meaning
changed; it can know the edit landed somewhere meaning lives.

### Report what the catalog is becoming

**A doctor, not a gate.** Individual entries are defensible and the aggregate is
what nobody sees.

- every constraint narrower than a major line, with its stated reason —
  *"a catalog drifting toward tightness is a problem nobody would otherwise see,
  because each one was reasonable on its own"*
- bundles whose unconditional `preload` weight is large, since that is a context
  tax every adopter pays and a number the catalog can compute
- cross-bundle links that do not resolve at the versions the catalog holds

### Possibly: create one

`cataloger init` scaffolding a new catalog — `catalog.md`, the directory, the
conventions. **Weakest item here.** A catalog is a file and a directory, so this
saves a copy-paste and earns its place only if the conventions turn out to be
easy to get subtly wrong.

## What it must never do

**Know an organization's conventions.** Structural checks only, exactly as
foreman's bundles rule already argues: which directories a bundle uses, how its
workflows are named, when it may call itself `1.0.0` — those are opinions that
arrive by adoption. **A tool that compiled them in would be deciding standards
rather than checking them**, and it would be useless to the second organization
that ran it.

**Adopt anything.** That is foreman's, and a cataloger that also adopted would
have collapsed the split it exists to express.

**Decide obligations.** Whether a bundle is mandatory is the catalog author's
declaration in `catalog.md`. The cataloger checks that the declarations are
mutually satisfiable; it has no view on what they should say.

**Publish to a registry.** There is no registry. A catalog is a git repository
and a bundle is available by being in it.

## The problem underneath: nothing publishes

**Every check above is described as running "at publication", and publication is
not currently an event.**

`luma-catalog` has no CI, no tags, and no release step. A bundle becomes
available by being committed to `main`. So *reject at publication* has no moment
to attach to, and the design has been written as though one exists.

**Three candidates, none chosen:**

| | catches it | costs |
|---|---|---|
| **merge to `main`** | before anything can adopt it | needs CI, which no repository here has |
| **a tag** | at a deliberate moment somebody chose | bundles are adopted from `main`, so a tag gates nothing today |
| **on demand** | whenever somebody remembers | **an unenforced check is decoration** — the failure mode the estate rejects everywhere else |

**This is the first thing to settle**, because it decides whether the cataloger
is a gate or a report, and those are different tools. Everything above is written
assuming a gate.

**A related question it forces:** if adoption takes whatever is on `main`, then
`main` is the published artifact and there is no unpublished state in which to
catch anything. Either publication becomes a real step, or the checks run
pre-merge and the catalog's `main` is its own guarantee.

## Alternatives considered

**A CI job in each catalog rather than a distributed tool.** Cheaper, and it was
the first suggestion. It fails the *anyone runs one* test: every organization
would copy and then diverge, and the checks that matter most are the ones that
must behave identically everywhere. *Revisit if catalogs turn out to want
genuinely different checks, which would mean this was always local policy.*

**Fold it into foreman.** One tool, one install, no new name. It puts a
project-side tool in a publishing repository and gives foreman a fifth verb
belonging to a different audience — and by the one-verb test, a tool whose jobs
cannot be named together is more than one tool. *Revisit if the cataloger stays
too small to justify its own release process.*

**Let adopters catch everything.** Installation-time checks are already required
by [bundle-dependencies.md](bundle-dependencies.md), so the machinery would exist.
But it surfaces a defect in front of the one person who cannot fix it, and it
scales the cost by the number of adopters rather than paying it once. *Not
revisitable — this is the argument the whole publish-side exists on.*

**A registry rather than a catalog.** Named only to close it: *the catalog is a
catalog, not a registry* is settled, and nothing here reopens it.

## Open, and unresolved here

- **What publication is.** The largest item, above.
- **Overlap with `foreman inspect --rule bundles`.** A catalog should not publish
  a structurally broken bundle, but those checks already exist elsewhere. Either
  the cataloger calls foreman, duplicates it, or the shared part becomes a third
  thing — and *no shared package until two real consumers exist* says to let the
  duplication happen first and extract when the shape is known.
- **Cross-catalog conflict.** Curation makes one catalog internally consistent; a
  project adopting from two can still reach a combination neither publisher saw.
  A cataloger cannot see the other catalog, so this is structurally beyond it.
- **What a check was validated against.** The rules will change, and nothing
  records which rules a given catalog was ever held to. Recorded in
  [bundle-dependencies.md](bundle-dependencies.md) as a conformance version rather
  than a content version, and unanswered there too.
- **Whether any of this is affordable before bundles have dependencies.** No
  bundle depends on another today, so the resolution checks have nothing to
  check. The version-tier and contradiction checks are useful immediately; the
  dependency half is a design for a situation that does not yet exist.
- **The name.** `curator` has the stronger argument and `cataloger` is the more
  guessable word. Cheap to settle now and expensive after anything installs it.
- **Whether a catalog should be checked for inertness at all.** *Nothing executes
  as a side effect of adoption* is enforced on the `adopt` side by having no
  execution path. Whether the cataloger should additionally refuse a bundle that
  *looks* executable — a `setup.py`, a hook, a binary — is unsettled. It would be
  defence in depth against an `adopt` that later grows a convenience nobody
  intended, and it is also exactly the kind of judgement call about file contents
  that this tool is elsewhere forbidden from making.

## On the name

**The register deliberately changes here.** `leader` and `foreman` are role
metaphors, which teach the architecture once you already know it and are hard to
recall before that. From here the rule is *name the tool for the verb it
performs*, as an agent noun — the `compiler`/`linker` form, naming the actor
rather than the artifact. The corollary is worth keeping: **if the job cannot be
stated as one verb, that is a signal about the tool rather than about the name.**

Existing names are unaffected. A convention requiring everything to be renamed
before it can be adopted is one nobody adopts.

### The candidates

| | verb it claims | does the tool do that? | collides with |
|---|---|---|---|
| **cataloger** | catalogs | yes | `<org>-catalog`, weakly |
| **curator** | curates | **yes, and it is the word the design already uses** | nothing |
| **publisher** | publishes | not yet — nothing publishes | nothing |
| **packager** | packages | **no. Nothing is ever packaged** | — |

**`curator` has the strongest support, and it is not a matter of taste.** *The
catalog is a catalog, not a registry* settles the whole catalog-versus-registry
question on admission policy, and states outright: *"Given that material travels
project → organization → universal by promotion, and that entries can be mandated
downward, **curation is the actual semantics.** A registry that mandates is
strange; a catalog with an editor is ordinary."*
[bundle-dependencies.md](bundle-dependencies.md) uses the same word for the same
job — deciding whether a narrow constraint is justified is *"a catalog-editor job
rather than a mechanism."* **A tool named for the semantics the design already
declared is not a new metaphor; it is the existing vocabulary.**

**`cataloger` is more guessable and carries the collision.** It names its object,
which `curator` does not — somebody meeting the word cold learns more from it.
Against that, `luma-catalog` and `luma-cataloger` are three letters apart, and the
same decision already established that avoidable near-collisions get avoided:
*"A local collision, small but free to avoid. `luma-foreman` already has
`inspect/registry.py`… Two registries meaning different things in one codebase is
a permanent tax."* That reasoning applies here with the same force.

**`publisher` is the right name for a job that does not exist.** If publication
becomes a real event — see *The problem underneath* — this is exactly the verb.
Naming the tool for it now would be naming it after the thing most likely to
change.

### `packager`, and the `luma-warehouse` rename it would need

**Recorded because it was raised, and it is the weakest of the four.**

**Nothing is packaged.** A bundle is a directory that gets copied; there is no
archive, no build, no artifact. `packager` names a job the tool does not do,
which fails the one-verb test in its worst form — the verb is wrong rather than
merely absent — and it imports exactly the package-manager expectations that
*the catalog is a catalog, not a registry* was written to keep out.

**And `warehouse` reintroduces the semantics that decision rejected.** The
argument that chose *catalog* over *registry* was **admission policy**: a registry
is permissive, and *"catalog implies an editor decided an entry belongs."* **A
warehouse holds whatever you put in it.** It is the permissive-storage reading
under a different word, so the rename would give up the one property the name was
chosen for — and it is a place metaphor besides, the category already set aside
with `depot`.

**The cost is also large and easy to underestimate.** `catalog` is a type
(`_types/catalog.md`), a filename (`catalog.md`), a field vocabulary, and the
subject of several settled decisions. This is not a repository rename; it is a
vocabulary migration with a reopened decision at the end of it.

*Revisit only if the mechanism changes — that decision's own re-open trigger is
remote resolution, and it has not fired.*
