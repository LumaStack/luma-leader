---
type: document
title: Retiring a concept
description: How a retired idea comes back — not from the files that still name it, but from the model about to name it again — and the framework that has to defend both directions at once.
lifecycle: draft
created: { by: human:benlinton, at: 2026-08-27T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-27T00:00:00Z }
---

# Retiring a concept

**Draft. Nothing here is settled.** It proposes a type, a published record, and
changes to two engines, none of which exist. Companion to
[silent-presence.md](silent-presence.md), which is the same shape of failure one
layer up: knowledge that is present and not acted on, versus knowledge that is
absent and confidently reinvented.

## The failure

**One repository moves a foundational idea, and the others go on teaching the old
one as current.** Not as history — as instruction. A reader arriving cold cannot
tell the difference, because a document that has simply not been updated reads
exactly like a document that means what it says.

**That is the observable half. The dangerous half has no file in it at all.**

> *A retired word comes back by being reinvented, not by being remembered. The
> words worth retiring are usually the natural English for what they described,
> so absence from a repository is a weak defence — an author reaches for one
> again and it reads as a fresh choice rather than a revival. **That happened
> here within minutes of a sweep that removed one.***
>
> — `luma-foreman`, `inspect/rules/vocabulary.py`

**Within minutes.** The sweep was clean, the word was gone from every file, and
it came back anyway — because the thing that produced it was never the files. It
was a model reaching for the obvious word for the job, which is exactly the word
that was retired for being obvious.

**This is an AI problem before it is a documentation problem**, and the two halves
need different defences:

| | the old idea comes from | what stops it |
| --- | --- | --- |
| **leak forward** | files that still assert it | a check, after the fact |
| **leak backward** | the model's own priors | delivery, before the fact |

**A check cannot touch the second one.** There is no file to grep before an agent
writes a line. By the time the retired word exists on disk, the reinvention has
already happened, and every subsequent reader of that line is being taught it.

## Why what exists does not close it

**`luma-foreman inspect --rule vocabulary` is the right idea and the wrong
scope.** It reads `[[retired]]` from one project's own config, which fails four
ways:

- **Hand-transcribed.** This repository's list was copied out of `SPEC.md`
  section 13 by hand on 2026-08-27. It goes stale the next time the format
  releases a name, silently, which is the same failure one level up.
- **Per-project.** A retirement decided in `luma-foreman` binds `luma-foreman`.
  Nothing carried it to the five repositories that describe `luma-foreman`.
- **Flat.** Every hit is a notice. A dead field named in a `policy` — which
  binds — is graded the same as one in a draft that is arguing about it.
- **Never delivered.** The config is read by `inspect` and shown to nobody. **The
  agent authoring the document has not seen it and cannot have seen it**, which
  is precisely the case that matters.

**And it is blind where it is most needed.** `luma-catalog` has no such config at
all, so the check is skipped there — and the catalog is what every project copies
from. F-004 of the 2026-08-26 audit was exactly this: a published policy teaching
`preload`, two releases after the format removed it, reaching every adopter of
`bundle-manager`.

## What a framework has to do

**Retirement becomes a published record rather than a project's private config.**
That is the whole move; everything below follows from it.

### A retirement is a record, not a setting

**It has the shape of a decision, because it is one.** What was retired, what
replaced it, where that was decided, and what it binds.

```yaml
type: retirement
term: preload
replaced_by: matches            # or nothing, when the referent is gone
retired_in: v0.0.12
decided_by: SPEC.md#13
scope: format                   # format | catalog | project
except:
  - .luma/records/              # append-only; they say what was true when filed
```

**`scope` is what per-project config could never express.** A format retirement
binds everybody who reads the format. A catalog retirement binds everybody who
adopts from it. A project retirement binds one repository. Today all three are
written in the same file and none of them travels.

**`except` stays**, because the exemptions are real and were earned the hard way —
records are append-only, `## Version` histories say what was true when written,
and a document arguing about which word should replace another *cannot be
restated in the word it settled on*.

### It travels by adoption, because that already works

**Published as a bundle in `luma-catalog`, adopted like anything else.** No new
distribution channel, no new sync, no second thing to remember to run. A
repository that adopts the estate's retirements gets them at a version, with a
receipt, and `bundle outdated` already answers *have they moved*.

**The format keeps section 13 as its own authoritative slice**, and the bundle
cites it rather than copying it. A retirement the format decided is the format's
to state; transcribing it is how this went stale in the first place.

### Its index is always loaded, and that is the prevention half

**Term and replacement, one line each, and nothing else.** Short enough to earn
`matches: always`, which is the only route to being in front of a reader before
work starts — and since the default reversed, the only way to choose it
deliberately.

**This is the part that defends against priors**, and it is the reason the record
cannot stay in a config file. An agent about to write `preload` has to have
already been told the word is dead. Nothing checked after the fact can do that
job, and nothing undelivered can either.

**It is the index pattern, applied to vocabulary** — load what says a thing
exists, load the thing when it matters. The estate already argued this out in
`an-index-of-what-exists`; a retirement list is close to its ideal case, because
the whole payload *is* the index and there is no body to defer.

### Severity comes from the document that carries the word

**Computable from frontmatter, and it is the gradient the audit already used by
hand:**

| the retired term appears in | report as | why |
| --- | --- | --- |
| a `policy` | **finding** | a policy binds. An author following it declares something nothing reads |
| a `workflow` | **finding** | its steps bind by being steps |
| a `document` | **notice** | background and drafts argue about words; a grep cannot tell argument from assertion |
| a `## Version` history, or a record | **exempt** | both state what was true when written |

**F-004 was rated high because it was a policy**, and that judgement was made by
a reader. It should not have needed one.

### Both engines read it, because the failure has two sides

| | runs where | catches |
| --- | --- | --- |
| `luma-foreman inspect` | a project that **adopted** | prose teaching the old way to this repository's agents |
| `luma-catalog-curator check` | a catalog that **publishes** | a bundle teaching the old way to every future adopter |

**The publish side is the one that matters more and is the one missing.** A stale
project document misleads one repository. A stale published policy misleads
everybody who adopts it, and keeps doing so.

## Where each piece lives

**By the estate's rule — *where does it run*, not what is it about.**

| piece | repository | why |
| --- | --- | --- |
| the `retirement` type | `luma-types` bundle | a type a second consumer reads. Via `change-a-shared-type` |
| the estate's retirements | `luma-catalog` bundle | knowledge, published, adopted. No executable code |
| the format's own | `luma-knowledge-format`, section 13 | the format names its own names |
| project-side detection | `luma-foreman` | runs where bundles are adopted |
| publish-side detection | `luma-catalog-curator` | runs where bundles are published |
| this reasoning | `luma-leader` | cross-project reasoning is what it owns |

**No new engine, deliberately.** The estate's naming rule doubles as a
junk-drawer detector: a job that cannot be stated as one verb is more than one
job. This is one — *publish a retirement, deliver it, check it* — and each third
of it already has a home.

## What this does not solve

**A grep still cannot tell a revival from an ordinary use of the same word.** It
never will. `compliance` and `obligation` were both tried in this repository's
retired list and removed the same day: they produced about thirty-three false
positives between them, and `obligation` also names a live concept here — how
strongly a catalog expects a project to adopt a bundle — which is not the field
that was renamed. **A notice list that is mostly noise teaches a reader to skim
past it**, and costs more than the stale references it catches.

**So retiring an ordinary English word is a decision with a cost**, and the
framework should make that cost visible rather than pretend the check is free.
`scope` helps — a word too common to retire estate-wide may still be worth
retiring inside one bundle.

**Nor does it catch a concept renamed without a word changing.** If the shape of
an idea moves but its vocabulary survives, every check here reads clean. That is
the harder half and nothing proposed addresses it.

**It sees only what git tracks.** The rule lists tracked files, so a document
that has been written and not yet committed is invisible to it — which is exactly
the moment an agent has just reinvented something and the cheapest moment to say
so. Writing this draft demonstrated it: three uses of `preload` here were
reported only after `git add`. A pre-commit hook closes it; nothing does today.

**And it does not make delivery certain.** `matches: always` puts the list in
front of an agent that loaded the bundle. An agent working in a repository that
adopted nothing — which describes `luma-catalog` itself today — still gets
nothing.

## What it owes before it is built

1. **Does `retirement` earn a type, or is it a `decision` with fields?** A
   retirement has a decision's shape, and the bar for a new type is that a
   consumer ignoring it would be broken. Argue it either way before writing one.
2. **Who may retire estate-wide?** A format retirement is the format's. A word
   retired in one engine binding all six is a stronger claim than it looks.
3. **What releases an entry?** `ADR-0005` in `luma-foreman` answers this for one
   repository — an entry goes when nothing in the product still needs a name for
   what the word named. Whether that survives being estate-wide is untested.
4. **What is the always-loaded budget?** The list is short now. Retirements only
   accumulate, and the entry that pays for itself least is the oldest one.
