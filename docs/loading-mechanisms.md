---
type: document
title: Loading mechanisms
description: By what mechanism knowledge reaches a reader — the three load classes any design must implement, the six candidates for delivering them, which class each can actually serve, and why five of the six readers are not models.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-24T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-24T00:00:00Z }
---

# Loading mechanisms

**Draft. Nothing here is settled, and most of it is half-baked deliberately.**
Eighth companion to [bundle-dependencies.md](bundle-dependencies.md),
[bundle-versioning.md](bundle-versioning.md),
[shared-types.md](shared-types.md), [curator.md](curator.md),
[catalog-namespaces.md](catalog-namespaces.md),
[adoption-use-cases.md](adoption-use-cases.md) and
[silent-presence.md](silent-presence.md). Kept out of
[DECISIONS.md](DECISIONS.md) on purpose.

**The use cases scored designs for acquiring and selecting content. This one
takes the question underneath them: by what mechanism does a document actually
arrive in a model's context.** That question was answerable in one sentence while
one mechanism existed. It is not any more, and the candidates have genuinely
different properties rather than being variations of one idea.

**Everything currently built is prototype.** A skill per workflow and a managed
block in `CLAUDE.md` is one point in this space, chosen quickly because it worked.
Nothing below argues from it, and the space is explored as though nothing had
been built.

**Two kinds of thing get cited here, and conflating them is how a greenfield
design quietly becomes an incremental one.**

| | how it is treated |
| --- | --- |
| **Platform facts** — what a harness can actually do | a constraint. Any design has to be true about these, and where one is a fact about *one* harness it is labelled as such and kept out of the model |
| **Prior positions** — what earlier drafts decided | **not a constraint.** Included only where the idea earns its place on merit, and the merit is stated. Cited as reading, never as settlement |

**Nothing is in this document because it was already written down.** Where an
earlier draft reached something that survives the test, it is restated as a
requirement with its argument, not deferred to.

## The one thing to implement regardless of what wins

**`preload: mandatory | optional` is a single axis hiding two.** Whether a
document's *body* is present at session start, and whether its *existence* is
advertised at all, are separate questions, and today's `optional` answers only
the first.

| class | metadata present at session start | body present at session start |
| --- | --- | --- |
| **mandatory** | yes | yes — read it now |
| **progressive** | yes | no — opened when the situation matches |
| **on-demand** | no | no — findable, not advertised |

**Splitting them is what makes anything cheap.** A design that only knows
*loaded* and *not loaded* has to advertise everything at full weight or hide it
entirely. With three classes, the standing surface carries mandatory bodies and
progressive titles, and on-demand content costs nothing until somebody goes
looking for it.

**But these three are outcomes, not inputs** — see the next section. They are the
observable states a document can be in, and they remain the right vocabulary for
*describing* where a document ended up. What an author writes should be something
else, from which these are computed.

## `mandatory` is three questions welded together

**The sentence that breaks it open: *we want the model to follow these rules and
never miss, but we do not want them loaded until it is time to follow them.***
Both halves are reasonable and the current vocabulary cannot express them at
once, because it has one word for how binding a rule is and when its text has to
be present.

**Three axes, and only the first two should be authored.**

| axis | question | who knows |
| --- | --- | --- |
| **Obligation** | how bad is it to violate this? *advisory / binding / blocking* | the author, mostly |
| **Applicability** | when does this rule govern? *always, or when X* | the author |
| **Delivery** | when does the text have to be present? | **nobody should write this** |

**Delivery is derivable, and that is the whole move.** A rule that governs work
on stylesheets should arrive when work on stylesheets begins. Given what a rule
governs, how binding it is, and what the harness can do, *when to deliver it* is
a computation rather than a declaration. Authors are bad at it anyway — the
delivery decision depends on what else the adopter took and which harness is
running, and an author knows neither.

**This also answers *different things are mandatory in different situations*.**
Working on stylesheets and working on Python are different applicability
contexts, so the set of rules in force differs. That is not a new mechanism; it
is applicability doing its job. The mandatory set was never one list — it was one
list because there was nowhere to say otherwise.

### Applicability needs a trigger vocabulary, not a condition language

**A general condition language is the thing to avoid** — a tool cannot follow
prose, and inventing an expression grammar to decide what loads is a large,
permanent commitment. A **closed vocabulary of triggers** is not that, and it is
enough:

| trigger | fires when |
| --- | --- |
| `always` | unconditionally |
| `path` | a file matching a glob is read or written |
| `tool` | a named tool or capability is invoked |
| `command` | a shell command of a given shape runs |
| `phase` | a known moment — session start, before commit, before push |
| `topic` | the work is *about* something — matched semantically, not mechanically |

**The first five are mechanical and the last is not**, and that distinction
decides push versus pull rather than taste. A mechanical trigger is something the
system can detect, so it can be pushed. `topic` can only be recognised by the
model reading a description, so it must be pulled. Neither can cover the other's
case: nothing mechanical knows the user is *thinking about* stylesheets before a
file is touched, and no description reliably fires *at the moment of commit*.

**A rule may carry more than one trigger.** *Never commit a credential* is
`command` plus `path` plus `phase`, and wanting all three is normal rather than
a modelling failure.

### Degradation is the property to design for

**A harness that cannot honour a trigger must cost more, never guarantee less.**

| obligation | trigger honoured | trigger not available |
| --- | --- | --- |
| **blocking** | intercept at the action | **load at session start** — expensive, guarantee intact |
| **binding** | deliver at the trigger | **load at session start** |
| **advisory** | deliver at the trigger | advertise only |

**Read the middle column against the right one: only the cost moves.** That is
what makes the declaration portable across harnesses — the author states
obligation and applicability once, and a weak harness pays in tokens rather than
in missed rules. It is also the answer to *how much harness-specific machinery is
acceptable*: as much as you like, provided it lives in a renderer and its absence
degrades cost rather than correctness.

**And it makes the failure legible.** *This project has four binding rules that
cannot be triggered on this harness, costing 3,200 tokens every session* is a
sentence somebody can act on — by changing harness, by rewriting the rule, or by
accepting it. Today the same situation is invisible.

### Where this leaves the three classes

They survive as **derived state**, which is the useful place for them. A document
with `binding` obligation and an `always` trigger computes to *mandatory*. The
same document with a `path` trigger, on a harness with hooks, computes to
*progressive* — advertised, delivered on match — **without any weakening of the
obligation**. That is exactly the thing the current vocabulary cannot say.

**Open, and genuinely unsettled:**

- **Is `blocking` an obligation level or a separate concern?** It requires
  interception, which not every harness offers, and it may belong with permission
  machinery rather than with knowledge.
- **Can obligation and applicability be adopter-overridden separately?** Probably
  applicability yes and obligation no — an adopter narrowing *when* a rule applies
  is configuration, and an adopter downgrading *how binding* it is is opting out
  of the rule.
- **What happens when two triggers fire at once and the rules conflict.** The
  contradiction failure, arriving through a new door.

### Who sets it

**The arguments run opposite ways.**
Whether a document is the spine of a bundle is a fact about the bundle, which
only the author knows. Whether a bundle is central to this project's work is a
fact about the adopter, which only the adopter knows — and the adopter is the one
paying the context bill. Both are true at once, which points at an author
suggestion and an adopter override rather than at either alone.

**A bundle-level adopter default is the coarse version of that** and is worth
having on the table, but it is all-or-nothing per bundle and does nothing for the
case where one document inside a wanted bundle is the problem. A per-document
override reaches into another bundle by path and breaks silently when the author
renames a file — unless the override names a **stable identifier** rather than a
path, which is a design option nobody has tried and which would remove the
objection entirely.

## Six readers, and only one of them is a model

**The question *should the index be loaded* assumes the reader is a model.** It
mostly is not. Enumerating the readers first changes which arguments are even
relevant, because they want incompatible things.

| reader | wants | cares about token budget? |
| --- | --- | --- |
| **a model at session start** | the smallest possible routing surface | yes, absolutely |
| **a tool** | complete structured data, queryable | no — size is irrelevant |
| **a person choosing what to adopt** | descriptions, scope, what it obliges, what it costs | no |
| **a person auditing** | *what is this project bound by, and since when* | no |
| **a person onboarding** | the map — what exists and where to start | no |
| **a person in three years, with no tooling** | anything at all, legibly | no |

**Five of the six do not care about the standing-cost argument**, which is the
argument the whole index question has been conducted in. That is a framing error
worth naming: an index does not have to be loaded to be worth having, and if no
model ever reads it, four readers still do.

**The human case alone justifies an index, and it survives all five pressures
below without help.** It does not break at two hundred bundles, because people
search and scroll. It does not care about harness churn, because a person is not
a harness. It works in a bare clone. It is the one use with no failure mode.
**Every model-facing use is upside on top of that** — which means the index
question should never have been contingent on winning a loading argument.

**It also inverts a design decision.** If the primary reader is human, the
primary form should be legible — browsable on a forge, stable anchors, readable
raw — and the machine form is the derivative. The opposite ordering, a data file
with a rendered human view, is also coherent. Which is source and which is
rendering is genuinely open, and it is a different question from whether to have
one.

## Seven duties, currently performed by one artifact

The word *index* is doing at least seven jobs. They have different lifecycles and
different consumers, and a design that separates them has more room than one
that does not.

| | duty | derivable? |
| --- | --- | --- |
| 1 | **Inventory** — what exists | fully derivable from the tree |
| 2 | **Routing** — which door for which job | **authored, and the valuable part** |
| 3 | **Obligation** — what must be read before acting | semi-derivable, from the load class |
| 4 | **Budget** — what fits in the context available | derivable, but only by measuring |
| 5 | **Provenance** — where it came from, is it current | recorded at adoption |
| 6 | **Evidence** — what was actually opened, and did it help | nothing captures this |
| 7 | **Attestation** — what was in force here, provably, at a given commit | **nothing captures this either** |

**Four, six and seven are where the leverage is, and none of them exists.**
Budget needs measured cost per document. Evidence needs to know what a session
actually opened. Attestation needs the answer to be reconstructable later.

**Attestation is the duty the human auditor needs and no model ever asks for**,
which is why enumerating readers first mattered — it is invisible from the
model-facing framing. *What knowledge was this repository bound by when this
commit landed* is a governance question, and a committed index answers it for
free, because the repository already carries a dated history of itself. For an
organization that has to demonstrate compliance rather than assert it, this may
be the most valuable thing on the list, and it is achievable as a side effect
rather than as a feature.

**Two and six are related in a way worth noticing.** Routing is authored, which
means it is a guess; evidence is the only thing that can tell you the guess was
wrong. A routing decision made in 2026 with no feedback loop can only decay.

## Five pressures any design must answer

| pressure | what it kills |
| --- | --- |
| **harness churn** — Claude Code now, something else in three years | anything that makes `CLAUDE.md` the source of truth rather than one rendering |
| **scale** — 5 bundles becomes 200, and an estate has thousands | anything flat, anything with per-document standing cost |
| **being wrong** — the routing decision made in 2026 is bad by 2028 | anything with no feedback loop; a static index can only decay |
| **tool absence** — bare clone, no network, no foreman | anything unreadable without the tool |
| **concurrency** — branches, worktrees, several agents at once | anything whose merge conflict cannot be resolved by rebuilding |

**The last two pull against each other**, and against tool-ownership. A
tool-owned artifact is the most powerful option and the one least readable
without the tool; a canonical serialization that rebuilds deterministically is
what keeps its merge conflicts resolvable. That tension is the real design
problem rather than a detail.

## Four rings

Progressive disclosure needs a granularity, and there are four available. Three
are in use somewhere; the fourth is unexplored.

| ring | what it holds | status |
| --- | --- | --- |
| **0** | what exists in the catalogs you are pointed at but have **not** adopted | nothing does this — the ignorance failure, one level up |
| **1** | what is adopted in this project | the ring every attempt so far has aimed at |
| **2** | what is inside a bundle | attempted by hand; never derived |
| **3** | **what is inside a document** — sections, not files | **unexplored** |

**Ring 3 is the interesting one.** A section-level index means a three-hundred
line policy stops being all-or-nothing: with cost attached per section, you load
the two sections that apply. That is the difference between a document being
*large* and being *expensive*, and it would let policy documents get properly
thorough without anyone paying for the thoroughness until they need it.

**Ring 0 matters for a different reason.** It is the only thing that closes
*cannot tell nothing-applies from I-never-looked* at the catalog level rather
than the project level. An agent that has never heard of a bundle does not go
looking for one.

## The candidate mechanisms

Six. Each gets the same treatment: how it works, what only it can do, where it
breaks, and what would make it excellent. **None of them is ruled out here.**

### 1 — Generated indexes

**How it works.** Walk the tree, read frontmatter, emit an index. Regenerate on
edit. The index carries identifier, title, description, load class, and anything
else derivable — cost, size, last change.

**What only it can do.** It is the only candidate that serves **all six readers**.
A committed index works in a bare clone, in a harness that does not exist yet,
for a person browsing on a forge, for a person auditing what a repository is
bound by, and for a tool. Every other candidate here serves the model and nobody
else.

It is also the only one that makes **negative space** visible — you can see what
you are *not* loading, which is the entire point. And it is the only one that
can carry attestation, because it is a committed artifact with a history.

**An index does not imply a loaded index.** These are separate decisions and
collapsing them is what makes the option look more expensive than it is. An index
can be: read by a person only, queried by a tool only, partially projected into a
standing surface, or loaded whole. **Choosing to have one commits to none of
those.**

**Where it breaks.** Two places, both addressable, and neither fatal. Standing
cost grows linearly *if the whole thing is loaded into context* — which is a
property of that choice rather than of the index. And regeneration has to
actually fire.

**What would make it excellent.**

- **Belt and braces on the trigger** rather than one perfect one: regenerate on
  adopt, on edit, on demand, and in continuous integration. Store a hash of the
  inputs so a missed trigger is **detectable** rather than silent. Idempotent and
  cheap means running it from four places costs nothing.
- **Hierarchy**, so depth is free: a project index of bundles, a bundle index of
  documents, a document index of sections.
- **Tool-owned and tool-mutable**, which is the option most easily missed. The
  choice is not *derived* versus *hand-authored*. A third category — the tool is
  the only writer, and authored intent enters through commands rather than an
  editor — carries anything an authored file could while staying honest, because
  writes go through validation. `Cargo.lock` and `go.sum` live there.
- **Rich, because size only matters to one of the six readers.** An index a hook
  or tool queries — and a person reads — can carry cost, evidence, rankings and
  section detail, because none of that reaches a context window unless something
  chooses to put it there. Most of the objection to indexes is an objection to
  *loading* them.
- **Two renderings from one source**, if the human and machine forms genuinely
  want different shapes. The open question is which is source and which is
  derived, not whether both can exist.

**Drift, honestly.** Tool-ownership does not eliminate drift; it makes drift
**detectable**, which is the same bargain `adopted.toml` already makes with a
checksum. That is a stronger position than refusing to author anything derivable.

### 2 — Hooks

**How it works.** The harness fires events; a hook runs code, and can inject
context or block an action.

**What only it can do.** Three things, each unavailable elsewhere:

- **Push instead of pull.** Nothing depends on the agent deciding to look. This
  defeats silent absence structurally rather than by persuasion — the only
  candidate that does.
- **Per-turn situational routing.** A hook that sees the prompt before the model
  does can match it against the index and inject **only what is relevant to this
  turn**. Progressive disclosure with zero standing cost, and the routing
  decision happens deterministically outside the model. Nothing else can do this.
- **Loading late.** A hook firing before a tool call can put text in front of an
  agent **at the moment it acts**, by returning a reason that reaches the model.
  *Platform fact, verified rather than assumed.* Its consequence is the useful
  part: **you cannot unload, but you can load late, and loading late is most of
  what conditional loading ever wanted.** A design that gives up on conditions
  because nothing can drop what is loaded has answered the wrong question.

Plus enforcement: a hook that blocks makes a rule **execute** rather than be
read, which is categorically stronger than any amount of presence. There is an
existence proof that this is buildable — a permission gate doing exactly it — but
the reason to want it is the category, not the precedent.

**Where it breaks.** No standard, so every harness needs its own adapter — the
one place adapters are unavoidable, since `SKILL.md` solved distribution and
nothing solved hooks. It is code running on somebody's machine, so it needs an
install and a doctor. It can fail silently and leave a session unguarded.
Injected context is invisible in the repository, so *what did the agent actually
know* becomes harder to reconstruct than a committed file.

**Compaction, and a warning about how to use this fact.** A long session
discards context, and anything mandatory that was loaded at the start is gone
without notice. *Platform fact, one harness:* Claude Code re-reads and re-injects
the project-root context file after a compaction, and reloads path-scoped rules
when a matching file is next read — so some content survives by virtue of where
it lives, and a hook is not required to achieve it.

**That fact belongs to a rendering, not to the model.** Designing the mandatory
class around one harness's compaction behaviour is precisely the anchoring this
document is trying to avoid. The durable requirement is the general one: **a
mandatory document must be re-established after any context loss, and the design
must name what does the re-establishing on a harness that offers nothing.**

**What would make it excellent.** Treat hooks as *delivery* over a substrate that
is not hook-specific — an index, or frontmatter — so a harness without hooks
degrades to reading files rather than getting nothing. And a verification path
that proves the hook fired, because the failure is silent.

### 3 — Frontmatter only

**How it works.** No aggregate artifact at all. Each document declares its own
load semantics, and whatever consumes it walks the tree and honours them.

**What only it can do.** It is the only candidate with **nothing that can go
stale**, because there is no derived thing. The contract travels with the file —
rename it, move it, copy it into another bundle, and the semantics come along. It
is also the only one with **no merge surface**: two branches adding documents
never conflict, where any shared aggregate does. Against the concurrency
pressure it is the strongest position available.

**Where it breaks.** Discovery requires a walk, and a walk by the *model* is one
read per file. At twenty documents that is genuinely cheap and the objection is
theoretical; at two hundred it is not, and something has to do the walking and
cache it — which is an index or a tool, arriving by the back door.

**The harder limit is that cross-document facts have nowhere to live.** Ordering,
*read A before B*, groupings, mutual exclusion, and any budget decision that
depends on what else was adopted. A document cannot know what it was adopted
alongside. Nor can frontmatter hold anything unauthored — measured cost, usage
evidence — because those are not things a person writes.

**What would make it excellent.** Richer declarations than a load class alone:
`when` conditions, `requires`, `supersedes`, `conflicts_with`. Then treat the
filesystem walk as a **live index**, always correct by construction, and cache it
only when measurement says it is needed.

### 4 — A tool the model queries

**How it works.** Retrieval on demand. `recall "merge conflicts"` returns three
ranked lines; `context --budget 4000` returns exactly what to load.

**What only it can do.**

- **Flat scaling.** Standing cost is the knowledge that the tool exists — call it
  thirty tokens — and it does not grow at two hundred bundles or two thousand.
  Nothing else here has a flat curve.
- **Sophistication outside the window.** Semantic ranking, evidence weighting,
  budget solving, freshness decay, cross-catalog search. None of it costs the
  session anything.
- **Evidence for free.** Every retrieval is a tool call in the transcript, so
  duty 6 arrives without a hook and without instrumentation.
- **Harness neutrality.** Every agent can run a command. That is more universal
  than hooks and arguably more universal than skills.

It also handles ring 0 naturally, since a tool can query catalogs rather than
only what was adopted.

**Where it breaks.** Pull, not push — the agent must decide to ask, which is
silent absence at full strength. Per-call overhead is real, so at small scale one
cheap index may cost less than ten queries; the flat curve wins at scale, not
everywhere. And it requires the tool present: a bare clone without foreman gets
**nothing**, which is a real regression against committed files and runs straight
into the tool-absence pressure.

**What would make it excellent.** Two moves. Ship it as a native tool the harness
surfaces — a tool definition with a description — rather than only a command, so
the **tool description itself becomes the standing advertisement**. That converts
*the agent must know to ask* into *the agent has a tool for this in its list*,
which is a much smaller leap. And make it degrade: if the tool is absent, the
committed index or the directory itself is still readable.

### 5 — Everything becomes a skill

**How it works.** Project every document — policy, concept, type — as a skill
whose description says when it matters. No new machinery at all.

**What only it can do.** Skills are an open standard read by many agents, and the
harness **already implements** the progressive class exactly: name and
description always loaded, body on match. Zero invention, and distribution is
somebody else's problem.

**Where it breaks, and this is the sharpest boundary in the document.**
Description matching is **probabilistic**. A rule that must always apply cannot
depend on a match that usually happens. So skills serve `progressive` and
`on-demand` natively and well, and **cannot serve `mandatory` at all**, no matter
how the description is written. That single fact partitions this space more than
any other consideration here.

**What would make it excellent.** Accepting the boundary rather than fighting it:
use skills for everything progressive, and something else for the mandatory
class. Also worth testing rather than assuming — how well does description
matching actually fire for a *rule* as opposed to a *procedure*? Nobody has
measured it, and the convention objection (`SKILL.md` answers *how do I do X*) is
about convention rather than mechanism.

### 6 — Deterministic imports

**How it works.** An `@path` import in a context file, read at session start,
unconditionally.

**What only it can do.** It is the only mechanism that **guarantees** a document
is in context. Not likely, not usually — actually. Full text, every session.

**Where it breaks.** It serves exactly one class, it is harness-specific, and it
is expensive by design. There is no partial version of it.

**What would make it excellent.** Nothing — it is already the honest price of the
claim. The design question it raises is upstream of the mechanism: **if
`mandatory` genuinely means the body is present, then paying full freight is
correct, and a project whose mandatory set does not fit its budget has a content
problem rather than a mechanism problem.**

## Which mechanism can serve which class

Mapping the mechanisms against obligation and trigger is where the candidates
stop competing.

| obligation + trigger | what can actually deliver it |
| --- | --- |
| **binding, `always`** | imports, or session-start injection by hook. **Not skills** — matching is probabilistic, and a rule that must never be missed cannot rest on a match that usually happens. **Not a tool** — that is pull |
| **binding, mechanical trigger** | a hook at the trigger point. Falls back to the row above where no hook exists — costlier, same guarantee |
| **binding, `topic` only** | **nothing delivers this reliably**, and saying so is the useful result. A binding rule whose only trigger is semantic either accepts probabilistic delivery or is rewritten to have a mechanical one |
| **advisory, `topic`** | skills natively; a tool whose description advertises the surface |
| **advisory, mechanical** | a hook, cheaply; or advertise and let it be pulled |
| **anything, on request** | tool query — and this is where an index earns its keep, as the thing the tool searches |

**The third row is the finding.** It is the one combination with no honest
answer, and it is also the combination authors will reach for most — *this rule
matters enormously and applies whenever the work is about X*. Naming it as
unservable is more useful than pretending a well-written description closes it.

**And the four families sit at different layers rather than opposite each
other:** frontmatter is the declaration, the index is the cache of those
declarations, hooks are the push channel, the tool is the pull channel. A
coherent system can use all four. The arguments worth having are about which
layer carries which duty, not about which candidate wins.

## Where `@path` fits, specifically

Worth deciding separately, because it is the only mechanism on the list that
*guarantees* anything, and guarantees are scarce here.

**For the mandatory class it is the obvious fit.** The distinction it draws is
between **naming a document and delivering it** — a list of paths under a heading
saying *read these first* is a recommendation, however forcefully worded, and an
import is not. If `mandatory` is to mean what the word says, something in this
category has to carry it.

**For a thin adapter it is more interesting and less clear.** An adapter that
tells a workflow's standing context to the model as a list of paths is relying on
the model to follow them; an adapter that imports them delivers them. The second
is strictly stronger and costs the full text **every time the adapter fires**
rather than once per session — which is the better trade when a skill fires
rarely and the worse one when it fires constantly. That is an argument for the
choice being per-adapter rather than global, and for it being informed by
measurement.

**Open questions before using it anywhere:**

- Whether imports resolve inside a skill body at all, or only in the context
  file. **Verify before designing against it.**
- Whether an import counts once or once per referencing document, when three
  skills name the same policy.
- Whether the transitive depth is bounded, and what a cycle does.
- Whether it survives compaction the way the project-root context file does.

**The pressure it fails is harness churn.** An import is a harness feature. If
the mandatory class rests entirely on it, a harness without imports has no way to
honour the strongest claim a bundle can make — which argues for the *declaration*
living in frontmatter and the import being one rendering of it.

## Working positions: index, bundles, entry points

Held loosely. Each is an argument rather than a decision, and any of them should
lose to a better one.

**A manifest of files is redundant; a manifest of routes is not.** The
filesystem plus frontmatter answers *what exists* without anyone writing it down.
What it cannot answer is *which door for which job, in what order, for whom*.
Only the second needs authoring — the same cut as duty 1 versus duty 2.

**A bundle should address itself in stable identifiers, never paths.** The
location a bundle occupies differs per project and per consumer, so a bundle that
names one stops being portable. Resolving an identifier to a location is the job
of whatever is doing the delivering, and this holds under every mechanism above
— it is the one position here that nothing on the list would change.

**Multiple entry points, chosen by the loader, looks right.** A single entry
point makes a bundle claim it has one way in; bundles that serve several distinct
jobs then have their other doors reachable only by scanning an inventory.
Three things follow from making the doors explicit. Bundles with genuinely
distinct jobs stop pretending otherwise. **Selecting a subset to deliver gains a
unit to select over** — an adopter who wants one capability from a bundle and not
another has no way to say so when the only unit is the whole bundle. And a door's
*when* clause becomes the routing entry, which is the index-shaped answer to
conditions rather than a condition language nobody wants to design.

**A door is also the natural place for the load class**, which is worth
considering against putting it on the document. *Opening this door brings this
standing context* is a statement about a job, and a document's importance is
usually a function of which job you are doing rather than a property of the
document alone.

**On the separate-file question, one piece of prior art is worth knowing.**
`index.md` is a reserved name at a bundle root in the format, defined as derived
navigation and a rebuildable cache, with its structure deliberately unspecified.
Claiming a *new* reserved name is expensive; that one is already paid for. **This
is a naming convenience, not a design driver** — if the right answer is not a
derived file at the bundle root, the reserved name is irrelevant.

**Any authored list needs an answer for the document nobody added to it.** A
curated *what is here* list is a defensible thing to want — it says why to open
something, which an inventory cannot. But hand-maintained, it silently omits
whatever was added last, and it reports a complete-looking list while doing so.
*The tool is the only writer* is one answer; deriving the list and authoring only
the reasons is another; accepting the gap and never claiming completeness is a
third.

## Hiding the filesystem

**Filed as an advanced feature**, though less distant than it first appears —
the section after this one makes it reachable in stages rather than as a single
inversion.

**The idea.** Lookup becomes the primary interface and the filesystem is not
exposed. The agent's view is a capability surface — *you can record a decision,
audit a bundle, measure token waste* — with no paths, no bundle names, no
document types. Which bundle a capability came from is an implementation detail
resolved after the agent picks. Restructuring bundles then never disturbs a
session.

**Browsing stays available as a secondary method, with hard guardrails around
when it is allowed.** Free rein is sometimes exactly what you want and sometimes
loads things nobody asked for, and the context is polluted with no way to take it
back. **Predictability is the objective**: a model whose loading behaviour is
bounded is one whose context you can reason about. The guardrails are what make
browsing a deliberate act rather than a default.

**What makes it hard.** It is the most inversion of the six — every other option
assumes the agent can see the tree. It needs the lookup to be genuinely good
before the browsing fallback can be constrained, or it degrades to a worse
version of option 4. And it interacts with tool absence: hiding the filesystem
from an agent that has no tool leaves it with nothing.

**Why it may be right eventually.** It is the cleanest possible progressive
disclosure, and it is the only option where the standing surface is *jobs* rather
than *files* — which is what the agent actually needs to know.

**And it stops being a someday feature once access is gated** — see the next
section. *Hard guardrails around browsing* is not a discipline to be written down
and hoped for; it is a permission rule, and that makes the whole idea shippable
in stages rather than all at once.

## Gating access, and the one thing only it can do

**The proposal: reads of bundle content are denied by default, and access is
opened dynamically as the situation warrants.** The model cannot reach for the
fallback without explicit permission. Not a seventh delivery mechanism — it
delivers nothing — but a **control layer over all six**, and the only thing here
that can act in the negative.

**Every other mechanism can supply, encourage or advertise. None can withhold.**
Once content is in the window it is in the window, and *the context is polluted
with no way to take it back*. A deny is the only intervention that happens before
the damage rather than after, which makes this categorically different from
everything above rather than one more option beside them.

### What it buys

- **Predictability, as a property rather than a hope.** Context composition
  becomes deterministic: what is in the window is what something decided to put
  there. A model whose loading behaviour is bounded is one whose behaviour can be
  reasoned about, and that is worth more than any individual token saving.
- **It dissolves the capability-surface versus browsable fork.** Those look
  mutually exclusive at ring 1 — one hides the structure, the other exposes it,
  and paying for both seems unavoidable. They are not exclusive once access is
  gated: lookup is primary, browsing exists, and the gate decides when browsing
  is a legitimate act rather than a default.
- **Applicability and access become one mechanism seen from two sides.** If a
  trigger says a rule governs work on stylesheets, that same trigger can *open*
  the stylesheet bundle when such work begins. One declaration drives both
  delivery and permission, with nothing extra to author.
- **Denied reads are the best evidence signal available.** A refusal records that
  the model wanted something it could not get — direct evidence that routing was
  wrong, which no amount of *what was opened* can tell you. Negative evidence is
  usually the stronger signal, and nothing else on this list produces any.
  Aggregated across projects it is also a promotion signal for a catalog: people
  reaching for things they do not have.

### What it costs — less than it first appears

**A denial is not a dead end, and treating it as one is a design error rather
than a property of gating.** The model cannot route around the gate unilaterally,
which is the point; recovery does not require unilateral action. The obvious path
is the one every permission system already uses: **deny, surface the denial,
grant — automatically or by asking — and retry.** A wrong routing decision costs
a round trip, not an answer.

**It is better than the ungated failure, not worse.** Ungated, a routing miss is
silent: the model browses, finds something adjacent or nothing, produces a
slightly worse answer, and **nobody learns that routing was wrong.** Gated, the
same miss is loud — something was reached for, refused, and recorded. Converting
a silent failure into a noisy one is the improvement, and it is the thing this
whole document keeps asking for elsewhere.

**Grant-after-denial is the highest-quality evidence in the system.** A refusal
that a person then approves is a human-confirmed routing miss — a labelled
example saying *this should have been delivered and was not*. Nothing else
produces a signal that clean, and it arrives as a by-product of the control layer
rather than needing its own machinery.

**Three costs do survive, and they are requirements rather than trades:**

- **Round trips.** Deny-escalate-retry costs latency, and a human round trip
  costs more. Cost, not correctness — admissible under the degradation rule, but
  real.
- **Denial rate has to stay low, or the gate gets switched off.** The risk is not
  any single denial; it is prompting so often that somebody disables the whole
  thing, leaving no gate at all. **That makes denial rate a quality metric for
  routing** — frequent prompting means routing is broken and routing is what
  needs fixing.
- **Unattended sessions have nobody to ask.** A scheduled or continuous-integration
  run cannot escalate to a person, so the policy needs a non-interactive branch:
  grant and log, or refuse and report. Choosing which is a real decision and it
  cannot be deferred to the moment.

**Fail-open and fail-closed are both bad, differently.** A gate that fails open
is worse than no gate, because control is believed and absent. A gate that fails
closed and cannot escalate blocks work. For a mechanism whose purpose is
predictability, failing closed **with a working escalation path** is the answer —
and the escalation path is what makes failing closed acceptable at all.

**It is harness-specific machinery of the most binding kind** — but it passes the
degradation rule. On a harness with no permission mechanism the model can browse,
which is what it would do anyway. **Absence costs predictability, not
correctness**, and that is the test.

### Gate the content, not the map

**They are separable, and the asymmetry decides it: the map is small and the
content is large.** Gating the index adds a round trip to every act of navigation
in exchange for almost no pollution prevented. Gating bodies prevents nearly all
of it, and leaves the model able to see what exists and ask for it.

So: **the index stays readable, bundle bodies are gated.** A model that knows
what exists and must request it is exactly the deliberate act wanted — and the
request is the audit record. This also keeps the floor above zero on a harness
where nothing else is installed, which is the fallback position reached earlier
by a different route.

This also means the gate is not a secrecy mechanism. Nothing is hidden. It
enforces **deliberateness**, which is a different and more achievable goal.

### Who opens it, and when

Three answers, materially different, and the design probably wants all three at
different obligation levels:

| | who decides | good for |
| --- | --- | --- |
| **static** | the adopter, in configuration | bundles this project simply never uses |
| **trigger-driven** | the applicability declaration, automatically | the common case — coherent with everything above, no extra authoring |
| **on request** | a person, in the moment | a small tier where the cost of a wrong load is high |

**Most opening has to be automatic or the ergonomics collapse** — a prompt per
read is unusable, and a mechanism people disable is worse than one never built.
Human approval is a tier, not the default.

### Staged, which is the argument for taking it seriously now

Each stage is independently useful and reversible, which is unusual for anything
this structural:

1. **Gate bodies, allow the index.** Predictability arrives immediately; the
   recovery path stays open.
2. **Route retrieval through a tool** and gate direct reads. Retrieval becomes
   ranked and logged; browsing becomes explicit.
3. **Hide paths entirely.** The capability surface, now enforceable rather than
   aspirational.

**Open:**

- Whether the gate is **per-session state or global**. Parallel agents in
  different situations need different access, and that is state something has to
  carry.
- Whether a denial reads to the model as **denied or as absent**. Denied lets it
  ask, which is the whole escalation path; absent makes the gate a secrecy
  mechanism, which it was not supposed to be. *Denied* looks obviously right,
  which is a reason to check it.
- **What escalation looks like when nobody is there.** Grant-and-log or
  refuse-and-report, and whether that is per-project configuration or per-rule.
- Whether an adopter can **turn the gate off wholesale**. They will want to, and
  it makes every guarantee conditional — but a mechanism people cannot disable is
  one they route around instead.

## Two harnesses are required, and that sets the floor

**Claude Code and Codex are must-haves; anything further is upside.** That is a
requirement rather than a preference, and it has a sharper consequence than it
looks.

**The intersection of what two harnesses can do is the guaranteed floor. The
union is the best case.** Design the declaration for the union and render to each
harness's capability, and the difference between them shows up as cost rather
than as a missing rule — which is the degradation property above, arriving as a
concrete requirement rather than an aspiration.

**What must be verified before the floor can be stated honestly:** what each
harness offers for context files and imports, whether either has an interception
point equivalent to a pre-tool hook, what survives a context reset on each, and
whether skill-style progressive disclosure is available on both. **These change
often enough that the answer must be dated**, and a design resting on an
unverified capability is a design resting on nothing.

**The floor also decides what `blocking` can mean.** If only one of the two can
intercept an action, then blocking is either not a portable obligation level or
it degrades to *loaded at session start and hoped for* on the other — and those
are very different promises to make to somebody writing a rule about credentials.

## Open questions that decide it

**Four of these now have answers or working positions, recorded inline.**

1. **Should the model ever read the index directly?** *Working position: it should
   normally not, but it must remain able to.* The consequence is specific — see
   below, because it decides how much has to be built before anything is usable.
2. **Which form is the source: the human one or the machine one?** Open. If the
   primary reader is a person, the index should be legible first and the
   structured form derived from it; if a tool, the reverse. Hard to unwind, so
   worth deciding deliberately rather than by accident.
3. **Is `mandatory` rare?** *Superseded.* The question assumed one kind of
   mandatory. With obligation and applicability separated, the real question is
   **how many rules are binding *and* have no usable trigger** — because only
   those cost a session anything. That number should be small, and if it is not,
   the content wants rewriting rather than the mechanism.
4. **How much harness-specific machinery is acceptable?** *Answered by
   construction:* as much as wanted, provided it lives in a renderer and its
   absence degrades cost rather than correctness. It stops being a values question
   once the guarantee is preserved without it.
5. **Push or pull for the progressive class?** *Both, and the trigger vocabulary
   decides which.* Mechanical triggers are pushed because the system can detect
   them; `topic` is pulled because only the model can recognise it. Neither covers
   the other's case, so a design with only one of them has a hole.
6. **What is the authored-through content, exactly?** Open. For a tool-owned index
   this decides whether it merges cleanly, and it is the part that cannot be
   rebuilt from the filesystem.

### What the answer to question 1 actually changes

Recorded because *how does this change the approach* is the useful form of the
question.

| | **model never reads it** | **model reads it** |
| --- | --- | --- |
| **richness** | unlimited — cost, evidence, rankings, sections | every field is a token fight |
| **shape** | optimised for parsing and for people | must be terse prose |
| **rich data** | lives in the index | needs a second artifact anyway |
| **first shippable unit** | **index *plus* a delivery mechanism** — an index alone reaches nobody | **the index alone** works on any harness, including one with nothing |
| **failure mode** | delivery not installed, model gets nothing | costs tokens forever |

**That bottom-left cell is the reason the fallback answer is the right one.**
Committing to *the model never reads it* means nothing is usable until a hook or
a tool is also built and installed — on both required harnesses. Committing to
*readable, but not normally read* means the floor on any harness is never zero:
worst case, a model opens the index and finds its way.

**It is achievable without compromise**, which is why it is not a fudge. Keep the
model-legible index small — a line per bundle at ring 1, a line per document at
ring 2 — and put cost, evidence and rankings in a sidecar the model is never
pointed at. Two artifacts, one derived from the same walk, and the expensive one
is never in anyone's context.

## The ceiling on all of this

**Every mechanism here buys presence. None of them buys compliance.** A document
can be in the window, correctly loaded by the strongest available mechanism, and
still not be applied at the moment it mattered — attention is finite, position
matters, and a rule read at turn five is not reliably applied at turn eighty.

**This is a requirement rather than a caveat, because it ranks the mechanisms
differently than the rest of this document does.** If presence does not imply
compliance, then:

- **Just-in-time beats up-front, for attention reasons and not only for cost.**
  A short thing re-encountered at the moment of need outperforms a large thing
  loaded once at the start. That argues *for* an index and *for* loading late,
  and it is a second independent reason on top of the budget one.
- **`mandatory` is a weaker act than it reads as.** It buys presence, which is
  the weakest rung available. Anything whose violation is expensive and
  irreversible should not rely on it — a mechanism that *blocks* is a different
  category from a mechanism that *informs*, and choosing prose for such a rule is
  a decision to accept the risk.
- **The mechanism a rule deserves is evidence, not declaration.** A rule violated
  repeatedly while sitting in context has demonstrated that context is the wrong
  mechanism for it. That is duty 6 again, and it is the only thing that can settle
  these arguments rather than continuing them.

Explored at length in [silent-presence.md](silent-presence.md), which is worth
reading for the enforcement ladder — cited as reading, not as settlement.

## Out of scope, deliberately

- **What decides between the candidates.** The purpose here is to hold them all
  open with their properties written down. None has been measured against a real
  project, and measurement is what should decide.
- **Two loaded documents contradicting each other**, and **more being loaded than
  fits**. Both are failures that happen *after* content arrives, and both are
  scored against the acquisition designs in
  [adoption-use-cases.md](adoption-use-cases.md). The budget duty above is the
  second of them seen from the loading side.

## Separable: the verb for what `outfit` does

**Lifts out whole if this document is split.** Captured here because it surfaced
in the same session.

The act is currently called *projection*. The problem is not that the word is
obscure — it is that **`project` is already this ecosystem's most load-bearing
noun**: `type: luma/project`, `.luma/project.md`, `consumers: [project]`,
`scope: project`, `project_root`, and a `project` module in foreman. The
projection module's own opening line reads *"Projecting what a project adopted
into what its harness actually reads"* — both senses, six words apart. That is
the two-meanings-of-one-word tax already refused twice, for `policy` and for
`store`.

**Five slots a replacement has to fill**, because the current word fills all
five:

| slot | current |
| --- | --- |
| imperative | "**project** the bundles" |
| state | "foreman's own repository is **projected**" |
| the failure | `unprojected` — an `inspect` finding, in code |
| the artifact | "the **projection**" — there are two, skills and the index |
| re-run | "then **reproject**" |

**Candidates considered.** *Collapse into `outfit`* — one word for the command
and the act, nothing to learn, weak only in the noun slot. *`equip`* — wins the
failure slot outright, since `unequipped` is a word and `unoutfitted` is not, but
implies renaming the command. *`brief` / "the briefing"* — the best noun
available and a useful reframe, since the object is arguably the agent rather
than the repository, though *brief the project* reads wrong.

**Ruled out with reasons, because these are what people reach for.** `load`
collides with the load class and with what the harness does — foreman writes
files, the harness loads them, and the distinction matters most exactly here.
`install` is what adoption deliberately is not. `publish` belongs to the catalog.
`present` already means *in context*. `wire up` is taken twice in-estate: tools
are *wired into a harness*, and the curator was *wired to nothing*.

**Cost of the rename.** Roughly twenty-five occurrences in foreman across code,
tests, README and changelog, and about nineteen across six published bundles —
which moves versions on `luma-tools`, `session-manager`, `project-documentation`,
`bundle-manager` and `luma-maintainers`. Unlike the `policy` rename there is **no
fail-open hazard**: no pattern matches on the word.

**One casualty worth noting.** The three-noun list *adoption, projection,
inspection* loses its parallel. Stating it as the commands — *adopt, outfit,
inspect* — is better anyway, since those are the words somebody types.
