---
type: finding
title: The trigger vocabulary is stated independently in three places and can drift
finding_id: F-005
severity: medium
location: SPEC.md §10.7, luma-foreman/src/foreman/inspect/rules/bundles.py, luma-catalog-curator/src/curator/catalog.py
---

# F-005: The trigger vocabulary is stated independently in three places and can drift

## Condition

**The closed vocabulary of trigger kinds and lifecycle events is written out
separately in three artifacts**, each maintaining its own copy:

- `SPEC.md` §10.7 — the normative table of kinds, and the six `event` values
- `luma-foreman` — `TRIGGER_KINDS` and `EVENTS`, used to report unknown kinds
- `luma-catalog-curator` — its own parser, which decides what counts as a trigger

**They agree today.** They were made to agree by hand, three times in one day,
during the `matches` migration: `always` was removed from the spec table and
from foreman's tuple in separate commits, and the curator's parser was changed
separately again.

**They have already disagreed.** Before 2026-08-26, `always` was a member of
foreman's `TRIGGER_KINDS` and of the spec's table, while `loading-mechanisms.md`
listed five kinds and omitted it — and the tool could not correctly parse the
value it accepted. That disagreement shipped and went unnoticed until a reader
asked about it directly.

## Criteria

**The estate has a rule for shared vocabulary and it is not being followed.**
`change-a-shared-type` exists because *"a breaking change is never one release,
it is three"*, and its check-your-work test is: *"Did you skip expand? If any
moment in the sequence required two tools to ship together, you did."*

**Removing `always` required exactly that.** The spec, foreman and the curator
had to agree simultaneously or a document valid under one would be misread by
another. The estate's own workflow says that is the failure, and it was
committed knowingly during the migration.

## Cause

**A closed vocabulary shared across a specification and two independent tools
has no owner.** The format cannot ship the tools' constants; the tools cannot
depend on the format at runtime, because a Bundle is self-contained and nothing
is fetched to read it.

**This is a known open problem in the design and is written down.**
`loading-mechanisms.md` records the same shape for path roles — *"shared
vocabulary needs an owner, and each candidate costs something"* — and defers it.
The trigger vocabulary is the same problem, arrived at from a different
direction, and it was not recognised as such.

## Effect

**A rename requires three coordinated edits and nothing detects a missed one.**
The failure mode is precisely the one already observed: a kind the tool accepts
and cannot parse, or a kind the spec defines and the tool rejects. **Both
publish cleanly.**

**It is small today and grows with each consumer.** A third tool reading
`matches` makes it four places.

## Recommendation

**Do not build a shared-constants mechanism.** It would be the shared-vocabulary
problem the design has already deferred twice for good reasons, arriving through
a smaller door.

**Make disagreement detectable instead**, which is cheap and fits what already
exists: a check that reads the vocabulary out of `SPEC.md` and compares it to
each tool's constant, run in the tools' own suites. The spec is the normative
source, both tools vendor the format's bundle already, and the check fails
loudly at the moment somebody edits one copy.

**Record the deferral either way.** *Shared vocabulary across the spec and its
consumers has no owner* is the same open question as path roles, and it should
be one entry rather than being rediscovered a third time.
