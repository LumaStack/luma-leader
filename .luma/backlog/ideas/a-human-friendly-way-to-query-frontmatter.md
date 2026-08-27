---
type: luma/idea
title: A human-friendly way to query frontmatter, so grep is not the interface
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: organization
lifecycle_status: draft
---

# A human-friendly way to query frontmatter

## The idea, as raised

**We need a CLI way to run grep that is human friendly.**

It came out of arguing about whether `created: { by, at }` should have been
`created_by:` and `created_at:`. The nested form won on three grounds, and the
one real cost was that **`grep created_by` works and `grep -A1 created:`
does not.** That cost is answered by tooling rather than by flattening the
shape.

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it. Evaluation is welcome here — it just does
not get to edit what was raised.*

### The reframe is the useful part

The grep objection had been treated as **a reason to change the format**. Treated
as a **missing tool**, it stops being an argument about field shape at all — and
it generalises past `actor_event` immediately. Every nested or list-valued field
has the same problem: `sources`, `verified`, `requires`, `starters`,
`vendored_from`. **Nothing in the estate can answer *which documents were
verified by a human* without a person writing an ad-hoc pipeline.**

### What it would have to do to be worth existing

**Beat `grep` on the cases `grep` is bad at, and not replace it elsewhere.**
Plain text search over bodies is already solved and should be left alone.

The queries that are currently painful are all structural:

- *every document where `created.by` is an agent* — nested field, prefix match
- *every idea with `horizon: next` and no `contributors` containing a human* —
  two fields, one a list of actors
- *every document whose `lifecycle_status` is unset* — **absence**, which grep
  cannot express at all
- *every bundle whose `matches: always` documents exceed some size* — a
  computed property

**Absence is the one that decides it.** `grep -L` finds files lacking a string,
which is not the same as a field being unset, and it gets `lifecycle_status:` in
a code fence wrong. Now that `unknown` is the default, *what has nobody
declared* is a question people will actually ask.

### It probably already half-exists

`luma-clarify` centralises all YAML through one round-trip module and has the
strictest parser in the estate. Whatever this becomes should read documents the
way that module does rather than growing a fourth partial parser — see
[[three-frontmatter-parsers]], which is the same problem viewed from the other
side.

### Where it might live

**Not the knowledge format** — `must_not_own: tooling that reads or writes the
format`.

**Probably not `foreman`**, whose job is acting on a project rather than
answering questions about documents, though the boundary is arguable and
`inspect` already reads frontmatter.

**Possibly its own thing**, in which case the naming rule applies: name it for
the verb. It queries.

**Or possibly nothing** — `yq` exists, and the honest question is whether this is
a tool or a documented `yq` recipe plus a few aliases. **Worth trying the recipe
before writing the tool**, because a documented incantation that works is cheaper
than a binary nobody has released.

### What would settle it

Whether the painful queries are frequent enough to justify anything. **Count them
first** — the estate is a few hundred documents, and if the answer is *three
people asked once*, the recipe wins.
