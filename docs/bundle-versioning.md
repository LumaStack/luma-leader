---
type: document
title: Bundle versioning
description: What major, minor and patch mean for a bundle whose content is prose — the test for each tier, the one part that can be checked mechanically, and why patch is the dangerous one here.
lifecycle: draft
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Bundle versioning

**Draft. Nothing here is settled.** Companion to
[bundle-dependencies.md](bundle-dependencies.md), which consumes versions;
this one defines them.

**The ordinary ladder, with the ordinary names.** `major.minor.patch`, meaning
roughly what they mean everywhere else. Nothing is gained by inventing a private
scheme, and a reader arriving from any other ecosystem should not have to learn
one.

What differs is the **material**. Semantic versioning was designed for code,
where a version is a claim about an interface: if the signature held, the caller
still works. **A bundle's content is prose, and prose has no interface — its
content is its effect.** So each tier needs a test written in terms of outcomes
rather than signatures.

## Size carries no signal, in either direction

Delete one word.

If the word is *the*, nothing changed. If the word is *not*, everything did.

**That is the whole difficulty in one example**, and it rules out every cheap
proxy: lines changed, characters moved, files touched, whether the diff added or
removed. A two-character deletion can invert a rule and a two-page rewrite can
leave every claim standing.

**So the unit that matters is the normative claim, not the file, the paragraph,
the line or the word.** The question at every tier is the same: *what did this
document require, permit or forbid before, and what does it require, permit or
forbid now?* Everything below is a way of asking that.

This is also why the one mechanical check that works is about **references**. A
reference is structural and countable — a link either resolves or it does not, a
field either exists or it does not. A claim is neither, and no diff tool will ever
find one.

## Major — the expected outcome changes

**An adopter would have to do something on their side to keep getting the result
they had.** That is the test. Not *did a lot change*, but *does someone have to
adjust*.

For prose that means: somebody following the old version would now get a
materially different outcome, and getting the old outcome back requires action —
changing their configuration, rewriting their own content, or deciding not to
upgrade.

**This is genuinely hard to judge, and pretending otherwise helps nobody.** The
author is predicting how a change lands for people whose situations they cannot
see. The prediction will sometimes be wrong, which is why
[bundle-dependencies.md](bundle-dependencies.md) does not let compatibility rest on
it — a recorded *verified against* is the mechanism that catches a misjudged bump.

### Removal is a strong flag, and only sometimes a verdict

**Subtraction is a useful signal that something is major, and it is not a rule.**
Removing things is also how prose gets stronger — dropping a carve-out, cutting a
permissive option, deleting advice that turned out to be wrong. All of those make
a policy firmer rather than broken, and firmer is the definition of *minor* above.

And subtraction is not even reliably *large*: deleting the single word *not*
subtracts almost nothing and reverses everything. **Removal tells you where to
look, never what you found.**

So the tempting rule — *a release that subtracts cannot be minor* — is right often
enough to be worth acting on and wrong often enough that acting on it must not mean
refusing.

**One kind of removal is objective, and can be rejected outright:**

- a document that other documents link to
- a field from a type definition, when records exist written against it
- a heading or workflow step that other content references

These break a **reference**. They are yes-or-no, diffable between two versions,
and need nobody's opinion — the content is provably broken rather than merely
suspicious. **Reject these at publication when the release does not call itself
major.** Removing a type field is where the damage is worst: a record written
against a field that no longer exists is silently wrong, and no care afterwards
recovers it.

**Every other removal is a flag rather than a verdict.** Deleting prose that
nothing points at can land on any tier, and the major test is what decides:
**would an adopter have to do something to keep getting the result they had?**

- removing an exception somebody was relying on — **major**, however much it
  strengthens the policy, because those adopters now fail
- removing a permissive option nobody was required to use — **minor**, if the
  people using it are choosing rather than depending
- removing a paragraph nobody ever acted on — **patch**

**So publication should surface removals rather than refuse them.** *"This release
removes X — is it still minor?"* is the useful behaviour: it makes the author look
at the one signal most likely to mean they got the tier wrong, without deciding on
their behalf in the cases where subtracting was the improvement.

## Minor — more, or sharper, but not surprising

**Additive in nature, and expected to change behaviour — just not startlingly.**
A reader's expectations still hold; there is simply more of the thing, or the
thing is better aimed.

What belongs here:

- **perfecting behaviour that is already there** — the rule existed, now it is
  right
- **tuning and improving** — same intent, better executed
- **strengthening a policy, workflow or piece of knowledge** — firmer, clearer,
  more complete

**Minor changes behaviour, and that is not a contradiction.** For prose it is the
normal case: adding a paragraph to a policy is meant to change what an agent
does, or there was no point adding it. The line is not *does behaviour change*
but *would somebody be surprised*. An adopter reading the diff should recognise it
as the same thing, done better.

## Patch — it cannot change what anyone does

**The test:** would a reader who correctly understood the old text behave any
differently under the new one? **If yes, it is not a patch.**

That covers what patch has always covered — typos, formatting, a dead link, a
broken example, restating an ambiguous sentence *to the meaning a careful reader
already took from it*. What it excludes is the thing that matters.

### Patch is the dangerous tier here, and it inverts the code intuition

In code, patch is the safe one: bugfixes, nothing structural, review it lightly.
In prose:

> `must not` → `must`

Two characters. The diff of a typo. A complete reversal of a rule, and by the test
above it is a **major** change, because everyone following the old text now has to
do the opposite.

Nothing about the shape of that change announces itself. A reviewer scanning
*"patch: fixed wording"* approves it in seconds.

**So patch deserves a rule rather than a promise: a patch does not touch a
normative sentence.** If the edit lands on a *must*, *never*, *always*, a
threshold, or a condition, it is not a patch however few characters moved. *"I only
fixed the wording"* is a claim worth a second reader.

## Two edges worth deciding

**A change to the bundle's own dependency constraints is a version change like any
other.** Loosening one is additive — adopters gain flexibility, so minor.
**Tightening one can break an adopter outright**, because their existing
combination may no longer resolve, which makes it major by the test above even
though no prose changed.

**Before `1.0`, the ladder means less and should be said to mean less.** The
knowledge format already declares its `0.0.z` tier unstable. A bundle in `0.x` is
announcing that its shape is still moving, and adopters should read the changelog
rather than trust the tier. **The ladder starts being a promise at `1.0`**, and
publishing `1.0` is the moment an author claims the shape has settled.

## What this does not solve

**The tiers are the author's judgment, and judgment fails.** Nothing here makes a
misjudged minor into a correct one, and only the subtraction check is enforceable.

That is a known limit rather than a gap to fill, because
[bundle-dependencies.md](bundle-dependencies.md) does not rely on the tiers being
right. Resolution keys on the major line, compatibility rests on a recorded
verification rather than on a number, and cross-bundle links are checked
independently. **The version ladder is how a change is communicated, not how
compatibility is guaranteed** — and it is worth keeping those two jobs apart,
because conflating them is what makes people distrust version numbers everywhere
else.
