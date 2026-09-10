---
type: luma/idea
title: Rethinking the luma-prefixed bundles
created: { by: human:luma-foundry, at: 2026-09-10T07:20:00Z }
contributors: [human:luma-foundry, agent:claude-opus-5]
scope: organization
stage: draft
---

# Rethinking the luma-prefixed bundles

**Rethinking the luma-prefixed bundles — what do we actually need?**

**What we have so far is not well thought out and will need to be reworked. It
is all experimental so far.** See `luma-catalog/catalog/bundles` for the current
set.

---

## As proposed

*Kept as written — spelling corrected, wording untouched.*

I think we will need shared types. But whether it should run in its own bundle
or not, I'm not sure yet.

I can't prove it, but I think we will need:

- **luma-user** — stuff to help users navigate what users need to know to use
  and install the tools. Probably, maybe not. Maybe this is called
  **luma-ecosystem** and it's what you need to embrace the full ecosystem of
  tools, if you want to have everything integrate together.
- **luma-maintainer** — has to be additive to luma-user, never contradicting or
  a replacement.
- **luma-core** — stuff that is always present, what you would need to use a
  single tool.
- **luma-essentials** — the bare minimum?
- **luma-types** — these are shared type definitions or schemas, that can't live
  in a single bundle because many bundles reference them.

---

## The discussion

*Also as written — spelling and formatting corrected, wording untouched. This is
the thinking, including the parts still unresolved.*

### The lens

We need to think of different use cases for `luma-` bundles, and how we can
**optimize progressive disclosure** and **minimize loading things into context
that you will never need**. We don't want the core bundles to be bloated, or to
become a junk drawer of stuff you might need.

*Corrected by the author from "minimize progressive disclosure": the verb
belonged to the second clause. We want as much progressive disclosure as is
useful — optimized rather than maximized, since each additional layer is also a
layer somebody can fail to descend.*

That is the exercise we must do. We need to look for the perfect design, not
anchor on what we have. Think green field.

### On core

Should luma config live in luma core? I think so, but I'm not sure. I think
config, layout, and essentials should all live inside core.

I don't know if you will ever be able to grab layout and not need config.

**So core lets you use one tool, and without it the tool won't work well. It
needs to stay aggressively small and tight.**

**I'm not sure if it should include install, init, get/apply.** It makes sense,
but now it's got double duty: getting stuff installed *and* running it. I was
thinking core was just running it — but maybe installing it is core too.
Interesting. But then I can see core getting bloated.

Yeah, I think install / init / get / apply might go in the ecosystem. Hmmmmm —
**capture this and let's evaluate it later.** Putting it in core makes the most
sense, but I'm trying to keep it exclusive and tight; when I think of everything
that could go in core it gets huge — help, guides, tutorials, etc, etc, etc.

### On ecosystem

`luma-ecosystem` is basically a new term for `luma-tools`, and I like it better.

**Ecosystem unlocks you from using one tool to using all of them, and how they
should interact with each other** — how the tools can chain together, workflows
for using the tools in sequence.

**`luma-ecosystem` is what will make the tools feel like they are aware of each
other, and then know when and how to use each other correctly. It should feel
like magic when that bundle is loaded.**

This is probably where guides and tutorials should live. **But I'm not sure if
that should be `luma-help`.** That is another debate: does luma help live in
luma core, luma ecosystem, or on its own?

**Naming — the top three choices should be `luma-ecosystem`, `luma-tools` and
`luma-estate`.**

### On maintainers

`luma-maintainers` is things that help us work and run the tools from a
maintainer's point of view. **It should be an additional layer, never a
replacement, and it should never crash into the other `luma-` bundles.**

### On types, and the glossary

`luma-types` — schemas to vendor. This should go into luma core, or be its own
thing. I'm not sure. **I think we might want it separate only because we don't
want core to get version noise every time a schema changes.**

Where should the luma glossary live — is that luma core? Or is that the
ecosystem? It might also go in maintainers.

**Resolved in discussion: it goes with the types.** Definitions that more than
one thing must agree on already have a home, and a glossary is the same shape as
a schema — agreement about meaning rather than agreement about form.

### On naming the definitions bundle

Should `luma-types` become `luma-schemas`? Or is there a better way to say *this
is how we shape data* that is more universal than just types?

`luma-data`?

**`luma-definitions` is good.**

*Still open. `luma-definitions` is the leading candidate and has not been
chosen — and the naming exercise may itself be moot; see below.*

### On separating terms from schemas

I am wondering if we should stop trying to combine terms — basically the
foundation of design principles — and schemas / types / contracts into one
bundle.

### On where the foundations of design live

Where should the foundations of design live? I think the glossary will be tapped
when maintainers work, but also maybe when the ecosystem needs to interact and
help orient itself, and also when trying to help the user use the correct terms.

This should go in core or ecosystem. **The only bad thing about putting it in
core is that now you're letting core know about the full ecosystem, if you're
not careful.**

### On spreading vocabulary across the layers

*Responding to the proposal that each layer defines the terms its own material
uses:*

| layer | defines | because it's the material that uses them |
| --- | --- | --- |
| **core** | `bundle`, `adopt`, `apply`, `catalog`, `consumer`, `project`, `.luma` | core's own documents use these |
| **ecosystem** | `chaining`, `sequencing`, handoffs, what each tool is for | only mean something once several tools are in play |
| **maintainers** | `estate`, `publishing`, retirement, promotion | authoring vocabulary |

**I'm not sure I want vocabulary spread all over the place — that might make it
hard to maintain. But it makes sense from a usability standpoint.**

**I also kind of want to keep the vocabulary in one place so I can easily see
when words collide.**

**I think vocabulary should have one place it lives. And then we can generate it
into the places it belongs, as a pipeline. So we get the best of both.**

**And we have to write it in such a way that `luma-core` understands some of
these things won't be present. Defined vocabulary does not tell you whether
something is present and available.**

### On what the vocabulary bundle should hold

**If we are going to create a new bundle for it, then I think luma vocabulary
should also include design principles and all that — maybe even the vision and
stuff.**

**Maybe the vocabulary is one piece of the architecture. Maybe
`luma-architecture` or `luma-approach`.**

### On why we split at all

I want to capture **why we split things, and how the division earns its keep.**
We should always have good reasons why we aren't combining it all.

**The more we split things, the harder it is to know what's present and what's
missing** — so there are more surface areas to test.

**So we should have good reason for stuff splitting.**

---

## Open questions, carried deliberately

1. **Does install / init / get / apply belong in core or ecosystem?** Core makes
   the most sense and gives core double duty — installing *and* running.
   Explicitly deferred for later evaluation.
2. **Where does `luma-help` live** — core, ecosystem, or its own bundle?
3. **Is `luma-types` separate on its own merits, or only to avoid version
   noise?** — separate, and it takes the glossary too. **Its name is still
   open**; `luma-definitions` leads and is not chosen.
4. **What keeps core aggressively small** once help, guides and tutorials all
   have a claim on it?
5. **The awareness circularity.** Ecosystem's job is to make you aware the other
   tools exist — but it is the layer *above* core, so the adopter who most needs
   that awareness is the one who has not adopted it. **The bundle that tells you
   the other tools exist is the one you only adopt once you know they exist.**
   Two ways out: the capability map lives in core while integration detail lives
   in ecosystem, or core and ecosystem merge and the question dissolves.

---

## Commentary

*Agent commentary and measurements. None of this modifies the sections above.*

### The physics, measured rather than assumed

Measured in `luma-foreman`, which carries nineteen adopted bundles:

| | tokens | conditional? |
| --- | ---: | --- |
| project `INDEX.md` — one line per bundle | 1,082 | **no** |
| 15 documents declaring `matches: eager` | 37,804 | **no** |
| everything else across nineteen bundles | 0 until triggered | yes |

**`eager` is 97% of the permanent cost; bundle count is 3%** — roughly 57 tokens
per bundle line. **Splitting a bundle to keep it lean raises the floor**, since
each new bundle adds an unconditional index line while the documents inside were
already free.

The current `luma-*` set demonstrates both extremes at once. `luma-config`,
`luma-layout` and `luma-maintainers` cost **7,089 tokens in every session,
forever**. `luma-tools` and `luma-types` cost **zero**. Same shelf, same
authors — the entire difference is `matches`. All three expensive ones declare
`matches: eager` with **no justification beside the declaration**, which
`organizing-a-bundle` requires, so eager is being defaulted into rather than
chosen.

### Two boundaries, currently conflated

- **Adoption boundary** — what gets copied into a repository together. Governed
  by *would anyone adopt half of it?*
- **Loading boundary** — what enters context together. Governed by `matches`.

**"Core gets bloated" is a fear about the second, solved at the first.** A
nine-document bundle with zero eager documents costs one index line. Bloat, in
the sense that matters, is a `matches` failure — and it cannot be fixed by
drawing a boundary somewhere else.

**What the boundary *does* control is bytes vendored into a repository that will
never want them.** That is a real cost and a real reason to split. It is just
not the context cost, and keeping the two apart is what stops the design being
argued in the wrong currency.

### Combining is the default; splitting carries the burden of proof

**The question is never "should these be separate." It is "what does the
separation buy, and is it worth what it costs."** A split that cannot name which
of the five criteria it satisfies is a split that has not earned its keep.

That inverts the usual instinct, which is to start separate and merge if it
hurts. Here the costs of separating are permanent and the cost of combining is
recoverable — **you can always split later; you can rarely un-drift two copies.**

**Three costs, all paid forever, none of them obvious at the moment of
splitting:**

**1. Every boundary is a duplication boundary.** Bundles have no dependencies,
so a rule needed on both sides has to be copied. `organizing-a-bundle` concedes
it directly: *"where several bundles need the same rule, each carries its own
copy… copies that drift are a finding, not a merge."* More bundles, more copies,
more drift.

**2. Every bundle raises the context floor.** Each adds an unconditional line to
every adopter's index — about 57 tokens measured — while the documents inside
were already free until triggered. **Splitting to reduce context cost increases
it.**

**3. Presence becomes unknowable, and nothing can check it.** This is the one
that compounds worst. **N bundles produce 2^N possible adoption states**, and
because nothing declares a dependency, **nothing can refuse an incoherent one**.
Ecosystem adopted without core does not error; it simply reads oddly, forever,
and no tool reports it.

So every reference across a boundary becomes a question — *is that bundle here?*
— that a reader cannot answer from inside a document, and a writer cannot answer
at all. **`organizing-a-bundle` asks that a reader be able to tell a boundary
from a gap, and that only works while the reader knows which boundaries exist.**
Past a handful of bundles, they do not.

**This is the same failure as *defined is not present*, one level up.** A term
that names a capability does not tell you the capability is installed; a bundle
that names another bundle does not tell you it was adopted. Both are silent, and
both get worse with every additional piece.

**It is also why the vocabulary decision came out the way it did.** One source
generated outward beats distributed sources for exactly this reason — it
collapses the states rather than multiplying them.

### The entry-point document: the rule, not the roster

**`luma-architecture`'s first document should be what becomes a bundle and what
does not — not a list of the bundles that currently exist.**

**A roster is derivable and rots.** The bundles and their descriptions already
live in two generated places: a project's `INDEX.md`, loaded every session, and
`CATALOG.md`. A hand-written third copy drifts the moment a bundle is added,
renamed or merged — and renaming is this estate's most expensive operation. It
is also the same rot as counting things in a heading: *"three categories"* fails
when there is a fourth, and a table of five bundles fails when there is a sixth.

**The rationale is what is not derivable** — *split off for change rate*,
*strictly additive*, *the substrate one tool needs*. No tool can compute those.
That is the architecture.

**The division, using machinery that already exists:** the index says *what
exists*; architecture says *why it is cut that way*. And the criteria answer the
question a reader actually arrives with, which is **where does a new thing go** —
a roster cannot answer that at all.

**Two constraints on writing it.** Bundles are named in prose, never linked —
*"a wikilink or a path into another bundle breaks self-containment and will be
reported."* And if a roster is wanted anyway, **generate it**: same pipeline as
the vocabulary, banner-marked, which makes it a second customer for that
machinery.

### The five criteria, and the one that is not a criterion

**Split a bundle for one of these five. Name which one.**

1. **Adoption** — *would anyone want half of it?* The bytes get vendored into
   every adopting repository, so material some adopters will never want is a
   real cost paid by them. `create-bundle` already carries the test: *if
   adopting half would leave someone with rules and no procedure for following
   them, it is one bundle.*

2. **Change rate** — *do these move at different speeds?* Version numbers are
   per bundle, so slow material bound to fast material makes every adopter read
   changelogs about things they do not use. **This is what separates the schemas
   from everything else**, and it is an argument neither adoption nor loading
   can make.

3. **Ownership** — *do different people edit these?* Separate bundles let each
   move without contending with the other. Weakest of the five here, since the
   estate has one maintainer, but real in an organization.

4. **Lifecycle** — *does one half need to promise more than the other?* `stage`
   is a single value per bundle, so **you cannot promise stability for half of
   one.** Material heading for `stable` cannot share a bundle with material that
   will be `draft` indefinitely — which is why a forming vocabulary and a
   versioned schema pull apart.

5. **Trust** — *do these carry different provenance or licence?* A bundle is the
   unit of adoption, so it is also the unit of vouching and of licensing. See
   `what-a-bundle-may-carry` in `bundle-manager` for what a carried licence does
   to every adopter downstream.

**And the one that is not a reason: "it would get too big to load."** That is
`matches`, not a boundary. Splitting for context cost **raises** the floor — each
new bundle adds an unconditional index line, while the documents inside were
already free until triggered. Bloat is a loading failure and cannot be fixed by
drawing the boundary somewhere else.

### Change rate is a third axis, and it is the strongest argument in the thread

**The version-noise argument for keeping `luma-types` separate is the best
reason in this discussion, and it is not an adoption argument.** Things that
change at different rates want different version numbers: a schema edit should
not make every core adopter read a changelog about something they do not use.

That is a genuine third criterion beside adoption and loading, and it has not
been written down anywhere in the estate. **It probably belongs in
`bundle-manager` regardless of what happens to the `luma-*` set.**

### `luma-essentials` did not survive the question *what file is it?*

If core carries no eager documents, then the bare minimum is **the index
ordering plus one entry-point document**. There is nothing left over to be a
bundle or a directory — it was describing a reading order, and a reading order
lives in the index.

### `layout` + `config` is settled by an existing test

`create-bundle` step 1: *would anyone adopt half of it? … If adopting half would
leave someone with rules and no procedure for following them, it is one.*
`luma-layout` alone is a description of a directory nothing fills; `luma-config`
alone is precedence rules for files nothing reads. Both are the "it is one"
case, and the layout bundle's own description concedes it — *"the `.luma`
directory every luma tool writes into."*

### On install / init / get / apply — the deferred question

The double-duty instinct is picking up something real, but it may be **two
questions wearing one name**:

- **`install`** is about a *machine* — getting binaries onto a laptop. It is
  true before any repository exists, and it is the same for every project.
- **`init` / `get` / `apply`** are about a *repository* — they operate on
  `.luma/`, which is the thing core already describes.

Split that way, `init`/`get`/`apply` are core by the same argument that put
layout and config there: they are the procedures for the rules core already
holds, and a rule with no procedure is the merge case. **`install` is the one
that genuinely sits outside**, since it precedes the repository entirely — which
would make it ecosystem's, or its own small thing.

That reading keeps core tight without moving the operational half out of reach.
Recorded as a candidate resolution, not a decision — the question is
deliberately open above.

### On `luma-help`, and the categories crowding core

`help`, `guides`, `tutorials` and `getting started` are the same five categories
raised in [[classify-bundle-contents]], and they are what makes core look like
it will get huge. Two things bear on it:

**Tutorials already have a home and a shape.** `luma/tutorial_step` and
`luma/tutorial_quiz` are real types, and `token-manager` runs a twenty-step
tutorial as `procedure/token-tutorial/` with `steps/` beneath it — **a tutorial
is a procedure that owns a directory**, not a top-level category. That precedent
answers most of *where do tutorials live*: inside whichever bundle they teach.

**And explanatory material is the cheapest thing to hold**, because it is the
easiest to leave un-eager. Help that never loads until asked for costs one line
in the bundle index. So the pressure it puts on core is about vendored bytes and
findability, not context — which is the distinction above, and probably means
this is a smaller problem than it feels.

### "It should feel like magic" is a loading requirement, and it costs something

**Cross-tool awareness cannot be triggered.** A rule you might break has a
trigger — the action that would break it. **A capability you do not know exists
has none**, because nothing you do points at it. An agent finishing a piece of
work has no reason to look up whether a tool exists for the next step, so the
integration never fires and the magic never happens.

That is the same shape as the one exception `organizing-a-bundle` already
allows: a register of retired words, *"useless unless it is present before
somebody uses one."* So there are **two** legitimate reasons to spend always-on
tokens, not one:

1. **A violation that cannot be undone** — a published credential.
2. **A capability nobody would think to look for** — this.

**The cheap way to buy it is the index line, not an eager document.** The line is
always on and costs about fifty tokens, so an ecosystem description that *names
the capabilities* — rather than describing the bundle — may create the awareness
on its own, with every detail still triggered. Something closer to *"foreman
adopts knowledge, backlog holds intended work, clarify resolves ambiguous
requirements — and how each feeds the next"* than to *"knowledge about the luma
ecosystem."*

**If that is not enough, the fallback is a deliberately thin map** — the tools,
one line each, and when each becomes relevant — and nothing else eager. What must
not happen is the whole integration story going eager to buy an awareness that a
sentence would have bought.

### Vocabulary: source and view are different questions

**"Spread out for usability" and "in one place so collisions are visible" are
not in conflict** — they are a source-versus-view problem, and this estate
already solves that shape. `INDEX.md` carries *"Generated by `luma-foreman
bundle index`. Regenerate, never edit."* Canonical content lives distributed;
the navigable view is derived.

**And collisions want a check rather than a location.** A single vocabulary file
lets somebody *see* a collision if they happen to look. A derived view lets a
tool *report* one, every run — which is what `inspect` already does for dangling
wikilinks and unreachable documents. **"Two layers define `consumer`
differently" is the same shape of finding**, and it is precisely the failure
`organizing-a-bundle` says nothing currently catches: *"both are conformant,
both validate, and nothing reports that a reader has to reconcile them by
hand."*

So the maintenance instinct is right, and a central file is the weaker way to
satisfy it: **a file is a place collisions are visible; a check is a place they
cannot be ignored.**

**Two costs, neither free.**

- **Terms must be typed, not prose.** A paragraph cannot be collision-checked.
  Each term becomes a small document — the word, the definition, the owning
  layer — which needs a `term` type, which is what the schema bundle is for.
  The pieces fit; it is still a commitment to structure over prose.
- **The collector does not exist.** Nothing gathers vocabulary today, so until
  it is built, distributed terms have exactly the invisible-collision problem
  and no compensating check.

**Superseded, and in the better direction.** The proposal above was *source
distributed, view derived*. The author inverted it: **source centralized,
distribution derived** — one canonical vocabulary, generated into each layer as
a pipeline.

**That is strictly better on the thing that mattered.** With one source and one
entry per term, **a collision is impossible by construction** rather than caught
by a check afterwards — there is nowhere for a second definition of `consumer`
to exist. Construction beats detection, and it removes the tool dependency the
distributed version was owing.

**And it keeps the leak closed**, because generation filters: core receives only
core's terms and never learns the word `chaining`. Each term declares its owning
layer, so the routing is data rather than a rule somebody applies.

**Critically, the generated copy ships inside the bundle**, which keeps the
bundle self-contained and adoption dependency-free. **The dependency moves to
authoring time, where it is allowed**, rather than adoption time, where it is
not.

**This is vendoring, automated.** The estate already does exactly this by hand
for type definitions — `luma-types` is the master and bundles carry copies, with
*"a vendored copy is a snapshot; record the version you took."* Vocabulary would
get the same relationship with a pipeline instead of a copy-paste. **Which
suggests types want the same pipeline**, since hand-vendoring is just the
un-automated version of it, and re-vendoring is currently a deliberate act
nothing reminds anyone about.

**What it still owes:**

- **A `regenerate, never edit` banner** on every generated copy, matching
  `INDEX.md`, plus a `--check` mode in continuous integration — `luma-foreman
  bundle index` already has exactly that pair.
- **A home for a source that is not itself a deliverable.** The canonical
  vocabulary is an authoring input rather than a bundle anybody adopts, and the
  estate has no established place for that. Probably the sharpest open question
  this design creates.
- **A `term` type** carrying the word, the definition and the owning layer.

### `luma-architecture`, and what it may hold

**A bundle can be both a deliverable and a generation source** — `luma-types`
already is, adopted as a bundle *and* the master others vendor from. So the
source-versus-bundle tension resolves: the architecture bundle is adoptable
whole by anyone wanting the full picture, and is what the vocabulary pipeline
generates filtered subsets from.

**Vision, principles and vocabulary pass the rise-and-fall test.** Vision
justifies the principles; the principles shape the vocabulary — `consumer`,
`catalog` and `bundle` mean what they mean *because* of vendored-not-resolved
and no-dependencies. Change the vision and the terms move. That is one thing
rather than three.

**`luma-architecture` beats `luma-foundations`, and specificity is the reason.**
`foundations` could hold anything, which was the objection to `luma-definitions`
too. Three checks pass:

- **No collision.** *Architecture* appears in the catalog only as an ordinary
  noun and is defined nowhere — unlike `estate` and `data`, both of which failed
  this.
- **It answers a filed gap.** [[no-format-for-non-procedural-knowledge]] names
  *"an architecture description"* as one of the artifacts with no home. The name
  is an answer to a recorded problem rather than an invention.
- **Vocabulary inside architecture is conventional.** Architecture documents
  routinely carry a glossary, so a reader is not surprised to find terms there.

**The caveat: vision sits upstream of architecture.** Architecture describes how
a thing is shaped *given* a goal; vision is the goal. In practice architecture
documents open with context and goals, so it is a small stretch rather than a
wrong one — **but check whether vision already has a home first.**
`project-documentation` governs published prose, and positioning currently lives
in READMEs. Pulling it into a bundle either duplicates it or moves published
positioning, which is a larger decision than filing a design document.

**Principles, structure and vocabulary are unambiguously architecture. Vision is
adjacent** — include it if homeless, reference it if not.

### The boundary that keeps it from becoming a drawer

The bundle holds the **why**; operational material stays with what it operates
on:

| belongs | does not |
| --- | --- |
| vision — what this is for | help — how do I do X |
| principles — why it is shaped this way | guides, tutorials — walkthroughs |
| vocabulary — what the words mean | procedures — steps to follow |

**Checkable by whoever adds the ninetieth document**, which *use good judgement*
is not. This matters because adding *"and the vision and stuff"* to a vocabulary
bundle is exactly the move the opening of this idea warns against — the bar has
to be a boundary rather than a judgement call.

### Defined is not present, and that has to be structural

**A definition teaches a word; it says nothing about whether the thing exists
here.** An agent that reads what a *work item* is has no way to tell whether
`luma-backlog` is installed, and the default assumption will be that it is — so
it reaches for a capability that is not there, or worse, describes it to a user
as available.

**The estate already has this rule one level up.** `organizing-a-bundle`:
*"Acknowledge, do not depend. A bundle may say the changelog is owned by the
release bundle without requiring it to be adopted. Nothing breaks if it is
absent — a reader can tell the omission is a boundary rather than a gap."* This
extends it from bundles to terms.

**Two kinds of term, and only one is safe to state flatly:**

| kind | example | what a definition claims |
| --- | --- | --- |
| **concept** | `adopt`, `bundle`, `consumer`, `project` | complete on its own — the word means this, always |
| **capability** | `work item`, `chaining`, a named tool | **names something that exists in the world, not something available here** |

**It has to be a field, not a phrasing habit.** A convention of writing *"may
not be present"* erodes across a hundred entries and cannot be checked. A term
that declares which kind it is can be rendered differently, checked
mechanically, and cannot be forgotten by whoever writes entry ninety-seven.

**And availability is genuinely checkable**, which is what makes this
actionable rather than a warning: adopted bundles are listed in
`.luma/bundles/MANIFEST.md`, and installed tools are on the path. So a
capability term can carry *how to find out* rather than merely *this might not
exist* — the difference between an agent that hedges and one that verifies.

**This is the same constraint that keeps the awareness line honest.** Core is
allowed to say other tools exist — that is what makes ecosystem discoverable —
but **naming a capability is not claiming it.** Both the awareness line and the
capability terms need the same discipline, and they fail the same way without
it: an agent confidently offering something the project does not have.

### Where a shared definition goes — partly retracted

**The conclusion below — that the glossary belongs with the schemas — was
withdrawn during the same discussion.** Two objections did it: a bundle name
that had to span schemas and terms kept not fitting after eleven candidates,
which is evidence the two are unlike; and running the merge against the five
split criteria gives **three of five saying split** — a validator wants schemas
and cannot read prose, schemas change mechanically while terms change when the
thinking changes, and schemas can be `stable` while a forming vocabulary is
`draft` for a long time.

**The table itself survives and is the transferable part** — it is where the
*general* rule came from, which is that definitions live with whatever
introduces the concept.

The glossary question is a special case of a general one, and the estate solves
it once already, in the same pair of bundles:

| | the definitions | the discipline for changing them |
| --- | --- | --- |
| **schemas** | `luma-types/_types/*` | `luma-maintainers/procedure/change-a-shared-type` |
| **terms** | *the glossary* | *the retired-words register, and how a word is retired* |

**Shared definitions are data everyone vendors; the obligation to keep them
coherent belongs to whoever maintains the tools.** That is why the glossary is
neither core nor ecosystem, and why the instinct toward maintainers was
half-right — **"glossary" is two artifacts.** A *reader's glossary* is
cross-cutting lookup and belongs with the definitions. An *author's register* —
which word we use, which are dead — is an authoring constraint, belongs to
maintainers, and **already exists**: six retirement records, and the catalog's
only `matches: always`.

The loading asymmetry falls out of the same split. A term you do not recognise
prompts you to look it up, so definitions are lookup. **A dead word prompts
nothing**, which is why the register has to be present before use — the same
reason cross-tool awareness cannot be triggered.

### Naming the integration bundle — and one collision to weigh

The three candidates, against the criteria the rest of this design uses:

| candidate | spans the job | collides with estate vocabulary | says what you are buying |
| --- | --- | --- | --- |
| **`luma-ecosystem`** | yes — the tools *and* how they relate | no | **yes** — you are opting into the whole thing |
| **`luma-tools`** | names the subject, not the relationships | no | no |
| **`luma-estate`** | yes, in principle | **yes — see below** | partly |

**`luma-estate` has a live collision, and it is the same shape as `luma-data`.**
`luma-maintainers/policy/the-estate` already defines *the estate* as **the six
repositories that build the tools, and the boundary each defends.** That is
maintainer vocabulary meaning *where the source lives*, and it is close to the
opposite of what this bundle is for — a user-facing map of tools that work
together. A reader who met `the-estate` first would read `luma-estate` as *the
repositories*.

**Recorded rather than ruled out.** It is a point against, not an elimination,
and the author may decide the word is worth reclaiming — but reclaiming it means
retiring the maintainer sense, which is a retirement record rather than a
rename.

**`luma-tools` is the safest and the flattest.** It describes the contents and
says nothing about the relationships, which is precisely the part that makes
this bundle worth having — *"it should feel like magic when that bundle is
loaded"* is not a claim `tools` makes.

**A pattern worth noticing.** This is the second naming candidate in one
discussion to collide with a word the estate has already defined — `luma-data`
against `luma-config`'s XDG sense, now `luma-estate` against `the-estate`. **The
vocabulary pipeline would make that a mechanical check** rather than something
somebody happens to remember: a proposed bundle name checked against the
canonical term list. That is a second customer for the same machinery, and an
argument for building it.

### Naming the definitions bundle: not `luma-types`, not `luma-schemas`, not `luma-data`

**The bundle name should not track a name being renamed downstream.**
`type_definition` is under active debate in two ideas at once —
[[rename-types-to-type-definitions]] wants the directory to become
`type_definitions/`, and `classify-bundle-contents` proposes the type become
`type_schema`. Both `luma-types` and `luma-schemas` would have to move again
when LKF settles; a name that does not reference the format's type system does
not.

**And the bundle is no longer only about data.** With the glossary in it, any
name built on *how we shape data* describes half the contents. What both halves
share is narrower and more durable: **the definitions more than one thing must
agree on** — schemas agree about form, a glossary agrees about meaning, and
breaking either fails quietly in the same way.

**`luma-data` was considered and rejected on a concrete collision.** `data` is
already a defined term in this estate: `luma-config` gives it a specific XDG
meaning — *things a program installs and manages* — with a section on choosing
between config, data and state, and there is a separate `data-files` idea about
something else again. **The bundle that holds the glossary should not be named
with a word the estate has already defined differently.** It is also inaccurate:
a schema is not data, it is the shape data takes.

`luma-contracts` was the close runner-up, and is the author's own phrase for what
a type definition is. Dropped for over-promising on prose — a glossary entry is
an agreement rather than a contract — and for colliding with the licensing
vocabulary this estate now carries.

**Keep the bundle name independent of the directory inside it.** The bundle name
answers *what is this collection*; `_types/` answers *what kind of file is this*.
Holding them apart means the live `_types/` rename does not drag the bundle name
with it.

### The structural risk, stated once

The design is now **three additive layers — ecosystem on core, maintainers on
both — and not one of them can declare it.** `CATALOG.md`: *"No bundle here
depends on another."* `organizing-a-bundle`: *"A bundle may reference another
for depth. Never for capability."*

So *never crash into the other `luma-` bundles* is achievable — that is
`acknowledge, do not depend`, and a collision means two bundles requiring
**different** things of the same path. But *additive, never a replacement* has
no mechanism behind it: nothing stops somebody adopting ecosystem alone and
getting a tool map for a substrate they do not have.

**This design is a bet that [[bundle-dependencies]] eventually lands**, and it
is better to record that as a dependency of the design than to discover it
later.

### One wrinkle in the core/ecosystem split

`what-each-tool-does` is currently the entry point — it is how a reader learns
`foreman` exists at all. Moved wholesale into ecosystem, a core-only adopter
never meets it. Likely resolution: core carries a short orientation naming the
tools, ecosystem carries integration, chaining and the endgame material. That is
an edit to a document rather than a filing decision.

### The highest-leverage thing in the whole design

With near-zero eager, **the one-line bundle description is the only text every
session pays for**, and it is the sole basis on which an agent decides to open a
bundle at all. Get those sentences right and the bundle count mostly stops
mattering; get them wrong and no partition rescues it.

### Related, already filed

- [[bundle-dependencies]] — the mechanism the layering assumes.
- [[classify-bundle-contents]] *(in `luma-foreman`)* — help, guides, tutorials
  and the rest, as a classification question rather than a placement one.
- `archived/starters` — named lists of bundles a consumer begins with,
  withdrawn 2026-08-27 because nothing declares what kind of consumer it is.
  **A `luma-core` bundle is not blocked by that withdrawal** — prose in a bundle
  needs no consumer support, where a catalog field did.
- [[a-repository-cannot-say-what-kind-it-is]] — why that blocker is still open.
