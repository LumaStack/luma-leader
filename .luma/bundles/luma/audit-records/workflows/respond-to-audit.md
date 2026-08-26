---
type: workflow
title: Respond to an audit
description: Take a position on every finding — agreed, partially agreed, disagreed, or accepted — and point at the evidence. Use when an audit has been filed against something you are accountable for.
---

# Respond to an audit

## 1. Read the whole audit first

Including `audit.md`'s scope. **A finding outside the stated scope is worth
querying**, and the scope is also where you learn what was *not* examined — which
matters when deciding whether a clean area is actually clean.

## 2. Take a position on every finding

Four, and none of them is a failure:

| position | when |
| --- | --- |
| **agreed** | correct, and you have addressed it |
| **partially agreed** | part is correct. Say which part, and why the rest is not |
| **disagreed** | the finding is wrong — the criteria did not apply, the condition was misread |
| **accepted** | correct, and you are choosing not to fix it. The cost of fixing exceeds the cost of carrying it |

**Answer every one.** Silence on a finding is indistinguishable from having
missed it, and an auditor cannot verify a position nobody took.

**Accepted is not disagreed**, and conflating them loses the more valuable
record. *You are right and we are carrying this deliberately* is a decision
somebody made; six months later it is the difference between a choice and an
oversight.

## 3. Point at the evidence

*"Fixed"* is not a response. Name the commit, the pull request, the file.

**Verification has to be possible without asking you**, or the loop is trust
rather than record — and trust does not survive the person who extended it
moving on.

## 4. Fix the cause, not the condition

Where a finding names a cause, addressing only the symptom leaves it to return
under a different number. If five findings share one cause, say so and address it
once — that is a stronger response than five separate fixes.

## 5. Write `response.md`, and stop

[The response template](../templates/response.md).

**Do not write the verification.** Closing your own findings is the failure this
shape exists to prevent, and it is the easiest one to commit when the same agent
is available to do both.

## 6. Commit it

Beside the audit, in the same directory. Never edit the findings — they are
somebody else's record, and disagreement belongs in your file rather than in
theirs.
