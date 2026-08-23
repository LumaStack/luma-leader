---
type: luma/idea
title: Rename deliverable to issue or story
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle_status: draft
---

# Rename `deliverable` to `issue` or `story`

## The idea, as raised

**Rename `deliverable` to `issue` or `story` or something similar, because that
is really what they are.**

**Quite often an issue is broken down into a deliverable — but not always.**
Sometimes it tracks a bug or a problem, and breaking that thing down costs you
multiple stories (or deliverables).

*This may be partially recorded in `luma-backlog` already.*

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it. Evaluation is welcome here — it just does
not get to edit what was raised.*

### It is partially recorded, and the recorded rejection is the one this answers

`luma-backlog/docs/OPEN-QUESTIONS.md` §16 rejects both candidates in a line:

> **issue, ticket** — Both imply something is wrong. Too negative for work that
> is usually ordinary.

**The idea meets that head-on rather than going around it.** The rejection treats
the negative connotation as a flaw; the idea says the negative case is *real and
currently unrepresented* — a bug or a problem is a thing that lands on a backlog,
and the vocabulary has no word for it.

### There are two different claims here and they want different fixes

**A naming claim.** The unit is misnamed for what it usually holds. **Fixable by
renaming**, and §16 already grants half of it: relabeling is supported, and
*stories* is named explicitly as a display alternative. So if the complaint is
about what people read, it is already solved by configuration and the argument is
only about the canonical name.

**A structural claim, which is the stronger one.** *An issue is broken down into
a deliverable, and breaking one down costs several.* That describes **a unit
above the current one** — a problem that yields multiple deliverables. Renaming
does not create that level; it relabels the level below it and leaves the gap.

**§16 says the slot above is occupied:** *"an epic groups several of them and
belongs among the dimensions."* So the level exists as a **dimension** rather than
a record. Whether a dimension can carry a bug report — with its own body,
history, and outcomes — is the question the structural claim actually asks, and
it is not a naming question.

### What would settle it

**Which claim is being made.** If it is tone, relabeling already answers it and
the canonical name is a smaller argument than it looks. If it is that one problem
produces several deliverables and nothing holds the problem, **that is a modelling
gap and a rename would hide it behind a better word** — which is the worse
outcome of the two.

**Worth checking against real records first.** One deliverable exists today
(`deliverables/first-usable-build/`), and §16's own note says the type
*"describes one record… a description, not a prediction."* A second and third
record that are plainly bugs would settle this faster than any argument.
