---
type: luma/idea
title: Committed derived material has no tier, and the cache deferral asked the wrong question
created: { by: human:benlinton, at: 2026-08-27T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
stage: draft
---

# Committed derived material has no tier

**`.luma/bundles/routing.toml` is generated from every adopted Bundle and is not
a Bundle.** It sits in `bundles/` because there is nowhere else. More derived
artifacts are coming — see `luma-foreman`'s
`apply-writes-an-entry-point-not-an-index` — and they have the same problem.

## The `cache/` deferral fired, and asked the wrong question

`DECISIONS.md` defers *a `cache/` tier for derived material* and sets a trigger:
**re-open when the first derived artifact exists.** It exists.

**But the deferred entry contradicts itself.** It argues for a tier *inside*
`.luma/` and justifies it with *"one `.gitignore` line instead of scattered
ignores"* — while the layout policy states **everything in `.luma/` is committed,
no exceptions**, and calls that the invariant that makes the directory
trustworthy. A never-committed tier under `.luma/` is not a hard call; it is a
category error, which is plausibly why it felt unfinished.

**So close it as wrongly framed rather than re-open it as asked.**

## Derived is provenance. Committed is storage. They are independent.

*Nothing generated is ever the source* applies to every derived artifact and says
nothing about where it lives. Whether to commit is a separate test:

> **Commit a derived artifact when something has to read it without the tool that
> made it. Otherwise it is a cache.**

Both halves are already argued in `DECISIONS.md`, about `routing.toml`: *"a
missing gate fails open"*, and *"a committed copy is what lets a bare clone with
no tooling reproduce the project, which is the property that separates adoption
from a package cache."*

| artifact | read by | |
| --- | --- | --- |
| `routing.toml` | the permission gate at runtime; a bare clone | **committed** |
| `CLAUDE.md`, `.claude/skills/` | a harness that is not the generating tool | **committed** |
| an entry point an adapter points at | a harness, transitively | **committed** |
| embeddings, digests, rolled-up search indexes | only the tool, rebuildable | **a cache, and never under `.luma/`** |

**A cache does not get a `.luma/` tier at all** — not because caches do not
matter, but because `.luma/` means committed. Machine-local material already
lives outside it, per *Per-user project settings live outside `.luma/`*.

## The adapter rule needs restating too

`DECISIONS.md` holds that *generated adapters — `.claude/`, `AGENTS.md`,
`CLAUDE.md` — stay outside `.luma/`*.

**Its premise is stale.** It reads *generated from `policy/`*, and a later
decision in the same file removed `policy/` entirely.

**And it fuses a platform fact with a principle.** *Live where their tool looks*
is not a choice anybody made — `CLAUDE.md` is where it is because Claude Code
looks there, and it could not be moved. *Nothing generated is ever the source* is
the durable half and concerns provenance, not location.

**The rule that survives both:** a generated artifact lives where its **reader**
looks. A harness's location if a harness reads it; `.luma/` if luma's own tooling
does. That explains `CLAUDE.md` and `routing.toml` at once, and it is what makes
a single harness-neutral artifact possible — several adapters pointing at one
file rather than a copy per harness.

## What this asks for

**A tier for committed, tool-owned, derived material**, or a decision that
`bundles/` is where it goes and `routing.toml` is not an anomaly. The tiers are
cut by lifecycle, and derived-from-what-is-in-force is arguably its own lifecycle:
it changes when its inputs change, never on its own, and losing it costs nothing
but a command.

**Whatever it is called, it inherits the properties that make `routing.toml`
safe** — the tool is the only writer, a hand edit is discarded on the next run,
and drift is detectable rather than prevented.

## Notes

**Naming is open and nothing is sacred yet.** `routing` is currently spoken for
by the permission gate, and `index.md` is reserved at a Bundle root rather than a
project root. The collision resolves in either direction: the gate's table holds
a permission list, which is the narrower job, and every copy of it lives in a
repository we own and is days old.

**Reported from `luma-foreman`, 2026-08-27.** Recorded in that project's backlog
first because there was nowhere else to put it. See
[[no-channel-for-what-a-project-learns]].
