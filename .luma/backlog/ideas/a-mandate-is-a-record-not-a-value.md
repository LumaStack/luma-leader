---
type: idea
title: A mandate is a record, not a value — who mandated it, since when, and when it applies
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: project
lifecycle_status: draft
---

# A mandate is a record, not a value

**Draft — not settled, not reviewed.** Recorded so the options survive, and
because one deferred alternative in `DECISIONS.md` may have had its re-open
condition met.

**The problem underneath is a context budget.** Loading every mandate would be
right if context were free; it is not, so mandates need to be **guaranteed loaded
when they apply and on standby when they do not** — and the hard part is that the
second one can fail silently.

Today `obligation: mandatory` is a **value**. The proposal is that it becomes a
**list of records**, each saying who mandated it, from when, and under what
circumstances it applies.

```yaml
requires:
  - bundle: acme/css-conventions
    mandate:
      - { by: team:legal,      since: 2026-03-01 }
      - { by: team:leadership, since: 2026-06-14, when: "doing taxes" }
```

Everything below falls out of that one shift, which is why it is one idea rather
than three — though it splits cleanly if it ever needs to.

## The four parts, and they are not equally new

**1. Situational loading — the strongest part, and genuinely new.** *Do I need
the CSS mandates loaded when I am working on an incident?* Probably not. Nothing
today can express *mandatory, but only when the work is X*, and the absence
pushes every mandate toward always-on, which is how a session's context fills
with things nobody is using.

**2. Provenance — who mandated it.** Partly new. See the section below: the
mechanism for stacking already exists, and provenance is what makes it safe to
*un*-stack.

**3. Stacking, so dropping one mandate does not drop the rest.** **Already
solved, and worth knowing before building anything.** *Obligations resolve
most-restrictive-wins* already allows a bundle to appear in `requires:` as many
times as it likes, with the strongest winning. Legal and leadership each get an
entry; legal removes theirs; leadership's still stands. The safety property is
already there.

**What is missing is the ability to tell the entries apart.** Two identical
`obligation: mandatory` lines are indistinguishable, so *remove legal's mandate*
is not an operation anybody can perform correctly. **Provenance is not a new
mechanism — it is the label that makes the existing one operable.** That is a
much smaller and much better-founded proposal than *let mandates stack*.

**4. A mandate nothing can override.** The most contentious, and it contradicts
something settled — see below.

## Where this lands against what is already decided

**It sits across two different questions that `DECISIONS.md` deliberately keeps
apart**, and the idea as stated moves between them:

| | the question | where it is declared |
| --- | --- | --- |
| **obligation** | must a project *adopt* this bundle? | the publishing catalog's `catalog.md`, in `requires:` |
| **preload / preload_default** | should it be *loaded* ahead of work? | the bundle author, and the adopter, respectively |

*"Mandating a bundle load is complicated"* is the **loading** question. The
`mandate: { by: legal }` sketch is shaped like the **adoption** one. Both are
worth having and they are not the same field — an organization can perfectly well
mandate that a bundle is adopted while leaving when it loads to the project.

**The situational part probably belongs to loading, and the provenance part to
adoption.** If that holds, this is two proposals wearing one syntax, and the
first thing to settle is which half each piece is for.

### A deferred alternative may have had its trigger met

*Bundles have two axes: reach and obligation* deferred an `authority` field with
this condition:

> *Re-open if a bundle can ever be mandated by a party other than the catalog
> that published the requirement.*

**Whether that has fired depends on a modelling choice nobody has made yet.** If
legal and compliance express themselves *through* the organization's catalog,
the catalog is still the only publisher and the trigger has not fired — `by:` is
then an annotation on an entry rather than a second mandating party. If they are
modelled as independent parties who mandate directly, it has fired outright.

**That question is the whole idea, compressed.** It is worth answering first,
because it decides whether this is a field or an architecture.

The same decision also says: **"Before adding a field, check whether the fact is
already implied by where the thing lives."** Provenance survives that test where
three earlier proposals did not — an organization's catalog is one location and
legal, leadership and compliance are three parties inside it, so location cannot
carry the distinction.

### The non-overridable mandate contradicts a settled position

*Preload is declared by whoever holds the knowledge* settles that for
**bundle-level** loading, **the adopter always wins** and the author only
suggests. Its reasoning is that whether a bundle is relevant to a session is a
fact about the adopter's work, which the author cannot know.

**A load mandate nothing can override reverses that**, so it is a supersession
rather than an addition, and it needs the argument for why an outside party
knows better than the project what the project's session needs.

**There may be one.** That decision's reasoning is about *bundle authors*, who
are publishing to strangers. A compliance function inside the same organization
is not a stranger and may legitimately know something the project does not. If
that distinction holds it is the seam to argue along — **the author cannot
override the adopter; the organization can** — and the settled decision simply
never contemplated a third party.

## The real constraint: a guarantee and a budget cannot both be absolute

**Loading every mandate would be right if context were free. It is not**, so the
job is a pair of properties that pull against each other:

- **Guaranteed loaded when they should be** — no mandate ever silently fails to
  apply
- **On standby when they are not relevant at all** — costing nothing

**Something has to give, and naming what is the first design decision.** With
finite context and unbounded mandates there are only three places to spend the
loss:

| | cost |
| --- | --- |
| **cap the mandates** | a catalog that exceeds the budget fails at publish. Loud, early, and it makes the organization ration deliberately |
| **degrade the content** | summaries load, full text on demand. Cheap, and a summary of a mandate is a mandate somebody can misread |
| **degrade the guarantee** | conditional loading — this idea. **Cheapest and the only one that can fail silently** |

**This idea takes the third, so everything below is about making that failure
loud.** A mandate that should have applied and quietly did not is worse than
always-on loading, because always-on at least fails by running out of room, which
somebody notices.

### The pattern that resolves most of it: load the index, never the content

**The thing that must never be missing is the knowledge that a mandate exists —
not the mandate.** Those have wildly different costs. A line per mandate, always
loaded, naming it, who mandated it, and its `when`:

```
acme/css-conventions   team:legal, team:leadership   when: frontend work
acme/incident-comms    team:compliance               when: incident response
acme/tax-handling      team:legal                    when: doing taxes
```

Thirty mandates is thirty lines. **An agent holding that can never conclude *no
mandate applies* out of ignorance** — it can see exactly what it is not loading
and why, and pull any of them the moment the work turns out to match.

**This is the same shape as `find-decision` in `luma-catalog`'s
`decision-records`**, arrived at independently and worth stealing deliberately:
the search procedure is `optional`, and the three-line trigger that says it
exists lives in the document that is `mandatory`. *The pointer is load-bearing in
a way the procedure is not.* A mandate index is that pattern at organization
scale.

### Three rules that make the degradation loud

**Fail toward loading.** Where it is unclear whether `when` matches, **load it**.
Only a confident non-match stays on standby. This is the same posture as the rest
of the estate — a check that cannot be performed is a failure rather than a pass
— and it means the ambiguous case costs context rather than compliance.

**Report every standby.** A session that held six mandates on standby says so,
and says which condition did not match. **A silent non-match is indistinguishable
from a bug**, and it is the only evidence that would ever show a `when` is drawn
wrong. Nobody will find a mis-scoped mandate by reading the catalog.

**Re-evaluate at boundaries, not once at the start.** Work changes shape mid-
session — somebody starts on CSS and ends up in an incident — and a `when`
evaluated only at the beginning is wrong by the middle. **This is the expensive
one**, because it means something has to notice the work changed, and nothing
does today.

### Authors do the compression, not the runtime

**A mandate that is too big to always load can often be split rather than
gated.** A small always-on core — the rule itself, in a few lines — and a large
on-demand body with the reasoning, examples and edge cases. The organization
mandates the core unconditionally and the body loads on match.

**That converts a guarantee problem into an authoring problem**, which is the
better place for it: whoever wrote the mandate knows which sentence is the one
nobody may miss, and a runtime deciding that by budget never will.

It also gives the budget something to bite on. LKF already says a bundle's total
mandatory weight *"is better surfaced where the Bundle is published than
discovered when something fails"* — **so mandates should declare their cost**,
and an organization whose unconditional core exceeds the budget finds out at
publish rather than in somebody's session.

## Problems worth solving before this is buildable

**`when:` has no vocabulary, and that is most of the work.** *"Doing taxes"* is
prose. Something has to decide whether the current work matches it, and the
options run from tags the project declares, through an explicit mode somebody
switches into, to a model judging free text. **The last one fails quietly** — a
mandate that silently does not apply is worse than one that always does, because
nothing reports the non-match.

**`since:` versus `by:`.** The existing schema already uses `by:` for a
*deadline* (`by: 2026-10-01`), while this sketch uses `by:` for the *actor*.
Direct collision, and LKF's own `actor_event` uses `by` for the actor — so the
catalog schema is arguably the one that is out of step. Worth settling
deliberately rather than in whichever gets written first.

**What does a stack of three mandates actually change?** *How heavy it is* is
real intuition, and today it changes nothing mechanical: three mandatory entries
resolve to the same `mandatory` as one. Either the count feeds something —
reporting, exemption difficulty, ordering — or it is documentation, which is
still worth having but should be admitted rather than implied.

**Who may remove an entry?** The stacking argument only pays off if legal can
withdraw legal's mandate and nobody else's. That is an access-control question
in a plain committed file, and the honest answer today is *nothing enforces it;
review does.*

## Notes

**Captured 2026-08-22 from a conversation about `preload` in the
`decision-records` bundle**, where only the bundle's entry point is `mandatory`
and everything else is `recommended` or loaded on demand. The question *should
this be mandatory for everyone, always?* is what produced it.

**The `archived_reason` open question in `luma-catalog`'s `decision` type is the
same shape** — *archived on a directive from the architecture board* is an
authority, not a reason, and it was recorded there rather than solved for the
same reason it is unsettled here. **If authority gets modelled once, it should be
modelled for both.**
