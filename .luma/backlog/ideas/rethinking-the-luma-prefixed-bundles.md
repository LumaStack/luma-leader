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
minimize progressive disclosure and loading things into context that you will
never need. We don't want the core bundles to be bloated, or to become a junk
drawer of stuff you might need.

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

This is probably where guides and tutorials should live. **But I'm not sure if
that should be `luma-help`.** That is another debate: does luma help live in
luma core, luma ecosystem, or on its own?

### On maintainers

`luma-maintainers` is things that help us work and run the tools from a
maintainer's point of view. **It should be an additional layer, never a
replacement, and it should never crash into the other `luma-` bundles.**

### On types

`luma-types` — schemas to vendor. This should go into luma core, or be its own
thing. I'm not sure. **I think we might want it separate only because we don't
want core to get version noise every time a schema changes.**

---

## Open questions, carried deliberately

1. **Does install / init / get / apply belong in core or ecosystem?** Core makes
   the most sense and gives core double duty — installing *and* running.
   Explicitly deferred for later evaluation.
2. **Where does `luma-help` live** — core, ecosystem, or its own bundle?
3. **Is `luma-types` separate on its own merits, or only to avoid version
   noise?**
4. **What keeps core aggressively small** once help, guides and tutorials all
   have a claim on it?

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
