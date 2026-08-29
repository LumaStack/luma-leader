---
type: luma/idea
title: A mandate is a record, not a value — who mandated it, since when, and when it applies
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: project
lifecycle: draft
---

# A mandate is a record, not a value

## The idea, as raised

**Mandating a bundle load is complicated, and sometimes the context of the
situation will matter.**

- **Sometimes a thing should be mandated outright** — it gets loaded, and there
  is **nothing the user or the agent can do to override it**.
- **Other times it is mandated only in a certain mode or situation.** *Do I need
  the CSS mandates loaded when I am working on an incident? Probably not.*
- **We will want to know who mandated it.** Did this come from leadership, from
  the project, from somewhere else?
- **Mandates should probably stack.** If leadership and legal and compliance all
  mandate something, that tells us how heavy it is — and it means that **if legal
  drops their mandate, we do not accidentally forget about leadership and
  compliance by design.**

```yaml
mandate:
  { by: "legal",      since: DATE }
  { by: "leadership", since: DATE, when: "doing taxes" }
```

*"I don't know, this is an idea. Maybe it's bad."*

**And the constraint behind it.** Loading all mandates would be nice if context
were unlimited. It is not — so there have to be strategies that **guarantee they
are loaded when they should be, and leave them on standby when they are not
relevant whatsoever.**

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it, so the raw version survives a reading that
may turn out to be wrong. Evaluation is welcome here — it just does not get to
edit what was raised.*

### It sits across two questions the estate keeps apart

| | the question | where it is declared |
| --- | --- | --- |
| **obligation** | must a project *adopt* this bundle? | the publishing catalog's `catalog.md`, in `requires:` |
| **delivery** | should it be *loaded* ahead of work? | the bundle author, and the adopter |

*"Mandating a bundle load is complicated"* is the **loading** question. The
`mandate: { by: legal }` sketch is shaped like the **adoption** one. Both are
worth having and they are not the same field — an organization can mandate that
a bundle is adopted while leaving when it loads to the project. **The situational
half probably belongs to loading and the provenance half to adoption**, and if
that holds this is two proposals wearing one syntax.

### The stacking mechanism already exists; the labels do not

*Obligations resolve most-restrictive-wins* already lets a bundle appear in
`requires:` as many times as it likes, with the strongest winning. Legal and
leadership each get an entry, legal removes theirs, leadership's still stands —
**the safety property is already there.**

**What is missing is telling the entries apart.** Two identical
`obligation: mandatory` lines are indistinguishable, so *remove legal's mandate*
is not an operation anybody can perform correctly. **Provenance is not a new
mechanism — it is the label that makes the existing one operable**, which is a
smaller and better-founded proposal than *let mandates stack*.

### A deferred alternative may have had its trigger met

*Bundles have two axes: reach and obligation* deferred an `authority` field with
this condition:

> *Re-open if a bundle can ever be mandated by a party other than the catalog
> that published the requirement.*

**Whether that has fired depends on a modelling choice nobody has made.** If
legal and compliance express themselves *through* the organization's catalog,
the catalog is still the only publisher and `by:` is an annotation. If they are
independent parties who mandate directly, it has fired outright. **That question
is the whole idea, compressed**, and it decides whether this is a field or an
architecture.

The same decision also says *"before adding a field, check whether the fact is
already implied by where the thing lives."* Provenance survives that test where
three earlier proposals did not: one catalog is one location, and legal,
leadership and compliance are three parties inside it.

### The non-overridable mandate contradicts something settled

*Preload is declared by whoever holds the knowledge* — a decision whose title
keeps a field the format has since released; read it as being about delivery —
settles that for **bundle-level** loading **the adopter always wins** — because whether a bundle
is relevant to a session is a fact about the adopter's work, which the author
cannot know.

**A load mandate nothing can override reverses that**, so it is a supersession
rather than an addition. **There may be a good argument**: that decision reasons
about *bundle authors* publishing to strangers, and a compliance function inside
the same organization is not a stranger. If that distinction holds, the seam is
*the author cannot override the adopter; the organization can* — and the settled
decision simply never contemplated a third party.

### Problems worth solving before it is buildable

**`when:` has no vocabulary, and that is most of the work.** *"Doing taxes"* is
prose. The options run from tags a project declares, through an explicit mode, to
a model judging free text — and **the last fails quietly**, which is worse than
always-on because nothing reports the non-match.

**`since:` collides with `by:`.** The existing schema uses `by:` for a *deadline*
(`by: 2026-10-01`) while this sketch uses it for the *actor* — and LKF's
`actor_event` uses `by` for the actor, so the catalog schema is arguably the one
out of step. Worth settling deliberately rather than in whichever gets written
first.

**What does a stack of three actually change?** *How heavy it is* is real
intuition, and today three mandatory entries resolve to the same `mandatory` as
one. Either the count feeds something — reporting, exemption difficulty,
ordering — or it is documentation, which is still worth having but should be
admitted.

**Who may remove an entry?** The stacking payoff needs legal to withdraw legal's
and nobody else's. That is access control in a plain committed file, and the
honest answer today is *nothing enforces it; review does.*

### On the budget constraint

**A guarantee and a budget cannot both be absolute.** With finite context and
unbounded mandates the loss lands in one of three places: cap the mandates (fails
loudly at publish), degrade the content (summaries, which can be misread), or
degrade the guarantee (conditional loading — **the only one that fails
silently**).

If it is the third, the work is making that failure loud: **load the index, never
the content** — a line per mandate naming it, who mandated it, and its `when`, so
an agent can never conclude *no mandate applies* out of ignorance. **Fail toward
loading** where a condition is unclear. **Report every standby**, since a silent
non-match is the only evidence a `when` is drawn wrong. And **re-evaluate at
boundaries**, because work started on CSS can end in an incident.

**Authors can also do the compression**: a small always-on core stating the rule,
and a large on-demand body with the reasoning. That turns a guarantee problem
into an authoring problem, which is the better place for it.

## Notes

**Captured 2026-08-22** from a conversation about `preload` — the field the
format has since released — in the
`decision-records` bundle, where only the entry point is `mandatory`.

**Related, reached from the other direction:** [[conditional-preload]] asks the
same question about the loading field rather than the obligation, and lands on
the same index pattern. If either is built, the other should reuse its answer.

**The `archived_reason` open question in the `decision` type is the same shape**
— *archived on a directive from the architecture board* is an authority, not a
reason. **If authority gets modelled once, it should be modelled for both.**
