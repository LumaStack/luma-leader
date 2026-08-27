---
type: luma/idea
title: Should preload become more sophisticated — a when, a hook, or an event
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: project
lifecycle_status: draft
---

# Should `preload` become more sophisticated?

## The idea, as raised

**Should bundle `preload` get more sophisticated?** Today it is one
unconditional value. Three shapes it could take instead:

- **A `when` condition.** `preload: { when: "working with CSS", ... }`
- **A label or tag other bundles can use to trigger preload routines.**
  `preload: { hook: "css", ... }` — how another bundle preloads the css
  routine or hook.
- **Hooks on events.** `preload: { on: "bundle_load", ... }`,
  `preload: { on: "bundle_error", ... }`

*"These are ideas, maybe bad maybe great."*

**And the constraint behind them.** Loading everything would be fine if context
were unlimited. It is not — so there have to be strategies that **guarantee
things are loaded when they should be, and leave them on standby when they are
not relevant whatsoever.**

---

## Commentary — `agent:claude-opus-5`, not part of the idea

*Below the idea and separate from it, so the raw version survives a reading that
may turn out to be wrong. Evaluation is welcome here — it just does not get to
edit what was raised.*

**One ambiguity worth resolving before evaluating.** *Bundle preload* could mean
either the per-Document `preload` (§5.2) or the per-Bundle `preload_default`
that `DECISIONS.md` describes and nothing has built. The three shapes read as
though they apply to Documents, but the sentence says bundle. They may want
different answers, since the adopter wins outright on the bundle-level one.

**The three are not equally cheap, and the ordering surprised me.**

**`hook:` may partly exist already as `tags`.** A Document tagged `css` and a
consumer that knows it is doing CSS work gets most of this with no format change
— what is missing is a *convention* that a consumer may use tags to decide what
to pull in. **But the idea asks for more than tagging**: *other bundles* trigger
the routine, which is cross-bundle coupling, and bundles deliberately do not
depend on one another. A shared label namespace is the decoupled version of
that, and whether it is enough is the open question.

**`on:` is where §5.2 already objects**, in words that read like they anticipated
it: *"the loading model is a property of the consumer, and a field carried by
every Document that uses it has to outlive whatever the current one turns out to
be."* `on: bundle_load` looks like a longer spelling of today's `preload`.
`on: bundle_error` is the genuinely new one — surface troubleshooting exactly
when something breaks — and it costs assuming consumers have a lifecycle, have
errors, and agree on their names.

**`when:` has the most content and the hardest problem.** Something has to decide
whether prose matches: tags a project declares, an explicit mode, or a model
judging free text. The last fails quietly. It also has to survive work changing
shape mid-session, and nothing watches for that.

**The hazard all three share.** A Document that should have loaded and did not is
indistinguishable from one that was never going to. Whatever ships has to make
non-activation **reportable**, and an ambiguous condition should fail *toward*
loading — so the unclear case costs context rather than compliance.

**A cheaper thing to try first.** The expensive part of a Document is its
content, not that it exists. An always-present index — a line per Document
naming what it is and when it matters — means a consumer can never conclude
*nothing applies* out of ignorance, and pulls the text when the work matches.
Thirty Documents is thirty lines. Same shape as `find-decision` in the
`lumastack/luma-catalog/decision-records` bundle, where the procedure is `optional` and the
three-line trigger lives in the document that is `mandatory`. **If that works,
`preload` never has to learn what a condition is.**

**What evaluating this would have to settle.** Whether the format describes
conditions at all or leaves them to consumers; what vocabulary is closed enough
that two consumers agree; how a Document declares a condition without asserting
a runtime; whether conditional and unconditional `preload` coexist without the
unconditional one becoming the exception; and who re-evaluates when the work
changes shape.

## Notes

**This would travel upstream as a change to the knowledge format** (§5.2), not be
decided here. It is filed in this repository because the thinking belongs beside
[[a-mandate-is-a-record-not-a-value]], which reaches the same problem from the
obligation side and lands on the same index pattern. **If either is built, the
other should reuse its answer rather than inventing a second one.**
