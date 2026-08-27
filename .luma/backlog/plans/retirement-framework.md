---
type: document
title: A retirement framework
description: How a retired idea comes back, and the framework that has to defend both directions — the decision that stays home, the strategy that travels, and the three tiers that decide who can detect it.
lifecycle_status: provisional
created: { by: human:benlinton, at: 2026-08-27T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-27T00:00:00Z }
---

> **Filed under `backlog/` rather than `records/`, deliberately.** A record is
> append-only and says what was true when written. **A plan is intended work and
> gets revised as it is executed** — this one changed substantially while it was
> being written. Putting it in `records/` would mean amending a record every time
> the build teaches us something, which is the same mistake this plan spends a
> section avoiding.
>
> *It is `type: document` because no `plan` type exists. Inventing one in passing
> would be worse than the gap.* **Plans belong in backlogs**, and `luma-backlog`
> is the repository that owns intended work — so a `plan` type, when it is worth
> having, is very likely its to define rather than this one's. Until then a plan
> is an ordinary Document filed where the work is tracked.
>
> **A plan should attach to the work item it plans**, not float beside it. That
> attachment is unbuilt because the unit has no settled name yet —
> `where-backlog-types-live` lists `deliverable`, `wave`, `outcome` and `task`,
> and `rename-deliverable-to-issue-or-story` records that `deliverable` is
> probably wrong. **Naming the field before naming the unit would be the wrong
> order**, so this plan carries no parent reference and should gain one when the
> unit is settled.
>
> **Approved 2026-08-27. Phase 1 is done** — `retirement-records` 0.1.0 is
> published in `luma-catalog` and adopted here. Its first sweep found two
> tier-3 hits, neither containing a retired word. Phases 2 and 3 are unstarted.


## Context

When one repository moves a foundational idea, the others go on teaching the old
one **as instruction rather than as history**, and a reader arriving cold cannot
tell the difference. Worse, the old idea returns even from repositories that are
clean: `luma-foreman`'s own `inspect/rules/vocabulary.py` records a retired word
coming back *"within minutes of a sweep that removed one"* — because what
produced it was never the files, but a model reaching for the obvious word for
the job, which is the word that got retired for being obvious.

Today's mechanism is `[[retired]]` in `.luma/config/luma-foreman.toml`. It is
per-project, hand-transcribed (this repo's list was copied out of `SPEC.md` §13
by hand on 2026-08-27 and will go stale silently), flat in severity, blind on the
publish side where `luma-catalog` has no config at all — and **never distributed to
the agent**, which is the case that matters.

It also only finds *words*. The retirements that actually did damage this week
were concepts whose vocabulary survived intact:

| retired concept | the word that survived |
| --- | --- |
| a catalog **declares** its namespace → it **derives** from where it lives | "namespace" |
| independence means a different **party** → a different **session** | "independence" |
| `refit` is an unbuilt stub → removed, split across three checks | "recurring check" |

A grep finds none of those. **Word matching is the floor, not the product.**

The outcome wanted: a retirement is a **published, adopted, distributed** record;
a concept retirement is as first-class as a word one; and any project can answer
*have I swept against everything the estate has retired, and at what version.*

### What "the estate" means here

**Every project we own, under every relevant organization** — not one GitHub org
and not the six repositories `policy/the-estate` currently enumerates. That
policy predates this and is narrower than the word now needs to be; a retirement
decided in one organization binds our projects in the others, and a catalog
namespace is already `<org>/<repo>` precisely because more than one org exists.

**Two consequences for this design:** `scope` cannot mean "this GitHub org", and
any citation of a decision has to carry *where it lives* — an ADR id alone is
ambiguous across organizations.

## The core idea: three recognizer tiers

A retirement carries **recognizers** — how to spot the dead idea. The tier
decides who can detect it, and that is the whole architecture.

| tier | recognizer | detected by | runs |
| --- | --- | --- | --- |
| 1 | **term** — a word, field, filename, command | deterministic match | engine, in CI |
| 2 | **shape** — a frontmatter key, a structural pattern | deterministic query | engine, in CI |
| 3 | **claim** — "documents asserting X, where we now hold Y" | **an agent reading** | skill, on demand |

Tiers 1–2 are cheap and narrow the field. **Tier 3 is the target**, and only a
reader can judge it — which is exactly why the sweep is a skill rather than a
rule. Every retirement declares at least one recognizer; most concept
retirements declare a `claim` and no `term` at all.

## What gets built

### 0. Two artifacts, not one — the decision stays home, the strategy travels

**The decision to retire and the strategy for retiring are different things**,
and separating them is what makes the rest work.

| | **the decision** | **the strategy** |
| --- | --- | --- |
| says | *why we changed our mind* | *what is dead, how to spot it, what replaced it* |
| type | `decision` (exists) | `retirement` (new) |
| lives | in the repo that decided — `.luma/records/decisions/` | in a catalog bundle |
| owner | whoever made the call | whoever has to comply |
| distribution | **none.** Cited by org/repo + id | **published, adopted, distributed** |
| lifecycle | **append-only.** Settled once, archived when superseded | **living.** Revised as new recognizers and exemptions are found |

**The lifecycle row is the argument.** A decision must never be edited — that is
what an append-only record is for. But a strategy *has* to be: you discover a
fourth way the old idea shows up, you find a document that legitimately argues
about the word, you narrow the scope after false positives. Today's session did
exactly that three times. Fusing them would mean editing a record every time a
sweep taught us something.

**Consequences worth planning for:**

- **One decision, many strategy revisions.** `retirement_id` is stable; the
  bundle version moves underneath it.
- **A consumer usually cannot read the decision.** `luma-foreman`'s ADR-0003
  lives in foreman's records; `luma-leader` would have to clone foreman to see
  it. So the citation is **a reference, not a link to follow**, and the
  retirement must carry a one-line `why` that stands alone.
- **Not every retirement has an ADR.** `SPEC.md` §13 is a spec section. A PR is
  sometimes the whole record. The citation field is free-form text.
- **A decision can retire several things at once**, and each gets its own
  retirement — ADR-0003 retired `outfit`, `projection` and `jobs` together.

### 1. Scope is decided first, from evidence, and it can be promoted

**`retire-a-concept` step 1 is settling how far this reaches**, before any
recognizer is written. Getting it wrong in either direction is expensive, and the
two directions fail differently.

| scope | means | distribution |
| --- | --- | --- |
| `project` | dies here; nothing else ever held the idea | **none.** Record stays in the project's own `.luma/records/` |
| `peer` | a few named projects hold it; the organization is not involved | published, but naming the projects it binds |
| `organization` | everything under one org | promoted to that org's catalog |
| `estate` | every project, every relevant org | promoted to the universal catalog |
| `unknown` | **nobody knows yet — probe before choosing** | deferred until probed |

**Promotion reuses the path that exists.** A retirement starting at `project` and
turning out to matter more widely is the same move as promoting a bundle out of a
project — `publish-to-the-catalog` already covers it. Scope only ever widens;
narrowing after distribution is not a supported move, because you cannot
un-adopt from everyone who took it.

#### `unknown` is a real state, and probing is cheap

**Do not guess the scope.** Run the tier-1 and tier-2 recognizers across
candidate repositories — mechanical, fast, no judgment — purely to answer *does
this idea appear anywhere else*. Scope then follows the evidence and is recorded
in `scope_evidence`, so a later reader can tell a measured scope from an assumed
one.

A probe cannot prove absence for a tier-3 claim. **It is enough to distinguish
"nowhere else" from "four repositories", which is the decision it has to serve.**

#### Conservative or blast: the posture follows the tier

**The default is conservative** — start at the narrowest scope the evidence
supports and promote when more arrives — because the two errors are not
symmetric, and today's session paid both:

- **Too wide** costs noise. `compliance` and `obligation` produced ~33 false
  positives here and were withdrawn the same day. **Noise teaches a reader to
  skim past notices**, which disables the check everywhere.
- **Too narrow** costs the idea surviving somewhere unseen and leaking back —
  invisible, and only discovered when it reappears as a fresh choice.

**So: blast a concept, be conservative with a common word.** A tier-3 claim
cannot be found later by grep and leaks back invisibly, so under-distributing it
is the worse error. A tier-1 term that is ordinary English is the reverse. That
is a rule, not a mood, and it falls straight out of the tier architecture.

*The one override:* if the idea is load-bearing and its failure is silent — a
published policy teaching a dead field, which is what F-004 was — go wide
regardless of tier. Reaching every adopter is the whole cost of being wrong.

#### Two origins, and they run in opposite directions

**`origin: project`** — it arose from the work. Scope is *measured* by probing,
starts narrow, and promotes upward as evidence arrives. This is the bottom-up
path and the one the previous section describes.

**`origin: organization`** — leadership decided it and it is handed **down**.
Scope is *asserted*, not probed, and `scope_evidence` records the mandate rather
than a measurement. A project cannot decline it: add-only overlays still let a
project widen its own net or exempt its own documents, but **narrowing a mandate
is not available**, which is the difference that makes it a mandate.

The estate already has the vocabulary — `obligation` on a catalog answers *must a
project adopt this*, and a handed-down retirement is that same act pointed at a
strategy rather than a bundle.

**This is what makes the sweep receipt earn its keep twice.** Bottom-up, it tells
a project whether it is current. Top-down, it tells the organization **which
projects have complied and which have not** — a question nobody can answer today.

#### `enforced` — an optional deadline, and it is a date

**A day, not a moment.** `retired_at` carries a time because minutes order it
against git history; nobody complies at 14:32. It is bare rather than
`enforced_at` to match the estate's existing split — `decided`, `audited` and
`archived` are dates, and `at` is reserved for timestamps.

**Absent means immediate**, which is the house default: foreman's ADR-0003 is
explicit that *"every rename is a clean break — no aliases, no deprecation
period."* A grace period is the deliberate opt-in, not the fallback.

**What the date changes is severity, not detection.** Before it, a hit is a
notice. After it, the same hit is a finding at the severity its carrying document
earns. That is one comparison, and it gives a mandate teeth without inventing an
enforcement mechanism.

**The known failure, named because the estate already found it:**
`tools-that-run-on-a-schedule` records that *"`by:` dates on obligations pass
silently until something happens to run."* An `enforced` date that nothing checks
on a schedule is decoration. MVP surfaces it whenever a sweep runs; making it
fire on its own is the same unsolved recurrence problem that backlog idea is
about, and this plan does not solve it either.

### 2. A `retirement` type — the strategy

New type, so purely **additive** — `change-a-shared-type` step 1 says adding is
not breaking, so this is one release rather than three.

```yaml
---
type: retirement
retirement_id: RET-0004  # 3–5 alpha, dash, digits. Stable across every revision
retired_at: 2026-08-26T12:48:49Z   # a time, not a date — minutes matter in git history
origin: organization     # project (arose from work) | organization (handed down)
enforced: 2026-09-15     # optional date. Absent = immediate. See §1
scope: estate            # project | peer | organization | estate | unknown — see §1
scope_evidence: "probed 7 repos; 4 carry it"   # measured, or the mandate that set it
decided_in: "lumastack/luma-knowledge-format#SPEC.md§13"   # where it lives, then which
was: "a document declares when it should be loaded, via `preload`"
now: "delivery is derived from `matches`; `matches: always` is the only route up front"
why: "consumption is the consumer's decision; the format defines what a Document is"
recognizers:
  - kind: term
    value: preload
  - kind: claim
    value: "any document telling an author to declare when their document loads"
except:
  - ".luma/records/"     # append-only; they say what was true when filed
lifecycle_status: stable # `archived` == released, per luma-foreman ADR-0005
---
```

**Four naming calls, since each one is load-bearing:**

- **`decided_in`, not `decided_by`.** This estate records actors as
  `human:fsmith`, `agent:opus-5`, `process:...`, so anything `_by` reads as an
  actor and would be misparsed by a reader on sight. `decided_in` names a place,
  and pairs with `retired_at` naming a time.
- **It carries org/repo *and* id** — `lumastack/luma-foreman#ADR-0003`. An ADR
  number alone is ambiguous once the estate spans organizations, which it does.
- **`retired_at`, a timestamp.** Renames landed minutes apart on 2026-08-26; a
  date cannot order them against the commits they describe.
- **`RET-` prefix**, per the estate convention that ids are 3–5 letters, a dash,
  then digits. *This exposes an existing violation: `audit-records` issues
  `F-001`, which is one letter. Worth a separate decision — do not fix it here.*

**`was` / `now` / `why` stay flat rather than nesting under `strategy:`.** Three
scalars do not earn a level, the format's other types are flat, and this repo
already carries `three-frontmatter-parsers` as a known cost — every nesting level
is paid three times. The type definition marks them as the distributed payload,
which is what a `strategy:` key would have communicated structurally.

**`recognizers` and `except` are the mutable parts** and the reason this is not a
record. Adding a recognizer after a sweep finds a new disguise is normal and
should cost a bundle patch, not an amended decision.

### 3. A `retirement-records` bundle in `luma-catalog`

Named for its siblings — `audit-records`, `decision-records`. Distribution needs
**nothing new**: a project adopts it, and `luma-foreman apply` already writes one
skill per `workflow` (`src/foreman/apply.py`, "skills — one per `workflow`"), so
the workflows below become skills automatically. `bundle outdated` already
answers *have the retirements moved*.

| file | what |
| --- | --- |
| `_types/retirement.md` | the type above |
| `policy/retiring-a-concept.md` | what may be retired, the tiers, the cost of retiring ordinary English, when an entry is released |
| `policy/what-we-retired.md` | **`matches: always`** — the distributed index |
| `workflows/retire-a-concept.md` | **two steps in two repos**: record the decision at home, then publish the strategy. Run by the project that moved |
| `workflows/sweep-retirements.md` | tier-3 review. Run by every other project |
| `workflows/release-a-retirement.md` | apply ADR-0005's test and archive an entry |
| `templates/retirement.md` | copy-blocks, per house convention |

### 4. The distributed index — the half that beats priors

`policy/what-we-retired.md` declares `matches: always` and carries **one line per
retirement**: `was → now`. Nothing else. It is the only thing in front of an
agent *before* it writes, which is the only moment a reinvention can be
prevented rather than reported.

**It must be generated from the records, not hand-maintained** — hand-copying is
precisely how §13 went stale here. Generate it on the publish side when
retirements change, so adopters receive it assembled.

Note this would be the catalog's **first** `matches: always` document; today
nothing declares it across nineteen bundles. That is a deliberate, arguable cost
and belongs in the bundle's `## Version` reasoning.

### 5. Sweeping, and where findings go

`sweep-retirements` reads adopted retirements and reviews this repository for
each one — tier 1–2 mechanically, **tier 3 by reading**. Findings go into
`audit-records`: a sweep is an audit with a pre-supplied lens, and that machinery
already gives severity, one file per finding, a response, and a verification.
No parallel reporting mechanism.

**Severity derives from the carrying document's `type`**, which is computable
rather than a judgment call:

| retired thing appears in | severity |
| --- | --- |
| `policy` or `workflow` — they bind | **high** |
| `document` — drafts argue about words | **medium/low**, a notice |
| a `## Version` history, or anything under `.luma/records/` | **exempt** |

F-004 was rated high by a human precisely because it was a policy. It should not
have needed one.

### 6. Tracking — the sweep receipt

The genuinely new tracking need: *have I swept, and against what version.*
Adoption tells you that you hold the list, not that you acted on it.

`.luma/records/retirements/swept.toml`, mirroring `adopted.toml`'s receipt shape:

```toml
[sweep]
against  = "lumastack/luma-catalog/retirement-records"
version  = "0.3.0"
swept    = 2026-08-27
by       = "agent:claude-opus-5"
audit    = "2026-08-27-<sha>"   # when it filed one
```

A later `inspect` rule compares adopted version against last-swept version and
reports *adopted 0.5.0, last swept 0.3.0 — four retirements never swept here*.

### 7. Decentralized evolution — a strategy learns from the projects running it

**A distributed strategy meets problems its author never saw.** A project adopts
`RET-0004`, sweeps, and finds the recognizer over-reports against its own prose,
or finds a disguise nobody anticipated. It needs relief *now*, and the estate
needs the learning. Waiting for an upstream release to do either is what makes
people quietly stop running the check.

**Two mechanisms, both of which already exist:**

**Local overlay, add-only.** A project may add `except` entries and add
recognizers to an adopted strategy, in its own `.luma/config/`. It may **not
remove** anything upstream declared. Add-only is the whole safety property: a
project can narrow its own noise and widen its own net, and cannot silently
weaken a retirement for itself. This is precisely what happened by hand today —
`compliance` and `obligation` were removed locally after false positives — except
that learning had nowhere to go.

**Contribution back is an audit finding.** A sweep that concludes *the strategy
is wrong* files a finding against the strategy rather than against the
repository. The respondent is whoever publishes it; the fix is a bundle patch;
the verification closes it. `audit-records` is already exactly this shape — one
party finds, another is accountable, the first confirms — so contribution needs
no new mechanism, only the convention that a strategy is a legitimate subject.

**The risk this creates, named rather than designed around:** overlays are
divergence. If every project accumulates them, the estate stops sharing one
strategy and nobody can see it happening. Two cheap mitigations, both MVP:
overlays are committed like everything else, and **the sweep reports its overlay
count** so drift is visible rather than silent. **A long-lived overlay is
evidence the upstream strategy is wrong** — that is the signal worth surfacing,
and it is free.

*Deliberately not in MVP:* reconciling an overlay when upstream absorbs it,
promoting an overlay to upstream automatically, and any notion of a project
forking a strategy outright.

## Where each piece lands

By the estate's rule — **where does it run**, not what is it about.

| piece | repository |
| --- | --- |
| **strategies** — the `retirement` type, bundle, policies, workflows, index | `luma-catalog` |
| **decisions to retire** | wherever the call was made — foreman's ADRs, `SPEC.md` §13, luma-leader's `DECISIONS.md`. **Never moved, only cited** |
| project-side detection + sweep receipt rule | `luma-foreman` |
| publish-side detection | `luma-catalog-curator` |
| this reasoning, and the decision to adopt this framework | `luma-leader` |

**No new engine, deliberately.** The estate's naming rule doubles as a
junk-drawer detector; this is one job — *publish a retirement, distribute it, check
it* — and each third already has a home.

## Build order

**Phase 1 — the MVP, and it needs no engine code.** The type, the bundle, both
policies, the three workflows, the generated index. Publish, then adopt into
`luma-leader` and `luma-catalog`. At the end of this phase concept retirement
works end to end: **scope settled first**, authored, distributed, swept by skill,
findings filed as an audit. Word matching is *not* what makes it work.

*In Phase 1 the probe is a person or an agent running the recognizers by hand
across candidate repositories.* `scope`, `origin` and `enforced` are recorded
honestly from the start — they cost nothing to write and cannot be backfilled
later — but nothing enforces `enforced` until Phase 2 has a receipt to compare
against.

**Phase 2 — tracking.** `swept.toml`, and a `luma-foreman inspect` rule that
reads adopted retirements (replacing the hand-copied `[[retired]]` config) and
reports unswept versions.

Migrating this repo's four `[[retired]]` entries is the worked example of the
split, and every decision already exists somewhere — none needs writing:

| strategy to author | decision it cites, already written |
| --- | --- |
| `outfit`, `projection` | `luma-foreman` ADR-0003, ADR-0005 |
| `preload`, `applies_to` | `luma-knowledge-format` `SPEC.md` §13 |
| the two concept retirements above | `luma-leader` `DECISIONS.md`, foreman ADR-0003 |

The `except` lists earned this week — records, `loading-mechanisms.md`,
`DECISIONS.md`, and the four false-positive findings — carry over verbatim as
`recognizers`/`except` on the strategies, and **none of them touches a
decision**, which is the split doing its job.

**Phase 3 — the publish side.** `luma-catalog-curator check` reads retirements
so a bundle cannot be published teaching a retired idea. This is where F-004
would have been caught before it reached any adopter.

Phase 1 is independently useful and reversible; nothing in it obliges 2 or 3.

## Critical files

- `luma-catalog/catalog/bundles/retirement-records/**` — all new
- `luma-catalog/catalog/bundles/luma-types/BUNDLE.md` — only if the type lands
  there instead; prefer the new bundle so `luma-types` stays about core shapes
- `luma-foreman/src/foreman/inspect/rules/vocabulary.py` — the working model to
  extend, not replace: it already has `except`, `## Version` blanking, and
  notice-not-finding semantics
- `luma-foreman/src/foreman/apply.py` — no change needed for skills; read only
  to confirm the `matches: always` path for the index
- `luma-leader/.luma/config/luma-foreman.toml` — its four `[[retired]]` entries
  become the first retirement records in Phase 2
- `luma-leader/docs/retiring-a-concept.md` — the existing draft; update it to
  point at this plan and record which of its four open questions are answered

## Verification

1. **The type is valid.** `luma-catalog-curator check` in `luma-catalog`, clean,
   and `check --against origin/main` for the version bump.
2. **Distribution works with no new mechanism.** In a scratch repo:
   `luma-foreman get lumastack/luma-catalog/retirement-records` then
   `luma-foreman apply` — confirm `.claude/skills/sweep-retirements/` appears and
   `CLAUDE.md` lists the bundle.
3. **Distribution reaches the agent.** Confirm `apply --explain` classes `what-we-retired.md` as
   always-on, and that it appears in the `always-on` count rather than
   `advertised` — today that count is 0 here.
4. **Tier 3 actually catches a concept.** The regression test is
   `docs/catalog-namespaces.md` in `luma-leader`: it asserted *a catalog declares
   its namespace* with no dead word in it. Author `R-000n` for that retirement,
   run `sweep-retirements`, and confirm it reports that document. **A sweep that
   finds only `preload` has failed this test.**
5. **Findings are well-formed.** The filed audit passes as `audit-records`
   documents — one file per finding, `severity` present, `luma-foreman inspect`
   clean afterwards.
6. **Tracking is honest.** Bump the bundle, re-adopt without sweeping, confirm
   the Phase 2 rule reports the gap rather than staying green.
7. **Scope is recorded, not assumed.** Author one retirement at each of
   `project`, `organization` and `unknown`, and confirm `scope_evidence`
   distinguishes a probed scope from a mandated one. A retirement left at
   `unknown` must not distribute.
8. **A mandate cannot be narrowed.** On an `origin: organization` retirement,
   confirm a local overlay can add an `except` for the project's own documents
   but cannot remove an upstream recognizer.
9. **`enforced` changes severity, not detection.** With a future date, a hit
   reports as a notice; with a past date, the same hit reports as a finding at
   the severity its carrying document earns.
10. **The split holds under pressure.** Add a recognizer to an existing strategy
   after a sweep finds a new disguise, and confirm it is a bundle patch that
   touches **no decision record**. If revising a strategy ever requires amending
   a record, the two artifacts have fused and the design has failed.

## What this does not solve — state it in the bundle, do not paper over it

- **A grep cannot tell a revival from ordinary English.** `compliance` and
  `obligation` were tried in this repo and removed the same day for ~33 false
  positives; `obligation` also names a live concept here. Retiring a common word
  has a cost, and `scope` is the pressure valve.
- **Tier 3 is a judgment.** An agent reading for a claim will miss some and
  over-report others. It is better than nothing and worse than a proof.
- **A concept renamed with no vocabulary change and no claim recorded** is
  invisible to all three tiers. The defence is authoring discipline at retirement
  time, not detection.
- **The rule sees only tracked files** (`git ls-files`), so a document written
  and not yet committed is invisible — exactly when a reinvention has just
  happened. A pre-commit hook closes it; nothing does today.
- **A repository that adopted nothing gets nothing**, which describes
  `luma-catalog` itself. Phase 3 matters for that reason.
- **Two estate documents fall out of date the moment this lands, and neither is
  in scope here.** `policy/the-estate` enumerates six repositories under one
  organization, which is narrower than the definition this plan needs.
  `audit-records` issues `F-001`, which breaks the 3–5-letter id convention.
  Both want their own decision; neither should be fixed in passing.
- **The join between the two artifacts is a human step.** A decision can be made
  and the strategy never authored, and nothing detects that — the estate would
  look clean while the old idea travels freely. `retire-a-concept` makes it one
  procedure across two repos, but procedure is not enforcement. Closing it means
  a check on the deciding side, and no phase here proposes one.
