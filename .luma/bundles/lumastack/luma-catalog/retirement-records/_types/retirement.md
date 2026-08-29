---
type: type_definition
defines: retirement
fields:
  retirement_id:
    field_presence: required
    field_type: text
    desc: "3–5 letters, a dash, then digits — RET-0004. Stable across every revision of this strategy; what a sweep, an overlay and an audit finding all cite"
  retired_at:
    field_presence: required
    field_type: timestamp
    desc: "when the thing stopped being how we do it. A time rather than a date — renames land minutes apart and a date cannot order them against the commits they describe"
  origin:
    field_presence: required
    field_type: enum
    values: [project, organization]
    desc: "project — it arose from the work and promotes upward on evidence. organization — leadership decided it and it is handed down"
  scope:
    field_presence: required
    field_type: enum
    values: [project, peer, organization, estate, unknown]
    desc: "how far it reaches. `unknown` is a real state and must not distribute — probe first"
  scope_evidence:
    field_presence: recommended
    field_type: text
    desc: "why that scope. A probe result when origin is project, the mandate when it is organization — so a reader can tell a measured scope from an assumed one"
  decided_in:
    field_presence: required
    field_type: text
    desc: "where the decision lives and which one — `lumastack/luma-foreman#ADR-0003`. A citation, not a link to follow; the reader usually cannot open it"
  was:
    field_presence: required
    field_type: text
    desc: "the retired idea, in one line. Distributed payload — keep it to a line"
  now:
    field_presence: required
    field_type: text
    desc: "what replaced it, in one line, or what is simply gone. Distributed payload"
  why:
    field_presence: recommended
    field_type: text
    desc: "one line, so the strategy stands alone when the decision is in a repository the reader has never cloned"
  enforced:
    field_presence: optional
    field_type: date
    desc: "when non-compliance stops being a notice and becomes a finding. Absent means immediate, which is the house default — a grace period is the deliberate opt-in"
  recognizers:
    field_presence: required
    desc: "how to spot the retired idea, one or more. No field_type: a list of records, which the format's field declarations cannot yet express. See below"
  except:
    field_presence: optional
    desc: "paths where a hit is correct as it stands. No field_type, same reason"
---

# Retirement

**One retired idea, and how to recognize it.** A retirement is the *strategy* for
retiring — what is dead, what replaced it, how to find it. It is not the decision
that retired it.

## It is not a decision, and the difference is which way each one is heading

A `decision` says *why we changed our mind*. It lives in the repository that
decided, and it is cited rather than distributed. **A decision is heading toward
being locked** — that is the point of settling one.

A retirement says *what is dead and how to spot it*. It is **published, adopted
and distributed**, and it **must stay revisable for as long as it binds** — you
find a fourth disguise, you exempt a document that legitimately argues about the
word, you narrow after false positives.

**How much a document may be edited follows its `lifecycle`**, and that is
what separates these two rather than any blanket rule:

| | `draft` | `provisional` | `stable` |
| --- | --- | --- | --- |
| a **decision** | revised freely | tweaked | **locked.** Corrections are appended and dated |
| a **retirement** | revised freely | tweaked | **still takes new recognizers** — that is not a correction, it is the strategy working |

A stable decision cannot absorb a recognizer discovered two years later. A stable
retirement has to. **Fusing them would force an amendment to a settled position
every time a sweep taught you something.**

`decided_in` is the join. One decision can produce several retirements, and one
retirement can be revised many times under a stable `retirement_id`.

## `recognizers` — the tier decides who can detect it

**One or more, and the `kind` decides who can act on it.**

```yaml
recognizers:
  - kind: term                    # a word, field, filename, command
    value: preload
  - kind: shape                   # a frontmatter key or structural pattern
    value: "frontmatter key `preload`"
  - kind: claim                   # prose stating the old model
    value: "any document telling an author to declare when their document loads"
```

| kind | detected by | costs |
| --- | --- | --- |
| `term` | a deterministic match | nothing; runs in CI |
| `shape` | a structural query | nothing; runs in CI |
| `claim` | **an agent reading** | a session, and it is a judgement |

**A concept retirement usually declares a `claim` and no `term` at all**, which
is the case this type exists for. A word is the cheapest recognizer, not the
important one — the retirements that do damage are the ones whose vocabulary
survived intact.

## `field_type: timestamp` is new

`field_type` is an open vocabulary, so this adds a name rather than a
mechanism. `retired_at` needs one because the estate's date fields — `decided`,
`audited`, `archived` — cannot order two renames that landed in the same
afternoon. **`enforced` is deliberately a plain date**: nobody complies at 14:32.

## `lifecycle` carries release

From the root type. `archived` means **released** — the entry no longer binds,
because nothing in the product still needs a name for what the word named. The
test is in [[release-a-retirement]].
