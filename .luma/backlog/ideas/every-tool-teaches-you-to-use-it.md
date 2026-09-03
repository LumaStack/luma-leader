---
type: luma/idea
title: Every tool teaches you to use it as you go, like a game's first level
created: { by: human:benlinton, at: 2026-08-24T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: organization
stage: draft
---

# Every tool teaches you to use it as you go

## The idea, as raised

**Like video game design, every tool should teach you how to use it as you go
— the beginning level.** Which means:

- **Each tool decides whether it teaches per project or per user.** That choice
  belongs to the tool.
- **Progress is tracked outside the project.** A cache or somewhere in the
  user's local state — not the git repository.
- **What is tracked is which tutorials or tool-learning advancements they have
  completed.**
- **Each tool has a *getting started* or *start here* kind of thing**, and it
  teaches one step at a time.
- **At any point they can break off and go do stuff, then come back and
  continue** the getting-started workflow.

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it, so the raw version survives a reading that
may turn out to be wrong. Evaluation is welcome here — it just does not get to
edit what was raised.*

### Most of this already exists, unsequenced

**Three separable pieces are being asked for**, and only one of them is new.

| | | state |
| --- | --- | --- |
| **a curriculum per tool** | an ordered set of steps that constitutes the beginning level | **largely written.** `install-the-tools` then `adopt-knowledge` are literally the first two steps, already invocable by name |
| **an interleaving discipline** | teaching happens inside real work, resumable | a convention, costs nothing to state |
| **progress that survives the session** | what has been taught, remembered outside the repository | **nothing. This is the whole build** |

**So the idea is smaller than it sounds and its centre of gravity is the
record**, not the content. `lumastack/luma-catalog/luma-tools` is already a *start here* for the
toolchain; what it cannot do is know it is talking to somebody who read it last
week.

### The prior question: who is the student

**Almost every invocation here is agent-mediated**, which splits the job in two
and the split decides the design.

| | needs | re-taught |
| --- | --- | --- |
| **the human** | what the tool is for, what to reach for, in what order | **once.** Suppressing a completed step is correct |
| **the agent** | how to drive it, at the moment of driving | **every session.** It has no memory to mark |

**A completion record that suppresses teaching is a human affordance and would
be actively wrong pointed at an agent** — an agent arrives with a fresh context
every time, and the thing that serves it is the always-present index from
[[conditional-preload]], not a curriculum it has already finished.

**The test:** *if a step, once completed, never needs to be shown to anybody
again, the student is the human.* Most of the getting-started steps pass it.
Settle this before anything is built, because it decides whether the record is
consulted at all.

### `per project or per user` is a property of the lesson, not the tool

**Splitting it per tool forces the wrong answer on half of any real
curriculum.** `foreman` has both kinds of lesson at once:

- **how to install it and what it is for** — learned once, per person, and
  re-teaching it in the next repository is noise.
- **what this project has adopted and what that obliges** — **new in every
  repository**, and a per-user completion mark would silence it exactly where it
  is needed most.

**Mark the step with its scope and let a tool's curriculum be a mix.** It costs
one field, it is no harder to store, and it is the difference between a tutorial
that follows you and one that goes quiet the moment you change repositories.

### Where the record goes is already settled, with one gap

**`~/.config/luma/`, per `what-each-tool-does`.** Never `.luma/` — *committed
but personal* is the named failure there, and one person's tutorial progress in
a shared repository is precisely it.

**The gap: the idea says *per user* and that home is per machine.** One person
with two laptops is taught twice. **That is the same tension as
[[a-third-kind-of-consumer]]** and it should be settled the same cheap way here:
being re-offered a step you have done is a skipped prompt, self-correcting in
seconds, and not worth building a sync for. Say so rather than leaving it to be
rediscovered.

**One more, small and easy to get wrong:** every tool writing to one shared
machine-local home means one file per tool or one namespaced by tool. Removing a
tool must not corrupt another tool's memory of what it taught.

### The analogy earns two constraints that would otherwise be lost

**Taking *the beginning level* literally is what makes this different from a
README**, and two things follow that a tutorial system would not otherwise get
right.

**The first level is a real level.** In a well-made game it is the actual game
with the difficulty turned down — not a sandbox, and never thrown away. So the
teaching happens **against the user's own repository doing work they wanted
done**, never a scratch project. Anything that produces a disposable artifact
has become a demo.

**It is never a gate.** Progress state is a memory of what has been shown, not a
permission. **A tool that refuses to work until level one is finished is a tool
people route around**, and the routing-around is permanent while the tutorial is
not.

**And the third, which the estate has already reached from the other
direction:** a game teaches a mechanic immediately before it is needed.
[silent presence](../../../docs/silent-presence.md) argues that presence does
not produce compliance and that just-in-time beats up-front for attention
reasons as well as cost. **This is the same conclusion arriving as pedagogy.**
Two independent routes to it is worth noticing.

### The failure it has to answer for

**The stale completion.** The record says taught, the tool has since changed,
and the person now holds an out-of-date model that the tool will never correct
— because as far as it knows, that lesson is done. **It is worse than never
teaching**, since it is silent, and silence is the failure mode this estate
already keeps a name for.

**The fix is not free.** A completion has to name **which version of the step**
it completed, and a step that has materially changed re-opens. That is a
curriculum with versioned steps rather than a checkbox list, and it is the real
cost of the idea — a fifth thing to keep honest, alongside bundle versions.

### Whether each tool builds its own is the design question

**Four tools implementing the same loop is four places for it to drift.** The
alternative follows the estate's own grain: **the curriculum is content — an
ordered bundle, which is a thing that already exists — and one engine runs it
against one shared record.** A tool then ships lessons, not a tutorial system.

**`foreman` is the obvious host**, being the tool that already runs inside a
project and already projects knowledge at agents.

**Except for the bootstrap, which is not a detail.** The first step of the first
curriculum is *install the tools*, and it runs on a machine where no engine
exists yet. **Level one has to be teachable by the README.** Any design that
makes the tutorial a feature of the engine cannot teach anybody how to get the
engine.

### What evaluating this would have to settle

Who the student is, and whether the record is consulted for agents at all;
whether scope belongs to the step or the tool; whether the curriculum is content
or code, and which repository owns the runner; how a step declares that it has
changed enough to re-open; what the entry point is called; and what happens on
the second machine.

**On the name:** `getting started` and `start here` are both from the idea and
both fine. The estate's rule is to name for the verb, which favours something
like `teach` for the command and leaves the noun free. Worth one decision, not
an argument.

## Related

[[conditional-preload]] — the always-present index, which is what serves the
agent where a curriculum serves the human.
[[a-third-kind-of-consumer]] — per-person scope, and the machine-versus-person
tension this inherits.
[silent presence](../../../docs/silent-presence.md) — just-in-time over
up-front, reached from the attention side.
`lumastack/luma-catalog/luma-tools` — the existing *start here*, missing only its memory.
