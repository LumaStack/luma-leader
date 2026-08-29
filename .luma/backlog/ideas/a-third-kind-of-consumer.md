---
type: luma/idea
title: A third kind of consumer, for a bundle installed across everything you work on
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: next
scope: organization
lifecycle: draft
---

# A third kind of consumer

## The idea, as raised

**`consumers` on a bundle is `project` or `organization` today. It should also
cover the case where something is installed across everything you work on** —
global, personal, machine, whatever that turns out to be called.

---

## Commentary — `agent:claude-opus-5`, not part of the idea

### What forced it

`lumastack/luma-catalog/session-manager`. Its practice is identical in every repository, so
adopting it once per repository means a copy to upgrade in each — and the
alternative today is a hand-written `SKILL.md` in `~/.claude/skills/` that
nothing generates, nothing versions and nothing checks.

**No value of `consumers` describes what that is.** The bundle is not for a
project and not for an organization; it is for the person.

### The candidates

| | for | against |
| --- | --- | --- |
| **`operator`** | **the estate's own word, already** — 22 uses across `luma-foreman/docs`, the catalog and here, against `machine-local` 17, `personal` 16, `workstation` 3. A noun, matching `project` and `organization` | not what any harness calls it, so a newcomer has to learn it |
| **`user`** | what Claude Code calls it — *user instructions*, *user-level rules* — and VS Code and pip. Most guessable | **reads as the end user of the software the project builds.** On a field attached to a bundle that misreading is live, not theoretical |
| **`personal`** | plain English, needs no explanation, 16 uses already | an adjective where its siblings are nouns. `consumers: [project, personal]` does not parse as one list of like things |
| **`global`** | npm and Homebrew; the word most people reach for first | **git's `--global` means per-user and `--system` means per-machine** — the most familiar naming confusion in version control. It also conflates *who adopts* with *where it is stored*, which are the two things this field must keep apart |
| **`machine`** | concrete, and matches where the files actually land | **the wrong axis.** One person with two laptops has one selection, not two — and two people sharing a machine have two |

**What decides it: `consumers` answers *what kind of thing adopts this*.** A
project is a repository, an organization is a headquarters, and the third is **a
person**. So the value has to name a person, which rules `machine` out on
grounds that have nothing to do with taste, and weakens `global` for the same
reason.

**Between the remaining three, `operator` is the recommendation** — it is a
noun, it is unambiguous, and the estate has already chosen it four times over
without formalising it. `user` is the strongest challenger and loses only on the
misreading, which is worth weighing rather than dismissing: guessability beat
precision when `curator` was named.

### The prior question, which may make all of this unnecessary

**`consumers` declares who *may* adopt a bundle — a publisher's statement about
audience.** An operator-scoped bundle is not obviously that. Nothing about
`session-manager` forbids a project from adopting it; a team that commits
session notes should. What differs is **where one person's copy lives and what
projects it**, which is a decision the adopter makes, not the publisher.

**That is foreman configuration, not a bundle declaration.**

| | |
| --- | --- |
| **if that is right** | `consumers` stays at two values, the format is untouched, and `personal-skill-selection-not-committed` in `luma-foreman`'s backlog is the entire answer |
| **if it is wrong** | some bundle genuinely only makes sense operator-scoped and a publisher should be able to say so — then a third value is right |

**Settle that first.** The test: *name a bundle a project should be forbidden
from adopting.* If none exists, this is configuration wearing a declaration's
clothes.

### If it is built, it is cheap

`consumers` is defined in **SPEC §11.1** on the built-in `bundle` type, so a new
value is an LKF release — **add-only**, because §4 makes consumers tolerate
values they do not recognise. A bundle declaring `consumers: [operator]` is
simply not adopted by anything that has not learned the word.

It does not disturb §5.2, which singles out `consumers` as the field where
**absence says nothing** because both possible defaults would be wrong guesses.

### What it still would not settle

**Where an operator-scoped bundle is vendored.** Not `.luma/bundles/` — the
layout is emphatic that everything in `.luma/` is committed, and an operator's
selection must not be. The machine-local tier is `~/.config/luma/`, and how a
machine-local selection composes with a committed one at apply time is
open.

## Related

`personal-skill-selection-not-committed` in `luma-foreman` — the `apply`
half, and the reason this may need no format change.
`lumastack/luma-catalog/luma-config` — the two homes, and the test for which is which.
