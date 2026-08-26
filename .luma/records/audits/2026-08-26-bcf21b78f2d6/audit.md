---
type: audit
title: The estate, under two lenses — unearned assertions and general quality
audited: 2026-08-26
commit: bcf21b78f2d6
scope: All seven repositories, at the commits pinned below. Two lenses, kept separate.
auditor: agent:claude-opus-5
---

# Audit: the estate, under two lenses

## Scope

**Seven repositories, each pinned before anything was examined.** An estate-wide
audit spans several artifacts, so the directory takes this repository's commit
and every subject is pinned individually here — a finding against a moving tree
is one nobody has to answer.

| repository | commit |
| --- | --- |
| `luma-knowledge-format` | `9b128dea5362` |
| `luma-catalog` | `ae2db5fcab0d` |
| `luma-catalog-curator` | `e044949d008a` |
| `luma-foreman` | `43669da89dbd` |
| `luma-leader` | `bcf21b78f2d6` |
| `luma-clarify` | `c5b06b1978bc` |
| `luma-backlog` | `43e40aceca9e` |

**Two lenses, reported separately** so a reader can tell which findings came
from which question:

- **Lens A — unearned assertions.** Claims that something *was decided*, *was
  chosen deliberately*, *was rejected*, or *is on purpose*, where nothing shows
  a person deciding it. Plus conventions invented by an agent and later cited by
  another agent as precedent.
- **Lens B — general quality.** What the existing checks do not catch: content
  contradicting itself, rules with no consumer, prose describing a design that
  no longer exists, documents nobody could act on.

## Not examined

**The half that makes a clean result meaningful.** None of the following was
looked at, and silence about them here means *never looked*, not *examined and
clean*:

- **Executable behaviour.** No test was written and no tool was run against
  inputs it does not already have fixtures for. Both engines have their own
  suites and those were taken at their word.
- **Security.** No credential scanning beyond what `luma-foreman inspect`
  already runs on tracked content, which excludes history by its own admission.
- **Git history before 2026-08-19.** The estate's earliest commits were not
  read. Lens A therefore under-reports: an assertion introduced early and never
  questioned is exactly the kind this lens exists to find, and it is the kind
  most likely to have been missed.
- **`luma-clarify` and `luma-backlog` beyond their README and structure.**
  Both are seeds with almost no content; they were confirmed to be so and not
  examined further.
- **Prose quality, style, and length** as such. A document is only reported for
  length where the content is dead rather than merely long.
- **Whether the design decisions are *correct*.** This audit asks whether a
  claim is supported and whether content agrees with itself, never whether the
  position is a good one.

## Method

Tool output first, kept as evidence rather than promoted to findings:

- `luma-foreman inspect` in every repository that has a `.luma/`
- `luma-catalog-curator check` and `check --against origin/main` on the catalog
- both engines' own test suites
- `grep` sweeps for assertion phrases, for provenance fields, and for names the
  format has retired

**Tool output is not an audit.** Where a sweep returned many instances of one
pattern, they are one finding with a count, and the judgement — does this
matter, by whose rule, what does it cost — is the part added here.

**Lens A cannot be run mechanically to completion.** A grep finds candidate
sentences; deciding whether a claim is supported needs the git history read
against it, and that was done for the candidates surfaced, not exhaustively.
**Treat Lens A's count as a floor.**

## Findings

| id | lens | severity | summary |
| --- | --- | --- | --- |
| F-001 | A | high | No published document records who wrote it, and none records that a human confirmed it |
| F-002 | A | high | Agent-authored assertions are cited by later agents as settled decisions |
| F-003 | A | medium | `DECISIONS.md` cannot distinguish a decision from a position an agent recorded |
| F-004 | B | **high** | A published policy teaches `preload`, a field the format removed two releases ago |
| F-005 | B | medium | Two engines and the format state the same trigger vocabulary independently |
| F-006 | B | low | `luma-clarify` and `luma-backlog` are adopted by nothing and adopt nothing |
| F-007 | B | low | The catalog's own bundles are unreachable to an agent working in the catalog |

## Summary

**Both lenses found the same underlying cause**, which is the result worth
carrying forward: **the estate records reasoning but not authorship.** Every
document argues for itself, and nothing distinguishes an argument a person made
from one an agent made and nobody read. Lens A finds that directly (F-001,
F-002, F-003). Lens B's largest finding (F-004) is the same failure seen from
the other side — content that outlived its subject and was never re-read by
anyone accountable for it.

**Nothing found here is a defect in behaviour.** Both engines pass their suites,
the catalog passes its own checks, and no repository contradicts the format at
the pinned commits. Every finding is about what a reader — most often an
agent — will conclude from content that is technically correct.

## Response

Not written here. See `conduct-audit` step 6: the auditor does not answer their
own audit, and this one was conducted by the agent responsible for a substantial
share of what it reports.
