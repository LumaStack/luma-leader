---
type: luma/idea
title: Next-steps documents — where they live, how they load, and how they stay honest
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle_status: draft
---

# Next-steps documents

## The idea, as raised

**Next-steps documents should be a real thing with rules**, not a file somebody
invents each time. Points, as raised:

- **Not in `docs/`.** They should live in `.luma/backlog` or `.luma/cache` or
  something like that.
- **Project next-steps and session next-steps are different things.** *These are
  the next steps for the project* versus *these are next steps for this
  session / agent / person* — they should go in different places, different
  files, or both.
- **A way for next steps to be loaded up easily.**
- **A workflow for making sure they aren't stale when loading them up.**
- **Some guardrails around them.**
- **A disclaimer at the top**, verbatim:

  > **This file is consumable.** It says what to do next, it is worked through,
  > and then it is deleted. Anything in it that outlives the work belongs
  > somewhere durable *before* it is deleted — a decision, an idea, a bundle —
  > not here.

- **A `next_steps` type in `luma-types`** is worth considering — *a possibility,
  not a recommendation.*

### And the deep end

**Should the knowledge format have a way to mark files as ephemeral** —
scratch, tmp, and the like — **even though they should still be captured in git
for history's sake?**

**The philosophy is capture everything**, so we can go back in time to learn
about how to improve our process or where things went in the wrong direction —
and so we can **rewind and start over from a safe point, as an always-available
nuclear option.**

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it. Evaluation is welcome here — it just does
not get to edit what was raised.*

### Half of this already exists, and it is the session half

The `luma/session-manager` bundle has a **`session_note` type** (`kind:
checkpoint | handoff | close`, plus a `pinned` record of the branch and commit
the note assumed), a template, and four workflows — `session-checkpoint`,
`session-handoff`, `session-close`, and **`session-resume`**, which is *the way
to load them up easily* for that half.

**So the split lands cleanly on what is there:** session next-steps are a
`session_note` and are solved. **Project next-steps have no type, no home, no
loader and no staleness check** — that is the actual gap, and it is smaller than
the idea looks.

`pinned` is also the staleness mechanism already invented once: a note records
the state it assumed, so a resumer can tell whether the world moved. **A project
next-steps document wants the same field for the same reason.**

### `.luma/backlog/` fits the existing tier definition exactly

`luma-layout` defines the tiers by lifecycle, and the backlog tier is *"what we
intend. **Churns — items are created and destroyed**."* A consumable document
that is worked through and deleted is precisely that.

**`.luma/cache/` would conflict with a settled rule.** The layout policy sends
cache to `~/.cache/luma/` and tests it with *"if deleting it loses a decision
somebody made, it is not cache."* A next-steps document is meant to be
deletable — but only *after* its durable content has been extracted, and the
philosophy above says keep it in git regardless. Cache is not committed, so that
is the wrong tier.

### The tension the philosophy creates, which is worth naming

**`.luma/` is committed. No exceptions** — the layout policy is explicit, because
uncommitted files there mean two agents reading different rules. And
machine-local state goes to `~/.config/luma/`, outside git.

**A personal session note is exactly the thing that falls between them.** Commit
it and everyone sees your scratch; keep it machine-local and *capture everything
so we can rewind* loses it. The philosophy and the layout rule disagree here, and
nothing has had to resolve it yet because nobody has written one.

### On marking files ephemeral in the format

**Nothing existing covers it.** `lifecycle_status` is maturity —
`archived` means *was in force, now retired*, not *never meant to last*.
`stale_after` says recheck, which implies the content might still be right.
`preload` is loading. **An ephemeral document is a durability claim, and the
format has no vocabulary for durability.**

**The strongest argument for it is that it makes a guardrail mechanical.** The
disclaimer above asks a human to check that nothing durable is left homeless.
Marked ephemeral, that becomes checkable: **a durable document linking to an
ephemeral one is a defect**, and a tool can say so. Guardrails that depend on
somebody remembering are the ones that fail.

**It may be a family rather than a single field.** The strict-mode idea in the
format's roadmap sketches `mutability: append_only | frozen | open` for the same
reason — a lifecycle claim the format cannot currently make. Durability and
mutability are adjacent, and two one-off fields added six months apart will
overlap awkwardly. **Worth designing together or not at all.**

**And it fails the built-in bar as a type, but not as a field.** A scratch note
can be any type — a `session_note`, a `next_steps`, a plain document — so
ephemerality is a property of the instance, not a kind of thing. That points at a
core field, which is a much bigger ask than a shared type and needs the *would a
consumer ignoring it be broken* test run properly.

### On a `next_steps` type

**The bar is: name the consumer, and name what it does differently.** A loader
that finds next-steps documents and a staleness check that reads `pinned` are
both real consumers doing real dispatch, so it would clear the naming bar.

**Whether it belongs in `luma-types` is the separate question**, and today the
answer is probably no — one tool would read it. `session_note` lives in the
bundle that gives it meaning, which is the same shape, and *shared types travel
inside bundles* says to leave it there until a second consumer exists.

**The likelier shape is that this belongs in `session-manager` beside
`session_note`** — same lifecycle, same loader, same staleness problem, and the
bundle already owns the vocabulary. Splitting project-scope from session-scope
across two bundles would separate two things that are read together.

## Notes

Raised while rewriting `luma-foreman/docs/next-steps.md`, which had gone stale
and needed emptying before deletion — the immediate evidence that these need
rules rather than good intentions.
