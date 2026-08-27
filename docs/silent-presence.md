---
type: document
title: Silent presence
description: The failure where knowledge loaded correctly and was not applied — why no loading discipline fixes it, the enforcement ladder it implies, and how a snapshot taken at the moment of failure tells four causes apart.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-23T00:00:00Z }
---

# Silent presence

**Draft. Nothing here is settled.** Split out of
[adoption-use-cases.md](adoption-use-cases.md), which names four ways knowledge
fails to reach an agent and scores the adoption designs against them. Three of
those four are answered by getting loading right. **This is the one that is not**,
and it grew large enough to stop being a section.

Seventh companion to [bundle-dependencies.md](bundle-dependencies.md),
[bundle-versioning.md](bundle-versioning.md),
[shared-types.md](shared-types.md), [curator.md](curator.md),
[catalog-namespaces.md](catalog-namespaces.md) and the use cases. Kept out of
[DECISIONS.md](DECISIONS.md) on purpose.

## The failure

**A document loaded and was not applied when it mattered.** Not missing, not
dropped, not contradicted — present, and inert.

It is named to pair with **silent absence**, the failure where a document that
should have loaded did not, because **the two are indistinguishable from
outside.** One never loaded, one loaded and did nothing, and the output is
identical: an action taken without the rule that governed it. The causes are
opposite and so are the fixes, which is exactly why one name for both would be
useless.

## Why no loading discipline fixes it

**A document can be in the window and still not be used.** Attention is finite
and position matters; something read at the start of a session is not reliably
applied sixty turns later, and a rule that never becomes salient at the moment
it applies may as well not have been there.

**It is the worst of the four to detect, and the reason is uncomfortable.** The
other three leave evidence: a checksum, a missing file, a budget, an `apply`
that never ran. This one leaves none, because **every mechanical check passes.**
The bundle is adopted, the copy is clean, the adapters are current, the document
is present, and the rule was ignored anyway. `inspect` reports green and is
right to.

**It puts a ceiling on `preload: mandatory`, and on the obligation ladder above
it.** Both assume that presence produces compliance. Presence is what they can
buy; compliance is what they are for. If that implication is weak, then marking
something mandatory is a weaker act than it reads as — which is the same doubt
`preload-levels-collapse-into-emphasis` reaches from the delivery side.

**It argues *for* the index pattern rather than against it**, which is worth
noticing because the index was chosen for budget reasons. If presence does not
guarantee attention, then a short index re-encountered at the moment of need can
outperform a large blob loaded once at the start. **Just-in-time beats up-front
for attention reasons as well as for cost** — the same decision, now supported
twice.

## The enforcement ladder this implies

**For a rule that genuinely must not be missed, presence in context is the
weakest available enforcement.** Ranked, weakest first:

| | |
| --- | --- |
| **prose loaded at session start** | present, and hoping |
| **prose re-stated at the point of use** | the skill adapter naming the standing context its workflow assumes |
| **a mechanical check** | `inspect` finding the violation after the fact |
| **a hook that blocks the action** | the rule executes rather than being read |

**Two of these already exist and one was built by accident.** The adapters
`apply` writes re-state each bundle's standing context inside the skill that
fires — done for thin-adapter reasons, and it happens to be the second rung.
`agent-permissions` is the fourth rung outright: it does not put a rule in
context, it runs at the tool call.

**The useful move is to notice which rung a rule deserves.** Most knowledge
belongs on the first two and is fine there. Anything whose violation is
expensive and irreversible — a published credential, a force push, a deleted
record — belongs on the fourth, and putting it in a policy document is a
decision to accept silent presence as a risk.

## It is last to fix, and the record-keeping is what makes that survivable

**This is the hardest of the four and will be the last one tackled.** There is no
mechanism to build. The other three each have a thing you could go and write;
this one is a property of how attention works, and the honest response is not a
feature.

**But it is observable in aggregate, and that is enough.** Per event it is
undetectable — nothing distinguishes *ignored the rule* from *the rule did not
apply here*. Across two hundred commits it becomes a **rate**: how often did work
land that contradicts a policy that was in force at the time. A rate is a
measurement, and a measurement is all that is needed to act.

**The apparatus for that already exists, as a side effect of decisions being
archived rather than deleted.** Reconstructing *what was in force on a given
date* requires knowing when each rule started and stopped applying — which is
exactly what `decided` and `archived` carry, and both were added for other
reasons. Records are append-only, archived decisions keep their reasoning, and
everything is committed. **Most systems cannot measure this because they do not
retain what the rules used to be.**

**And the measurement answers the question the ladder leaves open.** *Which rung
does this rule deserve* is not a declaration an author makes and not a judgement
somebody argues; it is **evidence**. A policy violated repeatedly while sitting
in context has demonstrated that context is the wrong rung for it, and that is a
finding rather than an opinion.

So the plan for silent presence is not a mechanism. It is: keep the records,
watch the rate, and move individual rules up the ladder as they earn it.

## The rate is a curve, not a number

**A violation rate means nothing without saying how deep into a session it was
measured.** Content loaded a moment ago is applied more reliably than content
sitting in the middle of a very large window, and a rule that holds firmly at
turn five may be effectively gone by turn eighty. *"This policy is violated 8% of
the time"* is not a finding; *"it holds until roughly turn N and then does not"*
is.

**At least four variables move it**, and none is under a bundle author's control:

| | |
| --- | --- |
| **depth** | how many turns since the rule was last in front of the model |
| **position** | where it sits in the window — the middle is the weak place |
| **window size** | a larger window is not a longer memory; it is more middle |
| **model** | each has its own profile, and it changes with every release |

**Everything here is tracked per model, and that is not a refinement.** A blended
figure across models is not a weaker measurement, it is a **misleading** one:
without partitioning, **a model change is indistinguishable from a policy
change**. Compliance shifts after an upgrade, somebody attributes it to the
rewording that shipped the same week, and the conclusion is confidently wrong.
That is the same attribution failure everything else in this document is trying
to avoid, arriving through the measurement rather than through the mechanism.

So the model is not one variable among the four. **It is the partition key**, and
a number recorded without it is not worth recording.

**The useful unit is something like a half-life** — the depth at which a rule
stops being reliably applied. That is a property of a rule *and* a model
together, which has two consequences worth stating plainly.

**It makes the enforcement ladder depth-dependent rather than only
importance-dependent.** *Which rung does this rule deserve* gains a second
question: **how deep into a session does it still have to hold?** A rule that
only matters in the first few exchanges is safe as prose. One that must hold at
turn two hundred is not, however unimportant it looks.

**And it argues for re-statement over louder up-front loading, for the third
time.** If the failure is decay, then loading harder at the start does not
address it — being re-encountered near the point of use does. The index pattern
wins again, now for an attention reason rather than a budget one: an index that
is re-read is refreshed, and a document loaded once at turn one is not.

## The measurement problem, which is worse than the measurement

**Git history records the action and not the context that produced it.** A
commit does not say what turn it was made at, how much was loaded, which model
was running, or where in the window the rule sat. **The rate is recoverable from
the records that already exist; the curve is not.**

That has an unwelcome implication: **if the curve is ever wanted, something has
to start writing it down now.** Depth, model, and what was loaded are cheap to
record at the time and impossible to reconstruct later. `session_note` already
carries `pinned` — the state a note assumed — which is the nearest existing
thing and was built for a different purpose.

**And the confounds are severe.** Work done deep in a session is not the same
work as work done at the start: it is more tangled, more likely to be the hard
part, and long sessions are selected for difficult problems. A naive comparison
would attribute to attention what belongs to task difficulty.

**The curve is also perishable.** Measured against one model, it does not
transfer to the next, and models change faster than a corpus of evidence
accumulates. Chasing a precise decay constant means re-running the measurement
forever.

## So do the cheap version

**Two buckets per model, not a curve. Early and late.** Does compliance with a
given rule differ between the first part of a session and the rest, holding the
model fixed? That is robust to most of the confounds, needs one recorded number
rather than a model of attention, and it is enough to decide a rung — which is
the only decision the measurement feeds.

**Per model even here**, because the partition is what makes the comparison mean
anything, and because a rule that needed a hook on one model may not on the
next. That is the good case and it is invisible without the key.

The curve is a research project. The bucket is a column — and the model is the
column next to it.

## Snapshot on failure, not telemetry always

**This resolves the tension the previous section left open.** Recording depth and
model continuously is ambient surveillance of every session, in a system where
everything is committed. **Recording them at the moment a failure is detected is
an incident record** — which is a discipline the estate already has, and the
shape everything else here uses: write when something happens, never sample.

Failures are rare, so the cost is small, and what is captured is proportionate to
what went wrong rather than to how long somebody worked.

**Most of the snapshot does not have to be captured, because it is derivable.**
Foreman wrote the adapters, so *what else was loaded* is reconstructible from
the repository at that commit — the same trick `adopted.toml` already uses.
**What actually has to be recorded is the volatile part**: the model and its
version, how deep the session was, what the agent did, what rule it contradicted,
and who noticed. Everything else is a commit away.

## Four hypotheses, and they are told apart by different fields

| | what it means | what would show it |
| --- | --- | --- |
| **misauthored** | **the rule never did what it intended in the first place** | it reads as background, buries the instruction, or its `description` does not match when it applies |
| **interference** | something else in context competed or contradicted | a *specific* other document present across incidents |
| **staleness** | the loaded copy was old, or the rule had been superseded | the loaded version against the current one; `decided` and `archived` dates |
| **decay** | it was simply too deep | high depth, and nothing else unusual |

**Misauthored is the null hypothesis and should be checked first**, because it is
by far the cheapest to fix and the most likely to be true early on. A rule that
was ambiguous, or filed as rationale when it needed to be an instruction, was
never going to be followed at any depth — and diagnosing that as an attention
problem means building measurement to discover a wording defect.

**One snapshot distinguishes nothing.** Every hypothesis is consistent with any
single incident. Decay only appears as a correlation with depth, interference as
a correlation with one other document being present, staleness as version skew.
**The value is entirely in accumulating them**, which means the first several
teach nothing and that is not a failure of the method.

## The one place a violation is already caught, and discarded

**`agent-permissions` denies at the moment of action, and keeps nothing.** The
gate emits a decision to the harness and writes no record. Every denial is an
instance of *the rule was in force and the agent attempted it anyway* — detected
at the only moment when the full session state still exists — and all of it is
thrown away.

That makes it the natural first snapshot site: **the detection already works and
only the recording is missing.** It is also the narrowest possible starting
point, covering the rules that reached the top rung rather than all knowledge,
which is the right place for a mechanism nobody has tried yet.

**The other three detection points are all worse, in the same direction.** A
mechanical check finds a violation after the fact with the session gone. A human
noticing is the most reliable trigger and the least systematic. A retrospective
audit over history has no session state at all — it can produce the rate, never
the diagnosis.


## Open

- **What actually computes the violation rate.** The substrate is there —
  append-only records, archived decisions with dates, everything committed — and
  nothing reads it that way. It is a retrospective audit rather than a check,
  which makes it `audit-records` territory rather than `inspect`.
- **Where a failure snapshot lives.** It is a record of something that happened,
  so `.luma/records/` by the tier definition — but it is written by a hook on a
  workstation, may contain a command somebody typed, and is the first thing in
  that tier not authored deliberately by a person.
- **Whether a denial is a failure at all.** The gate denying something is the
  system working. It is evidence of *attempted* violation rather than actual, and
  whether that predicts real silent presence is an assumption, not a finding.
- **Who decides a snapshot was misauthored rather than decayed.** The
  classification is a judgement, it is the whole value of the exercise, and
  nothing says whether it belongs to the rule's author or its adopter.
- **Whether an enforcement rung should also be declarable.** Measured is better
  than declared and slower to arrive. A bundle stating that a policy warrants a
  hook would give a new adopter the benefit before they have accumulated any
  history of their own — at the cost of every author having to answer it.
- **Whether any of this is worth building yet.** Nothing here has been tried, the
  failure it addresses has never been measured, and the honest position is that
  it is last for a reason. Recorded now because the cheap parts — recording the
  model, keeping what the gate already knows — are unrecoverable afterwards.
