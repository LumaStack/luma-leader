---
type: luma/idea
title: Work modes — named postures a session holds, and something that holds it there
created: { by: human:benlinton, at: 2026-08-31T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: organization
stage: draft
---

# Work modes

**A harness ships two or three working modes and you cannot add a fourth.**
Plan, edit-allowed, auto — one vendor's division of how work is done, offered as
the whole vocabulary. There is no way to say *this session is diagnosis*, *this
session is design*, or *this session is brainstorming*, and those are not
variations on planning.

**Nothing here is settled, and the mechanism least of all.** The options below
are listed as options, they are deliberately incomplete, and none of them is a
recommendation.

## The failure

**A mode stated in prose does not survive the session that states it.** The
instruction is given once, in the first message, and by the fourth or fifth turn
the agent is doing the thing it was told not to do — writing the file, settling
the question, reaching for code to demonstrate a design.

**This repository already has it written down as a fact about how its maintainer
works.** [[design-first-working-mode]] records that a session may be explicitly
scoped to design-only and that *the scope is expected to hold for the whole
session, not just the first reply* — which is a mode, described as a preference,
because there was nothing to describe it as.

**The existing modes are also the wrong axis.** Plan / edit / auto divides
sessions by *how much the agent may write*. That is one dimension of several,
and it collapses diagnosis, design and brainstorming into a single bucket —
"not writing" — when what separates them is what the agent is supposed to be
producing and how it is supposed to talk.

## What a mode is

A mode is a named posture, and a posture answers more than one question at once.

| | what the mode fixes |
| --- | --- |
| **Reach** | what may be read, written, run — and what is off limits |
| **Output** | what this session is supposed to produce, and where it lands |
| **Manner** | how the agent talks — proposes, asks, asserts, enumerates |
| **Stance toward what exists** | is the current codebase the ground, or is it noise |

**The last row is the one no harness has.** Reach is a permission setting and
every harness has it. *Ignore what is already built* is not a permission, and it
is the entire content of brainstorm mode.

## The three that prompted this

**Diagnostic mode — find out, write nothing that matters.** Read the code, run
what is safe to run, look things up. **Not planning** — a plan is a commitment
to a shape and diagnosis has not earned one yet. Scratch work goes to a
temporary directory, and there is somewhere — open where — that analysis and
established facts get captured, so a session's findings are not lost when it
ends. **Nothing lands in the project itself.**

**Design mode — options, discussion, no unilateral conclusions.** Present
alternatives rather than a chosen answer. Do not write a decision, and do not
treat a decision as settled, without confirmation. **Do not assume what the user
wants.** And a required shape for every proposal:

> **The best way** — what you would do with a free hand, abandoning what exists
> where it deserves to be abandoned.
>
> **The pragmatic way** — what you would do given the codebase as it is.

**Presenting both, every time, is the mode.** It stops the agent from silently
choosing which of the two it is answering — which it does now, invisibly, and
usually in favour of whatever is already there.

**Brainstorm mode — the current implementation is not evidence.** Do not anchor
on what exists; treat it as one option among many and not the privileged one.
Generate quantity, difference and range. **Convergence is a different mode**, and
mixing them is how brainstorming turns into a defence of the status quo three
ideas in.

## Common modes, and your own

**Two halves, and the second one is the point.**

**Ship the set most people would want** — the three above, plus whatever survives
a look at what people actually announce at the start of a session: review,
implement, refactor, debug, document, teach.

**And make adding your own a first-class act** rather than editing a vendor's
list. A mode that is specific to one person, one project, or one organization
has to be as easy to define as the shipped ones, or the shipped ones become the
ceiling. **Where a custom mode lives is open** — project, organization, or
published for anyone to adopt — and the answer is probably all three.

## How a mode gets entered — options, incomplete

| | what it costs |
| --- | --- |
| **A skill invoked by name** | Portable, discoverable, already how everything else here arrives. But entering a mode is a *session* act, and a skill is a *turn* act |
| **A terminal command** | Fits muscle memory, can write state somewhere durable that outlives a turn. Needs something installed |
| **Riding the harness's own mode switch** | Free where it exists, absent everywhere else, and the vocabulary is fixed |
| **Some combination** | Likely, and the reason the mechanism should not be picked yet |

**Both, probably.** The user's framing was *skills or terminal commands or both*,
and there is no obvious reason a mode could not be entered either way if the mode
itself is defined in one place.

## How a mode gets held — options, incomplete

**This is the harder half.** Entering a mode is easy; still being in it twenty
turns later is the problem the idea exists for.

- **Prose alone.** Free, portable, and demonstrably insufficient — it is what
  [[design-first-working-mode]] already relies on.
- **Hooks that block.** A hook firing before a tool call can refuse the write
  and say why. This is the only option that *executes* rather than asks.
  [[loading-mechanisms]] already works through what hooks cost — code on
  somebody's machine, per-harness, needing an install path — and none of that
  gets cheaper here.
- **Re-injection.** Something that puts the mode back in front of the agent
  every turn, or on the turns that matter. Cheap, harness-dependent, and it
  reminds rather than prevents.
- **Harness permission modes.** Read-only enforcement already exists in places
  and is worth borrowing rather than rebuilding — but it only covers reach, and
  reach is one row of four.
- **Nothing.** Trust the mode as a declared intention. Worth stating as an
  option because a mode that is merely *named and visible* may already be most
  of the value.

**The enforceable part and the unenforceable part do not overlap much.** A hook
can stop a write. Nothing outside the model can make it present two options
instead of one, or stop it anchoring on the existing codebase.

## What is open

- **Does a mode persist across a session boundary, or a compaction?** If it does
  not, it is a strong first message and nothing more.
- **One mode at a time, or composition?** Diagnostic-and-brainstorm is
  coherent; diagnostic-and-implement is not.
- **What ends a mode.** Explicit exit, task completion, or a timer.
- **Does a mode change what knowledge loads?** Brainstorm mode arguably wants
  *less* project context in front of it, which inverts everything
  [[how-knowledge-arrives]] is built to do.
- **Where diagnostic mode's findings land.** A temporary directory, a notebook,
  or a real record kind that outlives the session.
- **What this is.** A bundle, a set of skills, a tool, or a type plus a bundle
  that reads it. Named last on purpose.
- **Whether it belongs in the catalog.** Work modes are not specific to this
  estate and have no obvious home project, which is an argument for publishing
  rather than an accident of where the idea was written.

## Notes

**Raised by the maintainer on 2026-08-31**, from wanting the harness's own mode
switch to be extensible: the shipped modes are useful, the set is closed, and the
modes actually wanted — diagnostic, design, brainstorm — are not variations on
the ones offered.

**The mechanism was left open at the maintainer's explicit instruction.** The
options above are recorded as options and are known to be incomplete; picking one
is the work, not the preamble to it.
