---
type: policy
title: Retiring a concept
description: What may be retired, how far it reaches, and why a word is the cheapest recognizer rather than the important one. Read before retiring anything.
matches:
  - topic: retiring an idea, a concept, a field or a word
  - topic: deciding how widely a change should be enforced
---

# Retiring a concept

**A retired idea comes back by being reinvented, not by being remembered.** The
ideas worth retiring are usually the obvious ones for the job, so absence from a
repository is a weak defence — an author reaches for one again and it reads as a
fresh choice rather than a revival. **That has been observed happening within
minutes of a sweep that removed one.**

So this is not a documentation problem with a checking solution. It is two
problems needing two defences.

| the old idea arrives from | what stops it |
| --- | --- |
| files that still assert it | a sweep, after the fact |
| **the author's own priors** | **[[what-we-retired]], in front of them beforehand** |

**Nothing checked after the fact can touch the second.** There is no file to
search before somebody writes a line.

## A word is the floor, not the product

**Three recognizers, and the tier decides who can detect it.**

| tier | recognizer | detected by |
| --- | --- | --- |
| 1 | `term` — a word, field, filename, command | a deterministic match |
| 2 | `shape` — a frontmatter key, a structural pattern | a structural query |
| 3 | `claim` — prose stating the old model | **a reader** |

**The retirements that do real damage are tier 3**, because the vocabulary
survives and a search finds nothing. *A catalog **declares** its namespace* became
*it **derives** from where it lives* — the word "namespace" is in both. *A
different party* became *a different session* — "independence" is in both.

**Every retirement declares at least one recognizer, and a concept retirement
often declares a `claim` and no `term` at all.** A retirement with only a `term`
is usually a retirement somebody has not finished thinking about.

## Retiring an ordinary English word has a cost

**The check hands a judgement to a reader that it cannot make**, so every hit is
theirs to resolve. That is affordable until the list is mostly noise.

**Two words were tried in this estate and withdrawn the same day.**
`compliance` — *"a compliance team"*, *"compliance requirements"*. `obligation` —
which also **names a live concept**, how strongly a catalog expects a project to
adopt a bundle, unrelated to the field that was renamed. About thirty-three false
positives between them.

**Noise teaches a reader to skim past notices**, and a skimmed check is a check
nobody runs. **Do not retire a word that is ordinary English in the documents it
will be checked against**, unless `scope` can narrow it to where it is not.

## Scope is decided first, and from evidence

**Before any recognizer is written**, settle how far this reaches. It only ever
widens: you cannot un-adopt from everyone who took it.

| scope | means |
| --- | --- |
| `project` | dies here. Nothing else held the idea, and nothing is published |
| `peer` | a few named projects hold it; the organization is not involved |
| `organization` | everything under one org |
| `estate` | every project, every relevant organization |
| `unknown` | **nobody has looked yet.** Must not distribute |

**`unknown` is a real state and probing is cheap.** Run the tier-1 and tier-2
recognizers across candidate repositories to answer *does this appear anywhere
else*. Record the answer in `scope_evidence`, so a later reader can tell a
measured scope from an assumed one.

*A probe cannot prove absence for a `claim`.* It is enough to tell *nowhere else*
from *four repositories*, which is the decision it has to serve.

## Conservative or wide: the posture follows the tier

**The two errors are not symmetric.**

- **Too wide** costs noise, and noise disables the check everywhere.
- **Too narrow** costs the idea surviving somewhere unseen, and leaking back —
  invisible until it reappears as somebody's fresh choice.

**So: go wide on a concept, stay narrow on a common word.** A `claim` cannot be
found later by searching and leaks back invisibly, so under-distributing it is
the worse error. A `term` that is ordinary English is the reverse.

*One override.* If the idea is load-bearing and its failure is silent — a
published policy teaching a field nothing reads — go wide regardless of tier.
Reaching every adopter is the whole cost of being wrong.

## Two origins, running opposite ways

**`origin: project`** — it arose from the work. Scope is measured by probing,
starts narrow, and promotes upward as evidence arrives.

**`origin: organization`** — leadership decided it, and it is handed **down**.
Scope is asserted rather than probed, and `scope_evidence` records the mandate.
**A project cannot decline it.** It may widen its own net and exempt its own
documents; narrowing a mandate is not available, which is what makes it one.

## `enforced` gives a mandate teeth without an enforcement mechanism

**Absent means immediate**, which is the default here: a rename is a clean break,
with no aliases and no deprecation period. A grace period is the deliberate
opt-in.

**What the date changes is severity, not detection.** Before it, a hit is a
notice. After it, the same hit is a finding at whatever severity its carrying
document earns.

*Its known weakness, stated rather than discovered:* a date nothing checks on a
schedule is decoration. It is surfaced whenever a sweep runs, and nothing here
makes a sweep run.

## Severity comes from the document carrying the idea

**Not from the retirement, and not from a reader's judgement.**

| the retired idea appears in | it is |
| --- | --- |
| a `policy` or a `workflow` — both bind | a **finding** |
| a `document` — background and drafts argue about ideas | a **notice** |
| a `## Version` history, or a record | **exempt** — both say what was true when written |

**A binding rule that teaches a dead idea is teaching somebody to do something
that cannot work**, which is why the type decides this and not the severity of
the word.
