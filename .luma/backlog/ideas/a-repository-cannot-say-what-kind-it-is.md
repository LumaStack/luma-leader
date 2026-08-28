---
type: luma/idea
title: A repository cannot say what kind of consumer it is, so nothing keys on it
created: { by: human:benlinton, at: 2026-08-27T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle_status: draft
---

# A repository cannot say what kind of consumer it is

**Three mechanisms key on what a consumer declares itself to be, and nothing
defines where a consumer declares it.** `consumers` on a Bundle, `starters` and
`requires` on a `luma/catalog` all narrow by consumer kind or tag. `luma/catalog`
says *"a consumer states what it is"* and never says where. `luma/project` has no
field for it. A project's configuration has no field for it. No engine reads one.

**So all three are currently decorative.**

## The evidence that nothing is checking

`luma-foreman` is a project by `the-estate`'s own table. It has adopted
`lumastack/luma-catalog/luma-maintainers`, which declares `consumers:
[organization]`. Nothing noticed, because nothing could.

## Where the declaration belongs

**`.luma/PROJECT.md` frontmatter, as a new field on `luma/project`.**

**Not `type`.** `type: luma/project` is the *document* type — it says this
document is a project descriptor, in the same slot as `type: policy`. An
organization's headquarters has a descriptor too, and it is also a
`luma/project`. Overloading `type` is what would produce the confusing sentence
*this is not a project, it is an organization*.

**Not a tool's configuration file.** What kind of repository this is a fact about
the repository, read by something outside it deciding relevance. That is exactly
the descriptor's stated job, and a config file holds what a tool *overrides*.

Vocabulary matches `consumers`: `project` or `organization`, open to more —
see [[a-third-kind-of-consumer]], **which assumes this field works and is
therefore blocked on it.**

## Rejected: widening `consumers` instead

The shape considered was `internal-projects`, `any-projects`,
`internal-organization`, `any-organization`.

**It is a cartesian product of two questions**, so a third kind turns four values
into six. **And it does not achieve the exclusion it is for** — a value on the
Bundle cannot exclude anything unless the repository declares what it is, at
which point the declaration is doing the work and the prefix is redundant with
it.

## Three questions currently wearing one field

| | question | mechanism |
| --- | --- | --- |
| **floor** | would this Bundle even function in a repository like mine? | `consumers` |
| **fence** | is this mine to take, or somebody's internals? | **none** |
| **selection** | of what fits and is mine, what do I want in front of me? | **none, and it is the adopter's judgement** |

**The fence has no mechanism, and `luma-maintainers` reached for `consumers`
because it was the only audience field in existence.** Its `consumers:
[organization]` is justified in its own prose as *"adopting it into a project
would be adopting somebody else's internals"* — which is a fence argument, not a
floor one. Its body meanwhile tells estate repositories to adopt it, and those
are projects. *Filed against `luma-catalog` separately.*

`tags` cannot serve as the fence as specified: it narrows a catalog's `requires`
and `starters` — obligations to adopt — and no field anywhere says *only
repositories like this may adopt me*.

**Selection is deliberately left without a mechanism.** `the-estate` already
records it as missing, and it is a judgement exercised per repository. Inventing
one in passing would be worse than the gap.

## What to build, in order

1. **The declaration.** A shared-type change to `luma/project`, so it goes
   through [[change-a-shared-type]].
2. **A mismatch check.** An engine reports an adopted Bundle whose `consumers`
   excludes this repository's kind. Cheap, needs no catalog round-trip, and would
   have caught the case above.
3. **`starters` and `requires`**, keyed on kind and tags. Already designed, wholly
   unbuilt.

**Only after 1 can a fence be designed**, if one is wanted. The minimal shape
would be one optional field on a Bundle keyed on tags the repository declares —
absent means anyone — which keeps `consumers` at its two clean values and puts
the restriction where it travels with the Bundle between catalogs. **One case is
thin evidence for a field, though**, and the prose in `luma-maintainers` already
states the restriction emphatically.

## Notes

**The naming worry is separable and should not block any of this.** Once the kind
is its own field, `type: luma/project` stops looking like a claim about the
repository. What remains is `project` meaning both *any repository's descriptor*
and *one of two kinds* — real, small, a rename across published Bundles, and
worth leaving until the mechanism exists and it has actually tripped somebody.

**This is the prerequisite for more than it looks like.** It blocks
[[a-third-kind-of-consumer]], it blocks `starters` being offered at `init`, and
it is what would let a catalog rather than an engine decide what a new repository
begins with — which is the alternative to compiling a default Bundle into a tool.

**Reported from `luma-foreman`, 2026-08-27**, where the mismatch was found while
auditing which Bundles that project should carry. It reached here because
somebody went looking. See [[no-channel-for-what-a-project-learns]].
