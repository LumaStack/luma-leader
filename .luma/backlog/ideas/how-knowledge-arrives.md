---
type: document
title: How knowledge arrives
description: The target state for getting knowledge in front of an agent — three transports, rings fired by entrypoints, and one integrity rule. Designed from the requirement rather than from what exists.
lifecycle: draft
created: { by: human:benlinton, at: 2026-08-27T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-27T00:00:00Z }
---

# How knowledge arrives

**This describes where we are going, not how we get there.** It is written from
the requirement rather than assembled from the mechanisms that already exist,
because those accumulated as separate experiments and compete with each other.
Several will be deleted.

**It supersedes the conclusions of
[loading-mechanisms.md](loading-mechanisms.md)**, which remains as the reasoning
that produced them — read it for *why*, not for *what*.

## The requirement

**An agent working in a repository should have the knowledge that bears on what
it is doing, and pay nothing for the rest.**

And the half that is easy to forget: **it should be impossible for relevant
knowledge to exist without the agent knowing it could be had.** Silence and
absence must not look alike.

## Three transports, and there are no others

A piece of knowledge is in an agent's context or it is not. There are exactly
three ways in:

| | | |
| --- | --- | --- |
| **1. Always there** | loaded when the level it belongs to opens | costs on every open, so it must be small |
| **2. Fired** | a trigger matches and it arrives, whether or not anybody thought to ask | the only guaranteed path |
| **3. Asked for** | requested by name, by the agent or by a person | never fails, always available |

**Everything else is a detail of one of these three.** A design that grows a
fourth is describing an implementation, not a transport.

## Rings and entrypoints

**A ring is one level of knowledge. An entrypoint fires a ring.**

**Each ring is named with its number, as one token**, because the whole point of
a ring is that it is ordered and a bare name loses that. Write `2-bundle`, never
*the bundle ring*.

| ring | holds | fired by |
| --- | --- | --- |
| **`0-catalog`** | bundles you have **not** adopted | asking what else is out there — *not built* |
| **`1-project`** | what this repository knows | the harness, at session start |
| **`2-bundle`** | what one bundle holds | reaching for that bundle |
| **`3-document`** | what one document holds | opening that document — *not built* |
| **`4-section`** | content. The leaf, with no map of its own | reading it |

**Numbers go inward**, so what is outside everything you have is `0`.

**The rule that makes the sequence self-checking: a ring's map names the members
of the next ring in.** `1-project` names bundles, `2-bundle` names documents,
`3-document` names sections. Nothing to memorise beyond that.

**`0-catalog` is the one you do not own**, and it is different in kind rather
than merely in position: its contents live somewhere else, change without you,
and reaching it may need the network. It is also the only ring that answers *you
cannot ask for a bundle you have never heard of* — every other ring assumes the
thing is already in your repository. That gap is real and nothing addresses it.

**Firing a ring delivers two things and only two:**

- **what is always true at that level** — bodies, present immediately
- **the map of the next ring down** — every item's name, what it is for, and what
  fires it. Names and reasons, never bodies.

That is the whole mechanism, and **it is identical at every level.** `1-project`'s
entrypoint gives you the rules that hold everywhere plus a map of bundles.
`2-bundle`'s gives you the rules that hold throughout that bundle plus a map of
its documents. Nothing new is learned going down a level.

### `matches` decides which of the three a document gets

`matches` is the one declaration an author writes, and the three outcomes are
computed from it rather than named separately:

| `matches` | transport |
| --- | --- |
| `always` | **always there** — present whenever its ring is fired |
| a list of triggers | **fired** — arrives when one matches |
| `nothing`, or absent | **asked for** — on the map, body waits |

**`always` is scoped to its ring, and that is the point.** A rule declared
`always` inside a CSS bundle is present the moment that `2-bundle` ring is fired
and not before. It is not a permanent seat in every session — only what is
`always` in `1-project` gets that, which is why `1-project` must stay small and
why nothing else can inflate it.

**This is what makes depth cheap.** Standing cost is one line per item on a map,
paid once per ring you actually open. Bodies are paid only where you went.

## The map is the load-bearing half

**Every item in a ring gets a line on that ring's map, whatever its transport.**
A rule nobody can see governs nothing, and an agent cannot ask for what it does
not know exists. Existence is cheap; content is expensive.

A map line says what the item is for and what fires it. **It is not a summary of
the content** — it is what somebody needs in order to decide whether to open it.

## The integrity rule

**Nothing may exist that no transport can reach.**

Depth is fine and expected: an item reached through a ring, through a trigger, or
through a link from something itself reachable is properly reached, however many
hops that takes. **Being buried is by design.**

**Being unreachable is a defect.** Nothing gets force-loaded to compensate — a
tool reports it, and a person decides whether the indirect path is acceptable or
the item should be linked, triggered, or deleted.

## Adapters, and their one obligation

**This design ends at rings, entrypoints and `matches`.** How a ring reaches a
particular harness — a file it loads at startup, a skill, a hook, a command — is
an adapter's business, and adapters differ because harnesses differ.

**An adapter has exactly one obligation beyond rendering: do not render what the
harness already carries by another route.** Two renderings of the same fact, for
the same reader, is a cost paid twice for nothing. An adapter that cannot tell
what its harness already has will pay it.

## Deliberately not in this design

**No priority ladder.** There are three transports and `matches` chooses between
them. Any scheme with more levels than that has levels nothing can act on
differently.

**Nothing about what a repository adopts.** *What knowledge is here* and *how it
reaches an agent* are separate problems that sound alike. This document is only
the second. `starters` belongs to the first, is **deprecated until further
notice**, and returns as an idea that has to earn its place.

**No condition language.** A trigger vocabulary that a hook can evaluate and a
person can read is enough. Anything expressive enough to need operators is a
programming language arriving through a side door.

## What has to change to get here

Named for scope, not planned — the migration is its own pass.

- **`always` becomes ring-scoped** rather than session-scoped.
- **Bundles gain a real map of their own contents**, which is what a bundle's
  entrypoint fires.
- **`1-project` moves out of any single harness's file** so more than one adapter
  can point at it.
- **The reachability check gets built**, because the integrity rule is worthless
  unannounced.
- **`starters` comes out** of the catalog type and the live catalog.
- **Several existing mechanisms get deleted rather than migrated.** Which ones is
  the next pass.
