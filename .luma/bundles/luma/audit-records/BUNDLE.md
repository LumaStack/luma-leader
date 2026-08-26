---
type: bundle
version: 0.5.0
published: 2026-08-25
consumers: [project, organization]
entry_point: policy/audit-layout
description: Audits as records — findings written by one party, answered by another, closed by the first. The whole exchange lives in git.
---

# Audit records

An audit is worth keeping only if somebody answers it. This bundle carries the
shape that makes the answer part of the record: **findings written by one party,
a position taken by another, and a closure written back by the first.**

The exchange is the point. An auditor can see whether a finding was genuinely
addressed rather than merely acknowledged, a respondent can disagree without the
disagreement being erased, and both survive in git long after anybody remembers
the conversation.

```
.luma/records/audits/2026-12-23-e286f3bd9c11/
  audit.md          scope, commit, what was NOT examined      auditor
  findings/F-001-*.md                                          auditor
  response.md       a position on every finding                respondent
  verification.md   closed, or open and what would close it    auditor
```

## What is here

**Policy**

- [[audit-layout]] — where audits live, how they are named, and the three-party
  loop. Read first.
- [[writing-findings]] — what makes a finding actionable rather than an opinion.

**Workflows**

- [[conduct-audit]] — pin the commit, scope it, write the findings.
- [[respond-to-audit]] — take a position on every one, and point at evidence.
- [[verify-audit]] — check rather than accept, and close the loop.

**Templates** — [audit](templates/audit.md) · [finding](templates/finding.md) ·
[response](templates/response.md) · [verification](templates/verification.md)

## The four ideas worth knowing before reading further

**Independence.** One party must not write two of the three documents. An
auditor who writes the response is grading their own work; a respondent who
writes the verification is closing their own findings. Where a swarm of agents
does every step, that separation is arranged deliberately, because nothing
enforces it.

**Any of the three may be a person, an agent, or a command.** They are recorded
as actors — `human:fsmith`, `agent:opus-5`, `process:luma-foreman-inspect` — and
each means something different to a reader. A deterministic command may verify
its own findings, because re-running it is evidence rather than self-assessment;
that exception does not extend to agents.

**Nothing records its own status.** Findings are immutable. What happened to one
is written by whoever acted on it, in their own document, and the current state
is *derived* by reading the exchange. Storing a status would mean somebody
editing a record they did not write — the one thing an append-only record exists
to prevent.

**Disagreement and acceptance are outcomes, not failures.** *You are right and
we are carrying this deliberately* is a legitimate answer and the most valuable
record of the four: six months later it is the difference between a decision
somebody made and something everybody forgot.

## Consumers

Both levels. A project audits its code and its own practice; an organization
audits conformance across projects. The exchange is the same shape either way.

## Version

`0.5.0` — **`applies_to` is now `matches`.** The old name obliged an author to
write a false sentence: `applies_to: everything` claims a rule governs
everything, and none does — what a rule governs is stated in its body, where no
frontmatter value reaches. The field says what makes a Document *surface*, which
is smaller and true, and it reads as a sentence in every form it takes: matches
`git commit`, matches always, matches nothing.

**The default reverses with it.** A Document that says nothing is now available
on request rather than loaded into every session. Nothing here is affected —
every rule in this bundle already states what surfaces it — but a rule that
genuinely should always be present now says `matches: always` rather than
staying silent and being treated as though it had.

Minor. Nothing a reader is obliged to do has changed; the field it is declared
in has been renamed, and `applies_to` is still read while the rename finishes.

`0.4.0` — **vocabulary.** `moment` becomes `event` — a moment is a point in
time and `applies_to` takes nouns. `compliance` is dropped wherever it was
saying nothing: a policy binds unless it says otherwise, so only a strong
default declares `recommended`, and a workflow's steps bind by being steps.
Type Definitions use `field_presence: required` for what was
`obligation: mandatory`, matching the format.

Minor. Nothing a reader is obliged to do has changed; what declares it has.

`0.3.0` — **`preload` is replaced by `compliance` and `applies_to`.** An author
now says how strongly a rule binds and when it governs; *when it is delivered* is
computed from those and never declared. Every rule here could state when it
applies, so **nothing in this bundle is loaded unconditionally any more** — it
arrives when the work matches and costs nothing before then.

Minor: a consumer reading `preload` finds nothing, and the loading behaviour of
every document changes.

`0.2.0` — **the manifest is `BUNDLE.md`.** Reserved markdown files are now
ALL CAPS across the estate, because nobody types all caps by accident: a file
becomes load-bearing only when somebody deliberately made it so, and writing
`bundle.md` now fails in the safe direction — ignored rather than silently wired
into machinery. Minor rather than patch, and pre-1.0 that is the tier for a
breaking change: anything naming the old path by hand stops resolving.

`0.1.1` — a heading no longer says how many things are beneath it. Wording only.

Patch: no normative sentence moved and a reader who correctly understood
`0.1.0` behaves identically. See `writing-style` in `luma/project-documentation`
for the rule and the failure it prevents.

`0.1.0`. The structure is borrowed from internal-audit practice, which is
well-tested — but this particular compression of it, into files in a repository
answered by agents, has been run zero times. The round-two mechanics in
particular are reasoned rather than exercised.
