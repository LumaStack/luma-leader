---
type: document
title: Adoption use cases
description: Everything somebody will try to do with bundles and catalogs, scored against the designs on the table — which option wins each axis, which lose, and what the winners still cost.
stage: draft
created: { by: human:benlinton, at: 2026-08-23T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-23T00:00:00Z }
---

# Adoption use cases

**Draft. Nothing here is settled.** Fifth companion to
[bundle-dependencies.md](bundle-dependencies.md),
[bundle-versioning.md](bundle-versioning.md),
[shared-types.md](shared-types.md), [curator.md](curator.md) and
[catalog-namespaces.md](catalog-namespaces.md). Kept out of
[DECISIONS.md](DECISIONS.md) on purpose.

**Those design mechanisms. None of them starts from what somebody will try
to do.** This one enumerates that first and scores the mechanisms against it, so
an option wins or loses on cases rather than on argument. Where a verdict is
reached it is stated, with what the winner still costs.

## Four failures, and only one of them is about getting content

Every design below is trading between these. Naming them separately matters
because they are usually all called *context problems* and they have almost
nothing in common.

| | what it is | who notices | what it costs |
| --- | --- | --- | --- |
| **Contradiction** | two loaded documents say opposite things | **nobody** — both are plausible, the agent picks one | an agent confidently follows a rule the project abandoned |
| **Saturation** | more is loaded than fits, so something is dropped | **nobody** — dropping is silent | a rule that was in force was never read |
| **Silent absence** | a document that should have loaded did not | **nobody** — no rule was consulted, so none was missed | a decision made without the rule that governed it |
| **Silent presence** | it loaded, and was not applied when it mattered | **nobody, and every check says green** | the same cost as absence, from the opposite cause |

**All four are silent, which is the whole difficulty.** Nothing errors, nothing
crashes, and the output looks like a normal answer.

**And here is the structural point this document exists to make.** Contradiction,
saturation and silent presence happen entirely **after** content arrives. Only
silent absence is even partly a question of what you acquired. Roughly seven
hundred lines of design in `docs/` — dependencies and versioning — address
acquisition. Nothing addresses the other three.

**Silent presence has its own document.** It grew past a section, and it is a
different kind of problem from the other three — nothing on the axes below
touches it, because it is not about what you acquired or what you loaded. See
[silent-presence.md](silent-presence.md) for why no loading discipline fixes it,
the enforcement ladder that follows, and how a snapshot taken when a failure is
detected tells four causes apart.

## The use cases

Numbered so the tables below can point at them. One line each; the argument
comes later.

### A. Getting one bundle

| | |
| --- | --- |
| **UC1** | Adopt one named bundle and nothing else |
| **UC2** | Adopt at whatever version the catalog currently holds |
| **UC3** | Adopt a specific older version deliberately |
| **UC4** | Adopt everything a starter set names, for a new project |
| **UC5** | Adopt from a local checkout of a bundle still being written |
| **UC6** | Adopt a bundle that has since been retired upstream |
| **UC7** | Re-adopt the same bundle later to pick up changes |

### B. More than one source

| | |
| --- | --- |
| **UC8** | An organization catalog that sits below the universal one |
| **UC9** | Two catalogs with entirely disjoint content |
| **UC10** | Two catalogs sharing a namespace, with different bundles in it |
| **UC11** | Two catalogs sharing a namespace **and** a bundle name, different content |
| **UC12** | A catalog and a fork of it, wanted side by side for comparison |
| **UC13** | **Two catalogs that solve the same problem differently** — one says merge commits, one says rebase |
| **UC14** | **Two catalogs where one's guidance is the other's anti-pattern**, and both are adopted |

### C. Versions in tension

| | |
| --- | --- |
| **UC15** | Everything current except one bundle held back for review |
| **UC16** | Two bundles needing different majors of something they share |
| **UC17** | Bundles from one catalog adopted weeks apart, at different commits |
| **UC18** | Some of catalog A at one snapshot and some of A's sibling at another |
| **UC19** | Two bundles vendoring different versions of one shared type |
| **UC20** | An out-of-bundle document with two candidate contracts claiming it |
| **UC21** | A breaking type change, migrating records already written |

### D. What ends up in context

| | |
| --- | --- |
| **UC22** | Adopt a bundle for one workflow, and not want its policies ambient |
| **UC23** | Turn off a single over-eager `matches: always` document |
| **UC24** | Turn *on* something the author marked optional |
| **UC25** | Load a bundle only during a release, an incident, an audit |
| **UC26** | The mandatory set across eight adopted bundles exceeds the budget |
| **UC27** | A bundle is adopted and reaches no agent at all |
| **UC37** | Two kinds of work in one repository, each wanting different knowledge |
| **UC38** | Pick what loads at session start, because nothing can unload mid-session |

### E. Changing and leaving

| | |
| --- | --- |
| **UC28** | Edit an adopted bundle because it is nearly right |
| **UC29** | Fork it into your own namespace instead |
| **UC30** | Send a change back to the catalog it came from |
| **UC31** | Stop using a bundle, and have what came with it leave too |
| **UC32** | Promote a project-local bundle into a catalog |

### F. Knowing where you stand

| | |
| --- | --- |
| **UC33** | Is my copy still what I adopted? |
| **UC34** | Is there a newer version of anything I hold? |
| **UC35** | Which rules are actually in force in this repository? |
| **UC36** | What did I adopt that nothing reads? |
| **UC39** | **Is this rule actually being followed**, rather than merely present? |

**Most of these behave identically under every design on the table.** UC1, UC2,
UC5, UC7, UC28–UC30, UC33 and UC36 are already answered and do not discriminate.
The cases that decide anything are **UC3, UC11–UC18, UC22–UC27 and UC31** — and
notice that the largest cluster is section D, where no design exists.

## The axes

Six, grouped by whether they govern content arriving or content behaving.

**Acquisition**

1. **What pins a project's content** — per-bundle version · catalog snapshot ·
   current-at-adopt with the commit recorded afterwards
2. **How many sources** — one catalog · a linear `upstream` chain · many
   independent catalogs
3. **Do bundles depend on bundles** — no · yes, flat, one version

**Disposition**

4. **Who owns the namespace** — the catalog declares · the consumer aliases ·
   catalog default with a consumer override
5. **Contradiction handling** — nothing · structural prevention · imperfect
   detection · cost reporting
6. **Who controls what loads up front** — author only · adopter sets a bundle default ·
   adopter overrides per document · conditional and routed

---

## Axis 1 — What pins a project's content

| | UC3 hold one back | UC15 current except one | UC16 shared-major clash | UC17 mixed commits | UC18 two snapshots |
| --- | --- | --- | --- | --- | --- |
| **per-bundle version** | yes | yes | fails, and names who asked | visible | visibly unsolved |
| **catalog snapshot** | whole catalog only | **no — too coarse** | cannot occur | cannot occur | **looks solved, is not** |
| **current, commit recorded** | by not re-adopting | by not re-adopting | cannot occur | visible | visibly unsolved |

**Winner: current-at-adopt with the commit recorded.** It is what is built, and
the use cases say it is enough for now.

**Why, and this is the part that surprised me: vendoring already bought
reproducibility, so pinning buys far less here than it does in a package
manager.** The content is committed. A fresh clone reproduces exactly, offline,
with no manifest consulted. Everything a lockfile exists to provide is already
provided by the copy being in git — which leaves pinning with only the
change-control job, and *don't re-adopt* already does that job for a project
that wants it.

**Loser: the catalog snapshot.** It is coarse where UC15 needs precision, it
makes upgrades non-local, it needs snapshots that nothing publishes, and its own
proposal concedes it is *"worth nothing until [dependencies are] adopted."*
Worst, on UC18 it **manufactures confidence** — a coordinate across two catalogs
looks like a guarantee and is not, where per-bundle versions at least look as
unsolved as they are.

**But keep its receipt, which is what happens to be built.** `adopted.toml`
already records the source commit per entry. That is the snapshot proposal's
data with none of its coupling: you can answer *what was this taken alongside*
without the catalog acquiring states you must resolve against.

**Cost of the winner, stated plainly.** Nothing tells you a newer version exists
(UC34) — that needs the catalog, and inspection runs offline. There is no claim
that two bundles were verified against each other. And a compliance team's *do
not even offer me a newer one* has no expression, only a convention.

**Per-bundle pinning is the growth path, not a rejected option.** It becomes
necessary the day bundles depend on bundles, and not before.

## Axis 2 — How many sources

**Winner: the linear `upstream` chain, and it is already settled.** A project
names its organization's catalog and gets the chain; `requires` merges
most-restrictive-wins, `tags` union, `starters` inherit explicitly. UC8 and UC9
are answered.

**The verdict that matters is about the third option, and it is not a
prohibition.** Many independent catalogs is not *allowed* by the design — and it
is *reachable* regardless, because a person can point an adopt command at any
URL. UC12, UC13 and UC14 all arrive this way.

**So the honest position: possible, and unblessed.** A configured chain carries
guarantees — curation, publication checks, obligation resolution. An ad-hoc
adoption from a stranger's catalog carries none of them, and **the failure is
treating the second as though it were the first.** Nothing currently marks the
difference; `adopted.toml` records a source and says nothing about whether it
was configured or improvised.

**Cost of the winner.** `upstream` is single-valued with a known re-open
trigger, and an organization drawing on both a universal and an industry catalog
would hit it. And the chain gives no arbitration for the cases that arrive
outside it, which is the next axis but one.

## Axis 3 — Do bundles depend on bundles

**Winner: no, for now — and the count is lopsided enough to be worth stating.**
Of thirty-six use cases, dependencies serve exactly one that is not of their own
making: two bundles duplicating a policy that drifts. UC16 and UC31 are cases
that **only exist because dependencies exist**.

**The duplication argument is still the strong one**, and it is not dismissed
here: for prose the alternative to a dependency is a second copy, and two copies
of a policy drift and then contradict with nothing recording which is current.
That is a genuine failure and it is the contradiction failure from the top of
this document.

**But there is a competing remedy already on record**, from the `luma/catalog`
type: *"A coupling you cannot express is usually a Bundle boundary in the wrong
place."* If adopting A without B leaves somebody with rules and no procedure,
the split was wrong — and merging two bundles costs nothing while nobody has
adopted them.

**What would settle it is evidence rather than argument: name two bundles in the
catalog today that duplicate the same policy.** If the answer is none, the
duplication problem is anticipated rather than observed. If the answer is three,
that is the trigger.

**Cost of the winner.** Duplication when it comes will drift silently, and a
bundle that genuinely needs another can only say so in prose that nothing acts
on.

## Axis 4 — Who owns the namespace

| | UC10 shared ns, disjoint | UC11 same address, different content | UC12 fork side by side | UC7 re-adopt |
| --- | --- | --- | --- | --- |
| **catalog declares, collision fails** | fine if checked per address | **fails — correct** | impossible | **breaks unless identity is the source** |
| **consumer aliases** | fine | silently aliased — **bad** | easy | fine |
| **catalog default + written override** | fine | **fails unless declared** | possible, and visible | fine |

**Winner: the catalog declares a default, the consumer may override it, and the
override is written into committed config.** The model is worked out in
[catalog-namespaces.md](catalog-namespaces.md); the verdict and its reasoning are here.

**Because a self-declared namespace is a claim, not an identity.** Nothing
arbitrates it, no registry issues one, and two organizations may both publish
`acme/`. So `namespace:` can only honestly be *a suggested local name*. The
thing that is actually unique is the source URL, and it is already recorded.

**The mechanism that makes it work: collision is detected against the source,
not against the name.** Same address and same source is an upgrade; same address
and a different source is a collision. Without that distinction UC7 and UC11 are
indistinguishable, which is the flaw in the pure catalog-declares option.

**It is also the less registry-shaped answer**, which matters given *the catalog
is a catalog, not a registry*. Arbitrating global names is precisely what a
registry does.

**Cost of the winner, and it is real.** An alias override makes the forbidden
thing possible: hold upstream and a fork side by side under two names, and the
project holds two contradictory policies — the exact thing the one-version rule
exists to prevent. **The mitigation is visibility, not prevention.** That is the
same shape the estate has already chosen three times — a narrow constraint must
state a reason, an exemption is a sentence rather than a pattern — and it should
be chosen consciously here rather than by accident.

**Check per address, never per catalog.** Two catalogs sharing a namespace with
disjoint bundle names collide on nothing, and a rule phrased about catalogs
would fail UC10 for no reason.

**The override half is deferred, and only the failure ships.** An override is
needed once a collision happens; a collision needs two sources sharing a
namespace; there is one catalog. **It is an escape hatch for a situation that
cannot currently arise**, and the error message is a better instrument for
learning what people actually want than a guess made now. The bar for building
it is two namespaces that are genuinely different and non-competing — an
unlikely shape, and worth building only while it stays cheap.

## Axis 5 — Contradiction handling

**This axis has no winner, because the strongest option is not available.**

**Structural prevention is impossible in general.** To refuse two bundles that
contradict, something must know that two prose documents contradict, and nothing
can. What *is* structurally preventable is already prevented: two versions of a
bundle (the filesystem holds one slot) and two bundles at one address (axis 4).

**Cost reporting wins the saturation half outright, and it is cheap.**
`bundle-dependencies` already specifies it — *"Adopting A also brings B and C;
four documents load up front"* — and `matches` is declarative, so the number
is computable before anything loads. **No package manager reports this because
none has a budget to spend.** This is the single highest-value unbuilt item in
this document, and it addresses UC26.

**Detection wins the contradiction half by default, and must be honest about
being partial.** A checker that finds the obvious cases and reports clean
otherwise is worth having, and it carries exactly the danger
`an-index-of-what-exists` names: *nothing applies here* and *I did not look* are
indistinguishable from the inside.

### The gap nobody has drafted, and UC13 and UC14 are it

Two catalogs, both adopted, one says merge commits and one says rebase.
**Neither is wrong. Both are in force. The agent picks.**

**And this is believed to be solved when it is not.** *Obligations resolve
most-restrictive-wins* says outright: *"It also settles cross-catalog conflict
with no additional mechanism."* That is true, and it is narrower than it reads —
it settles **obligation** conflict, which is *how strongly must I adopt this*. It
says nothing whatever about **content** conflict, which is *these two things I
adopted disagree*. `bundle-dependencies` does flag the second, in one bullet
under *Open*, and calls the answer *"arbitration between catalogs, which does not
exist."*

**One candidate worth exploring, and it is new here rather than drawn from an
existing draft.** A bundle could declare what it **governs** — a value from a
published vocabulary, the same shape `tags` already uses for projects. Two
adopted bundles both claiming `git-workflow` is then a **declarative** collision:
mechanically detectable, no prose comparison, no undecidability. It does not ask
*do these contradict*; it asks *do both claim authority over the same thing*,
which is a question a manifest can answer.

Unvalidated, and it inherits every failure mode of a controlled vocabulary —
`git-workflow` versus `git-workflows` silently fails to collide, which is the
same hazard `tags` already carries and answers by publishing the vocabulary.

## Axis 6 — Who controls what loads up front

| | UC22 not ambient | UC23 one doc off | UC24 one doc on | UC25 only during a release | UC26 budget |
| --- | --- | --- | --- | --- | --- |
| **author only** (built) | no | no | no | no | no |
| **+ adopter bundle default** | **yes** | no | no | no | **most of it** |
| **+ per-document override** | yes | yes | already allowed | no | yes |
| **conditional / routed** | yes | yes | yes | **yes** | yes |

**Winner: the adopter's bundle-level default, and it should be built next.** It
is already settled — *the adopter always wins; the author only suggests* — and
unbuilt, and it answers the two cases that carry the most risk with the least
machinery.

**Because it puts the decision where the knowledge is.** Whether a bundle is
relevant to this project's work is a fact about the adopter, which the author
cannot know. Whether a document is the spine of a bundle is a fact about the
bundle, which the adopter cannot know. That split is already reasoned; nothing
implements it.

**Cost of the winner.** Coarse — all or nothing per bundle. It does nothing for
UC23, where one document in a bundle you otherwise want is the problem.

**Per-document override loses on the reason it was already deferred**: it reaches
into another bundle by path, the author renames a file, and the override silently
stops applying. UC24 needs it least of all — raising what an adopter loads is
already permitted.

**Conditional loading wins every column and is the only one nothing can build.**
No harness has a *conditions changed, load these now* hook. Projection-time
selection captures most of the value; runtime swapping has no mechanism anywhere.

**Which makes the session boundary the only reload point that exists** — and
that is UC38, and it turns the expensive half of this axis into something
buildable. If nothing can change what is loaded mid-session, selection has to
happen at session start, and selection at session start needs no harness
capability that does not already exist. A named mode, chosen when a session
begins, with the adapters written to match. It answers UC37 and gives up on
mid-flight conditions, which nothing can deliver anyway. See the closing section
of [catalog-namespaces.md](catalog-namespaces.md), where the idea came from.

> *Corrected 2026-08-23, later the same day.* **The claim above is true about
> unloading and the inference drawn from it is too broad.** Nothing can *drop*
> what is loaded, which is what the sentence says. But a `PreToolUse` hook can
> put text in front of an agent **at the moment it acts** — verified rather than
> assumed, since `luma-foreman`'s permission gate already returns a
> `permissionDecisionReason` that reaches the model.
>
> **You cannot unload, but you can load late, and loading late is most of what
> conditional loading wanted.** The verdict on this axis is unchanged — the
> adopter's bundle-level default is still what to build first — but *conditional
> loading is unbuildable* was wrong, and it was load-bearing for the paragraph
> above. Session modes remain worth having; they are no longer the only option.
>
> **And unloading is not impossible either — it is just not a hook.**
> `/compact` and `/clear` drop what is loaded. What survives a compaction is
> defined: the project-root `CLAUDE.md` is re-read from disk and re-injected,
> and path-scoped rules reload when a matching file is next read. So the
> question was never *how do I unload*; it is **what do I make survive**, which
> is answerable today by choosing where content lives.
>
> Hooks are harness-specific with no standard, which is a real cost and not a
> reason to decline. The design that follows from this is recorded internally
> while its supporting research is.

---

## The winning stack, and where it leaves each failure

| axis | winner | status |
| --- | --- | --- |
| pinning | current at adopt, commit recorded | **built** |
| sources | linear `upstream` chain; ad-hoc possible and unblessed | chain settled, unbuilt |
| dependencies | none, until duplication is observed | settled |
| namespace | catalog default, identity is the source; **override deferred** | **partly built** — default only |
| contradiction | cost reporting + partial detection | unbuilt |
| what loads up front | adopter's bundle-level default | settled, unbuilt |

| failure | how well the stack answers it |
| --- | --- |
| **Silent absence** | **best covered.** The index pattern is built and running |
| **Saturation** | **answerable and unanswered.** Cost reporting and an adopter-set default for what loads are both specified and neither exists |
| **Contradiction** | **open.** Partly prevented by structure, undrafted for UC13 and UC14 |
| **Silent presence** | **outside the stack entirely.** No axis here touches it — see [silent-presence.md](silent-presence.md) |

## What this suggests, in order

1. **Report context cost at adopt time.** Specified, cheap, addresses the
   failure with no other mitigation, and `matches` is already parsed.
2. **Build an adopter-set default for what loads.** Settled, unbuilt, and the
   adopter currently has
   no say at all in what loads.
3. **Make identity the source, not the name**, before two catalogs exist and the
   distinction has to be retrofitted. Failure only; no override.
4. **Surface mixed commits from one source** — the data is in `adopted.toml`,
   nothing reads it, and *visible* was the whole requirement.
5. **Decide whether ad-hoc adoption is marked as such** in `adopted.toml`.
6. **Take UC13 and UC14 seriously as a design problem**, or record deliberately
   that holding two catalogs that speak to one subject is unsupported.
7. **Start keeping what the permission gate already knows.** It is the one
   place a violation is caught at the moment of action and it records nothing —
   see [silent-presence.md](silent-presence.md), where the rest of that argument
   went.

## Open

- **Is a `governs` declaration worth it**, or does it become a vocabulary nobody
  maintains? It is the only mechanical handle on UC13 anybody has proposed.
- **Should ad-hoc and configured adoption be distinguishable** after the fact,
  and what would consume the distinction.
- **UC20 has no owner.** An out-of-bundle document with two candidate contracts
  is the one place the bundle-as-resolution-scope rule does not reach, and
  `.luma/_types/` is described but nothing writes or checks it.
- **UC6 — a bundle retired upstream.** Nothing detects it, and the project holds
  content its publisher has withdrawn.
- **UC31 — un-adoption.** No command removes a bundle, and under dependencies it
  would need the asked-for versus required-by flag that `adopted.toml` does not
  carry.
- **Whether the use-case list is complete.** It was written in one sitting from
  a conversation, and the D section grew fastest, which usually means it is the
  one still missing entries. Silent presence was found after the list was
  called done, which is evidence for that.
