---
type: retirement
retirement_id: RET-0006
retired_at: 2026-08-26T18:10:15Z
origin: project
scope: estate
scope_evidence: "decided in the catalog's audit-records bundle; binds anybody running the three-party audit loop"
decided_in: "lumastack/luma-catalog#PR-86"
was: "audit independence means a different **party** — one party must not write two of the three documents"
now: "independence is a **session** boundary. A fresh session of the same model is a real second party; a different model is better; one session doing both is permitted, weakest, and must be disclosed"
why: "what compromises a record is carried context, not identity — an agent that argued a finding into existence defends it rather than re-deriving it"
recognizers:
  - kind: claim
    value: "any statement that audit independence turns on who or what the actor is — a different person, a different model, a different party — rather than on whether context carried between the two acts"
  - kind: claim
    value: "any statement that one party must not write two of the three audit documents, which also contradicts the auditor writing both the audit and the verification"
except:
  - ".luma/records/"
lifecycle_status: stable
---

# RET-0006: independence means a different party

## What it was

*One party must not write two of these.* Independence was a property of the
actor: a different person, a different model, a different party.

## What replaced it

**The session is the unit.** A fresh session of the same model is a legitimate
second party; a different model is better still because it brings independent
priors as well as a clean context; one session playing both parts is permitted,
weakest, and has to be disclosed.

The old rule was also self-contradictory: the auditor writes both the audit and
the verification **by design**, because the party that raised a finding is the
one entitled to say whether the answer resolved it. Only the response has to come
from somebody else.

## How to recognize it

**No word was retired here either.** "Independence", "party", "auditor" are all
current. The recognizer is the *criterion*: prose grounding independence in
identity rather than in carried context.

A useful tell in practice: a response that agrees with every finding is the shape
one session playing both parts produces.

## Where a hit is correct

Version histories, and audit records filed under the old rule — they describe an
arrangement that was correct when they were written.
