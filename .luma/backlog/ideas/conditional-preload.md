---
type: idea
title: Should preload become conditional — a when, an event, or a label
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: project
lifecycle_status: draft
---

# Should `preload` become conditional?

**Draft — raised, not evaluated.** Today `preload` is one unconditional value:
how strongly a Document should be loaded before work begins, full stop. Several
shapes would make it situational, and they are not equally promising. Recorded
together because they share one hazard.

```yaml
preload: { when: "working with CSS" }     # a condition
preload: { on: "bundle_load", ... }       # an event
preload: { on: "bundle_error", ... }
preload: { hook: "css", ... }             # a label something else triggers on
```

**This is a proposal against the knowledge format**, so it would travel upstream
as a change to LKF §5.2 rather than being decided here. It sits here because the
thinking belongs with [[a-mandate-is-a-record-not-a-value]], which reaches the
same problem from the obligation side.

## The three shapes, worst first

### `on:` — an event. The format already argues against this

§5.2 says it outright, and it is worth reading before evaluating anything else:

> *Ahead of the work* is deliberately not pinned to a moment. In current
> practice it means what a consumer loads at the start of a session, and that is
> the clearest way to explain it — but **the loading model is a property of the
> consumer**, and a field carried by every Document that uses it has to outlive
> whatever the current one turns out to be.

**`on: bundle_load` is what `preload` already means**, spelled longer.

**`on: bundle_error` is genuinely new and genuinely wanted** — surface the
troubleshooting Document exactly when something breaks, rather than carrying it
in every session against the chance. The cost is that it assumes consumers have
a lifecycle, have errors, and agree on what to call them. **That is encoding a
runtime in a knowledge format**, and the field would outlive the runtime it
described.

### `hook:` — a label. It may already exist and be spelled `tags`

`preload: { hook: "css" }` says *this is relevant to CSS work*. So does
`tags: [css]`, which is already a core field with a deliberately loose,
organization-defined vocabulary.

**What is missing is not a field but a convention** — that a consumer MAY use
tags to decide what to pull in, and that doing so is the consumer's loading
policy rather than a claim the Document is making about itself.

**That needs no format change at all**, which makes it by far the cheapest of
the three and the one to try first. If tags turn out to be too loose in
practice, that is evidence for a dedicated field rather than a reason to skip
straight to one.

### `when:` — a condition. The one with real content and the hardest problem

**`when` is prose, and something has to decide whether it matches.** The options
run from tags a project declares, through an explicit mode somebody switches
into, to a model judging free text.

**The last fails quietly**, which is the worst available outcome: a Document
that should have applied and silently did not, with nothing reporting the
non-match.

It also has to survive work changing shape mid-session — somebody starts on CSS
and ends up in an incident — so a condition evaluated once at the beginning is
wrong by the middle, and nothing today notices that the work moved.

## The hazard all three share

**A Document that should have loaded and did not is indistinguishable from one
that was never going to.** Whatever ships has to make non-activation
**reportable**: *these were held back, and here is the condition that did not
match.* Without that, a mis-scoped condition is invisible forever, because the
only evidence is the absence of something nobody knew to expect.

**And it should fail toward loading.** Where a condition is ambiguous, load —
so the unclear case costs context rather than compliance. That matches how the
rest of the estate resolves: a check that cannot be performed is a failure
rather than a pass.

## What conditional loading is usually reaching for, and a cheaper way to get it

**The expensive part of a Document is its content, not the fact that it
exists.** An always-present index — one line per Document naming what it is and
when it matters — means a consumer can never conclude *nothing applies* out of
ignorance, and can pull the full text the moment the work turns out to match.

Thirty Documents is thirty lines. **That is a Bundle-level convention rather
than a per-Document field, and it does not require the format to know what a
condition is.** It is the same shape as `find-decision` in the
`luma/decision-records` bundle, where the search procedure is `optional` and the
three-line trigger saying it exists lives in the document that is `mandatory`.

**Try that before changing `preload`**, because if it works the field never has
to learn about conditions.

## What evaluating this would have to settle

- Whether the format describes conditions at all, or leaves them entirely to
  consumers — and if it leaves them, whether two consumers will agree enough for
  a Bundle to be portable
- What vocabulary is closed enough to be checkable, given that free text is the
  version that fails silently
- How a Document declares a condition **without asserting a runtime**, which is
  the objection §5.2 raises against events
- Whether conditional and unconditional `preload` can coexist without the
  unconditional one quietly becoming the exception nobody uses
- Who re-evaluates a condition when the work changes shape, since nothing
  watches for that today

## Notes

**Related, and reached from the other direction:**
[[a-mandate-is-a-record-not-a-value]] asks the same question about obligations —
*do I need the CSS mandates loaded when I am working on an incident?* — and lands
on the same index pattern. If either is built, the other should reuse its answer
rather than inventing a second one.
