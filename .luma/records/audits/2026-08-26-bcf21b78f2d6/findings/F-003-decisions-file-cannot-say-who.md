---
type: finding
title: DECISIONS.md cannot distinguish a decision from a position an agent recorded
finding_id: F-003
severity: medium
location: luma-leader/docs/DECISIONS.md
---

# F-003: `DECISIONS.md` cannot distinguish a decision from a position an agent recorded

## Condition

**The file has 25 entries and no field on any of them saying who decided.** Each
is a `##` heading plus prose, with a bolded `**Settled <date>.**` line. Nothing
records the decider, and the file's own header commits it to a stronger claim
than it can support: *"Settled positions and the reasoning that settled them."*

**It has already been wrong once, on 2026-08-26.** An entry titled *Generated
artifacts stay committed* was written as `**Settled 2026-08-26**` on the basis
of a maintainer message that hedged twice in one sentence — *"im not sure, but
my initial instinct is…"*. It was merged, then corrected the same day. The
correction is visible in the entry rather than silent, so anything citing it in
between can be found.

## Criteria

**The `decision` type has the field this file lacks.** `luma/decision-records`
defines `decided` as a date, `reopen_trigger`, `superseded_by`, `archived`, and
`archived_reason` — and it inherits `created` and `verified` from the document
root, so *who settled it* and *who confirmed it* are both expressible.

**The estate publishes the layout this file is not in.** `record-decision` ranks
the homes and says `.luma/records/decisions/` "wins when several exist, because
it is the mature shape"; `luma-layout` reserves `records/` as *"what happened,
and why. Append-only, dated, never edited."* `luma-leader` has no `.luma/records/`.

## Cause

**The file predates the bundle**, and `luma-leader` has never adopted
`luma/decision-records` — so the machinery that would fix this is published,
adopted by `luma-foreman`, and absent exactly where the decisions are.

**Migration is a known one-off with a known risk**, already written down:
`migrate-decisions` warns that *"a decision file is cited… splitting the file
breaks every one of those references at once, and a broken citation to a
decision is worse than a missing one."*

## Effect

**A file whose entire purpose is to be authoritative cannot say whose authority
it carries.** It is the most-cited artifact in the repository — design drafts
point at it — so an entry an agent wrote inherits the credibility of the
twenty-four beside it.

**And it is where F-002's damage lands.** An unearned assertion in a draft is
one agent's opinion. The same assertion promoted into `DECISIONS.md` becomes the
estate's position, and this session came within one correction of doing exactly
that.

## Recommendation

**Adopt `luma/decision-records` in `luma-leader` and run `migrate-decisions`.**
It is deferred to its own session by the maintainer, which is the right call —
the risk is the citations, not the splitting.

**Until then, one line per entry costs nothing:** who settled it, and where they
said so. An entry an agent drafted from a conversation says so. **An entry
nobody can attribute is a candidate for F-002 and should be read as one.**
