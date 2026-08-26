---
type: workflow
title: Conduct an audit
description: Examine something, record what is wrong and why, and leave a record somebody else can act on. Use when auditing a codebase, a process, or any artifact that has a commit.
---

# Conduct an audit

## 1. Pin the commit before you look at anything

```sh
git rev-parse --short=12 HEAD
```

**An audit is a claim about a specific state.** Pin it first, because a codebase
that moves under you produces findings that cannot be reproduced — and a finding
nobody can reproduce is one nobody has to answer.

```sh
DATE=$(date +%F)
SHA=$(git rev-parse --short=12 HEAD)
DIR=".luma/records/audits/${DATE}-${SHA}"
mkdir -p "$DIR/findings"
```

If several audits of the same commit will run on the same day, add a scope
suffix now rather than discovering the collision later:
`${DATE}-${SHA}-security`.

## 2. Decide the scope, and write down what you are not looking at

Before examining anything. Scope decided afterwards is scope fitted to what you
happened to find.

**The half people skip is what was excluded.** *"Reviewed the HTTP layer; did not
examine authentication or the data model"* lets a reader tell *examined and
clean* from *never looked*. Without it, a clean audit is indistinguishable from
a shallow one.

## 3. Examine

Where a tool does part of the job, run it and keep the output as evidence rather
than as findings. `luma-foreman inspect` and its equivalents belong here.

**Tool output is not yet an audit.** Eleven instances of one pattern is one
finding with eleven locations. The judgement — does this matter, by whose rule,
what does it cost — is the part you are here to add.

## 4. Write one file per finding

```
findings/F-001-<short-slug>.md
```

Numbered from `F-001`, stable within this audit, and the handle every response
and verification will use.

**Required when more than one auditor is running** — a swarm writing into a
single file is one merge conflict per agent. Recommended even alone; see
[[audit-layout]].

Each finding carries condition, criteria, cause, effect and recommendation, with
severity rated on consequence rather than effort. [[writing-findings]] covers
what makes each of those actionable, and [the finding template](../templates/finding.md)
has the shape.

## 5. Write `audit.md`

Scope, commit, what was excluded, who audited, and a summary of the findings by
severity. Not a restatement of them — a reader should be able to decide from this
file alone whether they need to read the rest.

[The audit template](../templates/audit.md).

## 6. Stop before writing the response

**You are not the party that answers this.** An auditor who writes the response
is grading their own work, and the record loses the only property that made it
worth keeping.

Where a swarm is doing every step, arrange that separation deliberately —
nothing enforces it, and one agent doing both jobs is the easiest failure to
commit by accident.

## 7. Commit it

One commit, the whole audit directory. It is a record: append-only, dated, and
never edited afterwards. What happens next is written by somebody else in their
own file.
