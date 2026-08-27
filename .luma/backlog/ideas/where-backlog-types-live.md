---
type: luma/idea
title: A luma/backlog bundle, and whether its types belong there or in luma-types
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle_status: draft
---

# Where the backlog types live

## The idea, as raised

**At some point we should create a `luma-catalog/backlog` bundle, and decide
whether the backlog types belong in that bundle or in the `luma-types` bundle.**

**The point of the bundle is so agents know how to use the tool, and how to use
the types.**

**And agents should reach for the backlog tool and use it, as soon as it is far
enough along.**

---

## Commentary — `agent:claude-opus-5`, not part of the idea

### The `luma-types` half is already answered by a rule we wrote

`shared-types.md` sets the bar: **three consumers, or a deliberate decision to
share** — and states outright that *"importance is not the bar. A type serving
one tool belongs to that tool, however load-bearing it is there."*

`luma/backlog/deliverable`, `wave`, `outcome`, `task`, `decision` and
`exploration` serve exactly one tool. **They do not qualify**, and the answer
does not depend on anything unresolved.

**What would change it** is a second real reader — `inspect` reporting backlog
health, or something aggregating work across projects. Plausible, not actual.

### The bundle half has a stronger argument than "somewhere to put the types"

**Today `luma-backlog init` writes `_types/` from types compiled into the
binary.** That is precisely the placement `shared-types.md` rejects first:
*"not the applier — welds two version numbers that must move independently."*

A tool shipping its own type definitions means the type version and the tool
version cannot move apart. Fixing a field description requires a tool release;
upgrading the tool silently changes the contract of records already on disk.
**A catalog bundle is what unwelds them**, which is a better reason for the
bundle to exist than tidiness.

### The bundle's real job is to make an agent reach for the tool

**Not "here is how it works" but "use this instead of whatever you were about to
invent."** An agent that does not know the backlog exists will build its own
tracking, and the estate has just watched that happen.

**The evidence is one session, 2026-08-23.** Fifteen items were tracked in an
ephemeral session-scoped list. Four of them stayed marked pending through
completion and four merged pull requests, because nothing made updating them a
step — and the stale list was only caught by a person reading it. **A working
`luma-backlog` binary with eight commands sat in a sibling directory, unused and
unmentioned, for the whole session.**

That is the gap in one paragraph: a real tracker existed, was never reached for,
and an ephemeral one silently rotted in its place. It is also **silent
presence** wearing work clothes — the tool was present in the estate and inert.

**So the bundle needs a `matches: always` policy**, not just workflows. An
agent has to know the backlog exists *before* it starts a piece of work, because
by the time it is tracking something it has already chosen how.

### It also has to say how to use the types, not only the tool

**A record gets written by hand as often as through the CLI** — an agent editing
an outcome, a person fixing a field. That reader needs the contract, and a
contract compiled into a binary is one they cannot read.

**Types and practice want the same bundle**, so this is one bundle rather than a
types bundle plus a practice bundle.

### What "far enough along" has to mean

The tool works — `init`, `new`, `list`, `show`, `set`, `close`, `journal`,
`verify` all run. So the bar is not *does it function*. Three things stand
between it and being the default an agent reaches for:

- **Where it writes** is unresolved, and a tool that might move its own
  directory is not one to adopt across the estate yet.
- **The unit names are unresolved** — `deliverable` is recorded as probably
  wrong, and names published in a catalog bundle are expensive to change.
- **Nothing projects it.** Until the bundle exists and `apply` writes it, an
  agent still has to be told.

**None of those is a code change**, which is worth noticing: the tool is ahead
of the decisions around it.

### The conflict underneath, which is not obvious

**`luma-backlog`'s open questions call `.backlog/` "itself a Bundle."** The
directory *is* moving under `.luma/` — settled 2026-08-23, *a luma tool writes
into `.luma/`, and anywhere else is opted into* — so there is now **a Bundle
living in the `backlog/` tier rather than in `bundles/`**, and the layout says
bundles live in `bundles/`.

Three ways out, and they are genuinely different:

| | |
| --- | --- |
| **the backlog is not a Bundle** | it merely has a `_types/` directory, and the claim was loose. Cheapest, and probably true |
| **the tier hosts one** | `backlog/` may contain a Bundle because its types are its own. A carve-out, and carve-outs accumulate |
| **the types move out** | `.luma/bundles/luma/backlog/` holds the contracts; `.luma/backlog/` holds only records. Cleanest split, and it means a project must adopt a bundle before it can keep a backlog |

**The third is the one that follows from everything else** — contracts are in
force, records are what happened or what we intend, and the tiers are cut by
lifecycle. It also has the highest cost: `luma-backlog init` would either adopt
a bundle or refuse, which makes a standalone tool depend on a catalog.

### What settles it, in order

1. ~~`.backlog/` versus `.luma/backlog/`~~ — **settled 2026-08-23.**
   `.luma/backlog/` always; `.backlog/` only when committed config asks. See
   `DECISIONS.md`.
2. **Whether the backlog is a Bundle** — one sentence in `luma-backlog`'s open
   questions carries a lot of weight and may not have meant to. **Now the
   blocking question**, because the location is fixed and the conflict above is
   live rather than hypothetical.
3. **Then the bundle**, which is mechanical once that is answered.

### Worth knowing before opening `luma-backlog`

It is pinned at `lkf_version: 0.0.2` — **nine format versions behind**, from
before `concept` was dropped, before namespaced shared types, before
`lifecycle_status: unknown`. Its README also says *"no implementation yet"*
while a working binary sits in the repository with eight commands. Anything
touching that repository meets both first.

## Related

`shared-types.md` — the three-consumers bar this is measured against.
`luma-layout` — the tier definitions the third conflict runs into.
[[rename-deliverable-to-issue-or-story]] — the unit names, unresolved, and
cheaper to change before a catalog bundle publishes them.
