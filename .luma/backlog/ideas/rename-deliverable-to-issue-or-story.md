---
type: luma/idea
title: deliverable is probably wrong — reopen the naming
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle_status: draft
---

# `deliverable` is probably wrong — reopen the naming

## The idea, as raised

**Rename `deliverable` to `issue` or `story` or something similar, because that
is really what they are.**

**Quite often an issue is broken down into a deliverable — but not always.**
Sometimes it tracks a bug or a problem, and breaking that thing down costs you
multiple stories (or deliverables).

*This may be partially recorded in `luma-backlog` already.*

**`deliverable` was a good try and it is probably not going to work — so back to
the drawing board, probably.**

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it. Evaluation is welcome here — it just does
not get to edit what was raised.*

### Why it broke, precisely

**The property that won `deliverable` is the property that fails.**
`luma-backlog` §16 chose it over `project` partly because *"it teaches
nothing"* was `project`'s flaw, and `deliverable` **"obliges every backlog entry
to answer *what gets handed over*."** That obligation is the whole case for the
word.

**A bug has nothing handed over.** *The retry queue drops messages under load* is
a backlog item, and the thing that happens to it is that something stops being
wrong — no artefact is produced, nothing is delivered. Forced to answer *what
gets handed over*, it either invents a fake artefact or does not get filed, which
is the exact failure §16 used to rule out `feature`.

### The test set is what was incomplete, not the reasoning

Every candidate was checked against four cases:

> *ship payments v2* · *lower resting heart rate to 55* · *establish a daily
> writing habit* · *publish the Q3 strategy document*

**All four are things you produce.** Not one is something that is wrong. That is
why `issue` and `ticket` read as *too negative for work that is usually ordinary*
— against that set they are, and against a fifth case they are not.

**So a fresh pass needs a fifth case**, and it should be a plain defect:

> *the retry queue drops messages under load*

Any candidate that cannot hold all five is out, and `deliverable` is now the one
that cannot.

### What a fresh pass inherits

- **The four cases stay** — the unit is not necessarily software, and health,
  habits and documents were load-bearing in the original pass.
- **Several rejections stay valid on their own terms.** `feature` fails the
  non-software cases; `epic` names the level above and is assigned to dimensions;
  `project` collides with the repository and with external trackers.
- **Relabeling already exists.** §16 supports displaying the unit as anything a
  team prefers, with the canonical name on disk. That solves presentation and not
  this — the argument is about the canonical name.
- **`deliverable`'s accepted costs are now worth revisiting too** — eleven
  characters, no usable short form (*del* reads as delete), and consultant tone.
  Each was accepted as a price for the *what gets handed over* obligation. If
  that obligation is the defect, its price stops being worth paying.

### The structural question is separate and should not be smuggled in

*An issue is broken down into a deliverable, and breaking one down costs
several* describes **a unit above the current one**, which §16 says is an epic
and assigns to dimensions. **Renaming does not create that level.** Whether a
dimension can carry a bug report — with a body, history and outcomes of its own —
is a modelling question, and answering it with a nicer word would hide it.

**Settle the naming and the level separately**, in that order, since the naming
is answerable now and the level needs real records to argue from.
