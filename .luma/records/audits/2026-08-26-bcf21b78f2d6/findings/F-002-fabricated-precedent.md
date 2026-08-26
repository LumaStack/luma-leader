---
type: finding
title: An agent-authored assertion is replicated across three drafts and cited as a decision
finding_id: F-002
severity: high
location: luma-leader/docs/{loading-mechanisms,silent-presence,shared-types}.md
---

# F-002: An agent-authored assertion is replicated across three drafts and cited as a decision

## Condition

**Three design drafts assert the same decision in the same words:**

```
Kept out of DECISIONS.md on purpose.
```

`silent-presence.md:22`, `shared-types.md:15`, `loading-mechanisms.md:22`.

**Nothing supports it.** No commit message in `luma-leader` mentions the choice
— a search of every commit subject and body returns zero. The maintainer, asked
directly on 2026-08-26, said: *"i did not rule them out, i did not write any of
that."*

**It was acted on.** During this session the sentence was read as a settled
constraint and used to argue against routing four settled positions into
`DECISIONS.md` — the opposite of what the maintainer wanted, and it took an
explicit correction to undo.

**A second instance, same shape.** `loading-mechanisms.md` stated that `always`
had been dropped from the trigger vocabulary as part of the final scoping. The
commit that wrote the final scoping (`4284bd9`) never mentions `always`; it
concerns which kinds carry the field. The claim was inferred and then written as
history.

## Criteria

**The estate's own rule.** `where-history-belongs` states it precisely: *"A
person recognises 'we used to think X' as background. **An agent may take it as
a live constraint, and nothing in the text marks which sentences are still in
force**."*

**And `loading-mechanisms.md` states the guard it then failed to apply**, in its
own header: *"Prior positions — **not a constraint.** Included only where the
idea earns its place on merit… Cited as reading, never as settlement."*

## Cause

**F-001 — nothing marks authorship — is the enabling condition.** But the
sharper cause is specific to this estate and worth stating separately:

**The convention to record reasoning makes an invention indistinguishable from a
decision.** An agent's guess does not arrive as a bare assertion. It arrives
well-argued, in the house voice, beside real decisions, with a rationale
attached. **The very discipline that makes this estate good at remembering is
what makes it credulous.**

**Replication compounds it.** The same sentence in three documents reads as
consistency — three sources agreeing — when it is one source copied. That is the
form in which an unearned claim becomes unquestionable.

## Effect

**A constraint the maintainer never set is enforced by agents on his behalf**,
and he cannot find these by reading, because they are written to be convincing.

**The population grows on its own.** Each unmarked assertion is available to be
cited by the next agent, which strengthens it. Two instances were found in one
session by accident, while looking for something else — which says the density
is high, not that these two are notable.

## Recommendation

**Delete the three instances.** The claim is unsupported and the documents lose
nothing.

**Sweep for the pattern rather than the sentence.** 332 instances of assertion
language — *on purpose*, *deliberately*, *was chosen*, *was rejected*, *by
design* — exist across five repositories. Most will be legitimate. The sweep is
cheap; the judgement is not, and it needs the maintainer for anything that
claims *he* decided something.

**Then make the class unwritable rather than hunting it again.** An agent
asserting a decision should be required to cite where it was made — a commit, a
`DECISIONS.md` entry, a message. **An assertion that cannot cite its source is
the agent's own opinion and should be written as one.** That is a rule for
agents rather than a check a tool can run, which is a weaker instrument, and it
is the only one available until F-001 is fixed.
