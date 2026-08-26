---
type: policy
title: How audits are stored
description: Where an audit lives, how it is named, and the three-party loop — auditor, respondent, auditor again — that makes the record settle rather than accumulate.
matches:
  - topic: conducting an audit, or recording one
sources:
  - id: iia-glossary
    resource: https://www.moxo.com/blog/internal-audit-glossary
    title: Modern internal audit glossary — independence, findings, follow-up
  - id: management-response
    resource: https://www.v-comply.com/glossary/management-response/
    title: "Management Response in Audits: Definition, Elements & Examples"
  - id: corrective-action
    resource: https://www.fieldpie.com/blog/audit-corrective-action-plan/
    title: "Audit Corrective Action Plan: Workflow, Owners & Closure"
---

# How audits are stored

```
.luma/records/audits/2026-12-23-e286f3bd9c11/
  audit.md                              scope, commit, what was not examined
  findings/
    F-001-unquoted-frontmatter-links.md
    F-002-...
  response.md                           a position per finding
  verification.md                       closed, or open and why
```

## The directory name pins the claim

**`<DATE>-<SHA>`** — the day it was performed, and the commit it is true of.

`2026-12-23-e286f3bd9c11`

The date sorts chronologically and reads without tooling. The commit is what
makes the audit checkable: anybody can reproduce the state it examined, which is
the difference between a record and an assertion.

**At least twelve characters of SHA.** Git's seven-character default begins
colliding in repositories large enough to warrant auditing, and an ambiguous
commit makes the record unverifiable exactly where verification matters.

*Two audits of the same commit on the same day* — different scopes, different
auditors — take a suffix: `2026-12-23-e286f3bd9c11-security`. Rare, and cheaper
to allow for than to discover.

## The parties, and why their independence is the point

The shape is internal audit's, compressed: an independent function reports
findings, the party accountable for the work responds with a position and a plan,
and the auditor follows up to confirm the response actually
worked.[^iia-glossary][^corrective-action] What is borrowed is the separation of
those three acts — not the committees around them.

| | writes | who |
| --- | --- | --- |
| 1 | `audit.md` and the findings | the **auditor** |
| 2 | `response.md` | the **respondent** — accountable for the thing audited |
| 3 | `verification.md` | the **auditor** again |

**Any of the three may be a person, an agent, or a command.** They are recorded
as actors (§7.4), so `human:fsmith`, `agent:opus-5`, and
`process:luma-foreman-inspect` are all valid and all mean something different to
a reader. Record the most specific one available — a report attributed to
`unknown:unknown` cannot be weighed against anything.

**One party must not write two of these.** An auditor who writes the response is
grading their own work; a respondent who writes the verification is closing their
own findings. Where a swarm of agents is doing every step, that separation has to
be arranged deliberately, because nothing enforces it.

*The exception is a deterministic command.* A checker that produced a finding can
legitimately verify it, because re-running it is evidence rather than an opinion
about its own work — it will report the same thing whoever is watching. That is
not true of an agent, which is why the exception is narrow and worth naming
rather than generalising.

That is also why this shape is worth the ceremony. **The exchange is the record**
— an auditor can see whether a finding was genuinely addressed, a respondent can
disagree without the disagreement being erased, and both survive in git long
after anybody remembers the conversation.

## Findings are one file each

**Required when more than one auditor is running.** A swarm writing into a single
file is a merge conflict per agent, which serialises the work parallelism was
for. This is mechanical, not stylistic.

**Recommended when one auditor is running**, for two reasons that arrive later
than the audit does. An audit that starts solo often does not stay solo. And a
finding gets cited individually — in a response, in a verification, in a
decision record a year afterwards — which needs it to have an address.

`F-001`, `F-002` — stable within their audit, and the handle everything else
refers to. The slug after the number is for humans; the number is the identity.

**A single auditor may instead keep findings as sections in `audit.md`.** The
cost, stated plainly rather than discovered: those sections are not `finding`
Documents. They cannot be linked to, they carry no `severity` a tool can read,
and a consumer of audits now has two shapes to handle. Splitting them out later
is a mechanical move but it is a move.

Take the shortcut for two or three findings in a one-off audit. Do not take it
for anything that will be responded to formally.

## Nothing records its own status

An audit carries no state. Neither does a finding. **What happened to a finding
is written by whoever acted on it, in their own Document.**

Current state is *derived* by reading audit, response and verification together.
It is never stored, because storing it would mean somebody editing a record they
did not write — which is the one thing an append-only record exists to prevent.

The cost is that *"show me every open finding"* reads three Documents rather
than one field. That is what a derived index is for, and an index is a cache: it
can be rebuilt and it is never the source.

## Rounds

A verification that leaves a finding open produces `response.md` round 2, then
`verification.md` round 2 — not an edit to the first pair.

The first response is a record of a position genuinely held at the time, and
overwriting it destroys the reason the exchange was worth keeping. Most audits
never reach a second round.

## Auditing across repositories

Uncommon, and worth having a rule for before it happens rather than after.

**One repository owns the record.** The audit lives there, in its `.luma/`, and
**the commit in the directory name is that repository's**. There is no shared
place for it to live, and inventing one would mean a directory nothing owns.

Choose the owner by **where the work will happen** — the repository whose change
would resolve the most findings. Failing that, the one most affected. Do not
choose the one you happen to have open.

**Pin every other repository you looked at**, in the audit's `also_examined`:

```yaml
also_examined:
  - repository: ../luma-foreman
    commit: 07c062e1a2b3
```

Without it the audit is unreproducible. One commit pins one side of a claim about
several, and the others move immediately — a finding about how two repositories
interact cannot be checked against *whatever those were at the time*.

**Say which repository each finding is about.** In a single-repository audit it
is obvious and needs no field; here it is the first thing a respondent needs, and
it belongs in the finding's `location` — `luma-foreman:src/foreman/policy/gate.py`.

### The failure this shape does not solve

**A finding about repository B, filed in repository A, is not visible to anybody
working in B.** Nothing propagates it, and no amount of care in the record fixes
that — it is a distribution problem rather than a format one.

Two mitigations, both manual and both worth stating rather than implying:

- **Tell the other repository.** An issue, a message, a pull request — whatever
  that project actually reads. The audit is the record; it is not the
  notification.
- **The response may have to be written by somebody else.** When a finding is
  resolved by a change in B, the response still goes in A beside the audit, and
  it cites B's commit. Whoever writes it needs access to both.

If cross-repository audits become routine rather than rare, that is a signal to
solve distribution properly rather than to keep refining this section.

## What belongs here, and what does not

**This is for findings, whoever or whatever produced them.** A command can be an
auditor: a checker that emits well-formed findings — condition, criteria,
severity by consequence — is doing exactly the job, and its output belongs here
under `auditor: process:<name>`.

**What does not belong is a raw dump.** A scanner reporting eleven instances of
one pattern is **one finding with eleven locations**, not eleven findings. Piping
tool output straight into this directory inflates the count, flattens severity,
and buries whatever a person actually reasoned about — which is how a project
learns to stop reading its own audits.

The test is not who wrote it. It is whether each finding says what is wrong, by
whose rule, and what it costs. A command that produces that is an auditor. A
command that produces a list of line numbers is evidence, and belongs cited
*inside* a finding rather than filed as one.
