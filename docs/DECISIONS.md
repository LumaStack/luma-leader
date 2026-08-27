# Decisions

Settled positions and the reasoning that settled them. Recorded so they are not re-argued from scratch. A path not taken is recorded as deferred with a re-open trigger, never as rejected.

## Generated artifacts stay committed, for now

**Not settled. Current practice, and the reasoning for it, recorded 2026-08-26
so that it is not re-derived — but nobody has decided it.**

*It was first written here as settled, which it was not: it came from an
instinct explicitly hedged in the same sentence that raised it. **A position
recorded as settled acquires an authority nobody granted it**, and this file is
read as the record of what was decided. Corrected the same day. What follows is
an argument worth keeping, not a decision to cite.*

**`CLAUDE.md`, `.claude/skills/` and `.luma/bundles/routing.toml` are committed**,
though every one of them is written by `luma-foreman apply` and could be
rebuilt in seconds.

**The argument is asymmetry rather than conviction.** Removing them from the
index later costs one commit. Reconstructing a year of how delivery actually
behaved — what an agent was shown, and when it changed — costs everything,
because it was never recorded anywhere else. **Cheap to undo, impossible to
recover, so lean toward capture.**

**Two of them have a second reason that does not depend on the argument above.**
`routing.toml` is read at runtime by the permission gate, and a missing gate
fails *open* — a rule that declares `block` and silently stops blocking is the
one failure a gate cannot have. And a committed copy is what lets a bare clone
with no tooling reproduce the project, which is the property that separates
adoption from a package cache.

**The cost is real and is accepted: they conflict on every parallel branch.**
Two branches that both adopted something will both have rewritten all three.
**The conflict is normal and the resolution is always to re-run `apply`**,
never to hand-merge — a hand-resolved generated file is wrong in a way nothing
reports, and the next run discards it. That rule is published in
`adopt-knowledge` so it reaches adopters rather than living here.

**What would settle it, either way:** a deliberate call on whether the history
of a generated file is worth the conflicts, made when somebody has felt both.
Until then this describes what the repositories do and why, and a reader is free
to disagree with it without arguing against a decision.

**What would force the question:** the conflicts stop being an occasional
nuisance and start costing real time, **or** somebody hand-resolves one and it
reaches `main` — the failure this practice is betting will not happen.
`.claude/skills/` is the first to go if so: it serves one harness and nothing
reads it at runtime.

## A document says what makes it surface, not what it governs

**Settled 2026-08-26.**

**The field is `matches`.** It was `applies_to`, and the name obliged an author
to write a false sentence: `applies_to: everything` claims a rule governs
everything, and none does. **What a rule governs is stated in its body**, where
no frontmatter value reaches — `writing-style` governs prose whatever its
frontmatter says. The field says what makes a document *surface*, which is a
smaller claim and a true one.

**The vocabulary had outgrown the old name independently.** `applies_to` was
chosen on the convention that in policy languages it means enforcement scope —
*this rule applies to these targets*. `path` is a target; **`event` is a
moment**, and nothing about a moment is a resource a rule scopes over. The
convention stopped applying the day `event` joined the list, and nobody noticed
because the name still read plausibly.

**`matches` was chosen on one test: does it read as a sentence in all three
forms the field takes?** *Matches `git commit`. Matches always. Matches
nothing.* Every alternative broke on at least one — `matches_on` and `fires_on`
turn clumsy at `always`; `triggered_by` is the most internally consistent, since
the entries are called triggers everywhere, and fails at *triggered by always*;
`when` collapses at both scalars.

**Deferred, not rejected:** renaming the entries from *triggers* to *matchers*,
so the field and its contents share one word. **Re-open trigger:** the two-word
seam causes a real misreading, rather than being noticed and shrugged at.

## The expensive delivery mode is asked for, never fallen into

**Settled 2026-08-26.**

**A document that says nothing about what surfaces it is available on request.**
It used to be loaded into every session — so forgetting a field bought the most
expensive delivery mode in the system, silently.

**The lazy path was the costly path**, and it broke a rule this estate had
already written down: *failing by loading nothing is recoverable, failing by
loading everything is a token bomb.* The default violated the direction the
same design chose deliberately elsewhere.

**The argument for the old default was that falling in makes the cost visible as
a gap rather than as a decision somebody made.** That is weaker than it sounds,
because the visibility depends on somebody running `inspect` and reading a
low-severity finding. **Requiring `matches: always` makes the expensive outcome
impossible by accident, which needs no tooling at all.** Structural beats
diagnostic.

**It cost nothing to change.** All thirty-two policies in the universal catalog
already stated what surfaces them, so the reversal changed the behaviour of zero
published documents. **That window closes on the first outside adopter.**

**One casualty was not predicted.** `an-index-of-what-exists` justified its own
permanent presence by *having no trigger* — the very property that, after the
reversal, means nothing surfaces it. The one document that must always be
present would have become the one nobody sees. **A rule stated in terms of a
default breaks when the default moves.**

**Re-open trigger:** a rule genuinely governs everything, its author writes
`matches: always`, and the resulting cost is disputed rather than accepted.

## `always` is a value, never a member of the trigger vocabulary

**Settled 2026-08-26.**

**`matches: always`, and never an entry inside the list.** Two reasons, and the
second decides it.

**Every trigger kind narrows; `always` refuses to.** It was never a peer of
`path` and `command`, and giving it the same shape claimed a kinship it does not
have.

**As a list member the invalid combination is writable.** `[always, path:
"src/**"]` parses and validates, and under OR semantics `always` swallows the
entry beside it — the path is dead weight that does nothing, silently. As a
value of the field, that combination cannot be typed. **Making an invalid state
unrepresentable beats a rule forbidding it.**

**It shipped as both, briefly, and the failure is the argument.** `always` sat
in the tool's list of kinds while being unwritable: `matches: always` and
`- always:` were silently discarded, and `- always: true` parsed into a trigger
that classed the document as *cheap*. **A rule declaring itself ever-present was
the one rule that would not be there** — and nothing failed. It published,
validated, and lied.

## Load classes are a printed column, not a vocabulary

**Settled 2026-08-26.**

**A document's body is *always loaded*, *delivered when matched*, or *available
on request*.** Those are sentences, and they stay sentences.

**This slot has been named five times** — `preload`, `standing`, `advertised`,
`always-on`, `on-match` — and every one needed a paragraph underneath explaining
it. Five failures at one word is the finding: **the meaning is a sentence, so
naming it is the mistake.**

**`standing` is the instructive failure.** It was chosen carefully, with a
precedent already in use — `organizing-a-bundle` says *standing, kept present* —
and a reader still took it to mean *left over from before*, which is close to
the opposite. **A word chosen by argument lost to a word read on sight.**

**The names survive in exactly one place: a derived column printed beside its
input**, which `luma-foreman apply --explain` produces. A lookup table printed
with the answer already in it is not a glossary, and nobody has to learn it
before writing a document.

**Deferred:** whether *declared nothing* and *nobody looked* should be
distinguishable. They are the same to every consumer today, and **a field
nothing acts on is what `compliance` turned out to be.** The candidate answer is
`verified` (§7.2), which carries the distinction for every document rather than
as a special value in one field. **Re-open trigger:** a reviewer asks for it
twice, or a consumer appears that would behave differently.

## A luma tool writes into `.luma/`, and anywhere else is opted into

**Settled 2026-08-23.**

**`.luma/backlog/` always.** A tool may write somewhere else only when a
committed configuration file asks it to, and `.backlog/` is the name reserved
for that case in `luma-backlog`.

**The question was raised by a real conflict.** `luma-layout` reserves
`backlog/` as one of four `.luma/` tiers — *what we intend, churns* — while
`luma-backlog`'s specification puts the backlog at `.backlog/` in the repository
root. Four repositories already hold `.luma/backlog/ideas/`, so running
`luma-backlog init` in any of them produces a second backlog: two directories,
one lifecycle, and no reader able to tell which is authoritative.

**One default with an explicit override, never an inferred one.** The rejected
shape was *`.luma/backlog/` when `.luma/` exists, `.backlog/` otherwise* — which
makes the layout depend on repository state nobody was thinking about. Two
repositories, the same tool, different answers, and the rule invisible unless
you already know it. **A default that varies by environment is a default nobody
can predict.**

**This is the third time the same shape has been chosen**, which is why it is
recorded as a general position rather than a fact about the backlog. A catalog
declares a namespace and an adopter may override it in committed config. A
preclusion fails by default and is overridden in committed config. Now this.
*One default, one written override, and the override is reviewable because it is
in the repository.*

**Why not `.backlog/`, given the tool is pitched standalone.** It is the only
real argument on that side: `luma-backlog` is presented as a product rather than
an estate component, and `.backlog/` is guessable to somebody who has never
heard of luma. It loses on three counts. **A generic root is precisely what
`.luma/` was chosen over** — the layout's own argument is that a generic name
claims a universality nothing has earned, while `.luma/` is collision-proof by
construction. **`rm -rf .luma/` was named as a clean uninstall**, and two
dotdirs break it. And **the vendor name is already in what they typed**: you do
not install `luma-backlog` and find `.luma/` surprising.

**Ideas and tracked work share the tier, and that follows from the tier rule.**
Both are *what we intend*; `luma/idea` and a backlog record differ in
commitment, not in lifecycle. Splitting them across two roots would be topic
deciding location, which the layout forbids in the same paragraph that defines
the tiers.

**The cost, accepted:** a standalone adopter gets a vendor-named directory for a
tool they think of as a backlog, and changing it costs them one line of config.

**Standing consequence — this binds any tool writing durable project content.**
`luma-backlog` is the first to face it because it is the first to write records
rather than read them. The next one does not get to re-argue it.

**Re-open trigger:** if a tool ever has a reason to write outside `.luma/` that
is not a user preference — a format another ecosystem reads at a fixed path, for
instance — then this is a rule about *our* content and needs narrowing rather
than an exception.

## Tools are named for the verb they perform

**Settled 2026-08-23.**

**Name a tool for the job it does, in a word somebody can guess.** `curator` curates. The form is a verb plus `-er`/`-or` — the `compiler`/`linker` shape, naming the actor rather than the artifact.

**The test:** could somebody who has never read the docs guess what it does from the name? `curator` passes. `leader` and `foreman` do not, and that is the cost being accepted below.

**And the corollary is the useful half: if the job cannot be stated as one verb, that is a signal about the tool rather than about the name.** A name that has to be a metaphor usually means more than one job is hiding behind it. Run it on what exists — `curator` curates; `foreman` inspects, bootstraps, outfits and refits, which is four verbs and exactly why it needed a metaphor. **That makes the naming rule double as a junk-drawer detector**, which is the thing it is actually worth having.

**Why it matters:** the previous register was role metaphors — a leader decides what good looks like, a foreman makes it true on each site. That pairing teaches the architecture *once you already know it*, and is hard to recall before that. It also stopped scaling at three: adding a `quartermaster` to `leader` and `foreman` means three people-who-manage and no way to remember which does what. **Thematic naming families carry no information** — the Ruby failure, where knowing one name tells you nothing about the next.

**Apply it:** new tools take plain verb names. **Existing names are unaffected**, deliberately — `luma-leader` was renamed on 2026-08-21 after weighing sixty candidates, and a convention whose adoption requires renaming everything is one nobody adopts. The set is now mixed on purpose.

**Standing consequence — do not name a tool after the artifact it operates on.** That is the defect that forced the `luma-hq` rename: *"named after the thing it operates on, not after what it is. Git is not called repository; Terraform is not called state."* An agent noun escapes it because it names the actor, which is why `curator` is admissible where `catalog` would not be.

**Standing consequence — `<org>-catalog` is a catalog, and no engine takes a name near it.** The same reservation as the `-hq` suffix, and for the same reason: two sibling checkouts three letters apart is a glob away from the mistake that has already been made once here. This is what retired `cataloger`; see the correction below.

> *Corrected 2026-08-23 — the deferred alternative won and its trigger fired.* **The tool is `curator`, not `cataloger`.** The rule above is unchanged and `curator` complies with it; what changed is which candidate satisfies it for this tool, which is the deferred-alternative mechanism working rather than a reversal. Nobody who followed the old text is now in breach.
>
> **The trigger was *re-open before anything installs a binary, since renaming is free until then*, and nothing installs one.** It was the only open trigger in this document that got more expensive with time rather than staying flat, which is why it was fired deliberately rather than left.
>
> **It won on the argument it always had.** *The catalog is a catalog, not a registry* states outright that *"curation is the actual semantics"*, and `bundle-dependencies.md` independently calls judging a narrow constraint *"a catalog-editor job"* — so this is the vocabulary the design already used rather than a new metaphor.
>
> **And the guessability objection was outweighed rather than refuted.** `cataloger` does name its object where `curator` does not, and that remains true. It loses to the collision: `luma-catalog` and `luma-cataloger` are three letters apart, both would be siblings in the same checkout layout, and **a fresh agent has already been misled once by exactly this shape** — that is what reserved `-hq`. A cost paid every time somebody reads a name lost to a cost paid once by whoever reads the docs. In practice the tool runs inside a catalog, so the working directory supplies the object the name does not.
>
> **Nothing was renamed on disk** because nothing existed: no repository, no binary, no import. This is the whole cost of firing the trigger on time.

**Deferred alternative:** `packager`, with `luma-catalog` renamed to `luma-warehouse`. Rejected on two counts — **nothing is packaged**, so the verb is wrong rather than merely absent; and **a warehouse holds whatever you put in it**, which is the permissive-storage reading that *the catalog is a catalog, not a registry* was written to keep out. *Re-open only if the mechanism changes to match, which is that decision's own trigger.*

**Re-open trigger:** if a plain verb name ever collides with a widely-used tool in the same space, or if the mixed register — two metaphors and the rest plain — causes real confusion rather than mild untidiness.

## Repository boundaries do not control agent context

**Settled 2026-08-09.**

An agent working in a repository does not load the repository — it loads the skills and files it opens. Splitting work into two repositories does not protect an agent from over-context, and keeping it in one does not doom it. The lever is skill scoping and what each skill reads.

**Why it matters:** this was very nearly the argument for splitting `luma-foreman` out of `luma-leader` — a fear that the organization-level repository would drown an agent in context. That reasoning was wrong, and would have produced the right outcome for a reason that fails on the next split.

**Apply it:** when a split is proposed to manage context, the proposal is not yet justified. Find the real boundary or keep them together.

## Projects split on runtime location, not on subject matter

**Settled 2026-08-09.**

`luma-foreman` is separate from `luma-leader` because it *executes inside other repositories* — in a fresh project, in continuous integration, in a repository with no access to the rest of the organization. `luma-leader` is never installed anywhere; it is a place you visit. That is a physical difference, and it is the thing that forces a repository boundary.

**Deferred alternative:** starting foreman as skills *inside* `luma-leader` and extracting later. Legitimate, and it matches the house bootstrap order (lead with a skill, backfill the command). It was not taken because foreman's purpose is to run where hq is not checked out, which forces the split immediately rather than eventually.

**Re-open trigger:** if a second capability appears that is organization-level but must also run inside foreign repositories, revisit whether "runs elsewhere" is really the boundary or whether distribution is a packaging concern.

**Standing consequence:** foreman must never read `luma-leader` at runtime. The first check that needs organization context to run has broken the boundary, and extraction becomes a rewrite.

## Naming

**Settled 2026-08-09. `luma-hq` renamed to `luma-leader` on 2026-08-21 — the trigger fired.**

Roughly sixty candidates were considered across several registers before these two.

- **`luma-hq`** — the seat of authority. Chosen over `atlas`, `charter`, `canon`, `admiralty`, `citadel`, `foreman`'s superiors, and others. `atlas` and `charter` were both rejected for the same defect: they name a static artifact (a map, a founding document) when the repository has to keep acting — deciding what to build next, not just recording what exists.
- **`luma-foreman`** — runs the site, directs new work *and* inspects existing work. Chosen over `audit`, which named only the inspection half once the scope grew to include bootstrapping, tool installation, and periodic refits.

**Known costs, accepted at the time:** `luma-hq` used initials, against the house rule that terminology is spelled out. `foreman` is a single-site role for a multi-project job, and is shared with an existing infrastructure tool of the same name.

**Re-open trigger:** if the initials rule is ever extended to repository names, or if the `foreman` collision causes real confusion in practice.

> *Note added 2026-08-23.* **The register this established no longer applies to new tools** — see *Tools are named for the verb they perform*. Role metaphors stopped scaling at three, so `leader` and `foreman` keep their names and nothing after them takes one. This record is unchanged otherwise; the rename it describes still stands and its reasoning is untouched.

### The rename, and the defect that forced it

**`luma-hq` was named after the thing it operates on, not after what it is.** Git is not called *repository*; Terraform is not called *state*. Every tool named for its subject eventually collides with its subject, and this one did: an organization following the convention ends up with two checked-out repositories ending in `-hq` — the engine it installed, and its own headquarters.

**A fresh agent hit it on first contact.** Running `migrate-ideas` with no prior context, it found `luma-hq` checked out as a sibling with an ideas directory inside it, and concluded that was the headquarters. That was a reasonable inference from the only signals available, and it was wrong.

**`luma-leader`, because it pairs with `foreman`.** A leader decides what good looks like; a foreman makes it true on each site. The two names teach the architecture without anybody explaining it, which is what the previous name could not do. It is also spelled out, so the initials cost above is retired rather than merely accepted.

**Standing consequence — `-hq` is now reserved.** A repository name ending in `-hq` means *an organization's own headquarters*, and nothing else ever. That is what makes the inference above correct rather than caught: an agent that sees `acme-hq` and concludes headquarters is now right by construction. **The engine must never reclaim the suffix.**

*`luma-leadership` was the runner-up and lost on register: a discipline paired with a role is a category mismatch, where `leader` and `foreman` are both people. `luma-engine`, `luma-chief`, `luma-governor` and `luma-planner` were also considered.*

## No shared package until two real consumers exist

**Settled 2026-08-16.**

`luma-foreman` keeps the logic a shared package would hold. There is one tool and a plan, not two tools. A core extracted before the second consumer exists is shaped entirely by the first; the second then arrives, does not fit, and is either contorted or forked. Until then every change that was one commit becomes a version bump, a release, and a bump in the consumer, bought with nothing.

**Why it matters:** this is the same failure as splitting to manage context, reached by a different route. Anticipated sharing is not a boundary any more than anticipated context flooding was.

**Apply it:** extraction is mechanical, and by the time it is needed the real shape is known. Premature extraction is the expensive direction, so let the duplication happen first.

**Re-open trigger — all four, not any one:**

1. Two real tools exist, not one and a plan.
2. They duplicate *domain* logic, not utilities — how the two layouts are detected, how a tier is resolved, how a record is formatted. String helpers and retry wrappers never justify a repository.
3. The duplication has already bitten once, with the copies drifting into a real bug.
4. The shape has stopped moving. Extracting a moving target makes both consumers churn on every change.

**Watch item:** layout resolution. Locating `.luma/` and resolving a tier has to be implemented identically by foreman, by skills, and by anything else that reads one, which makes it the most likely first genuine duplication. *Reduced 2026-08-18:* a single `.luma/` layout replaced two supported shapes, so this is now a path join rather than a detection. A written contract that several places implement is cheaper than a package, and duplicating a small, well-specified function is not a sin.

**Standing consequence:** a dependency boundary is not the runtime-location boundary that justifies a split here. If extraction ever happens it will be the first split made on a different basis, and that basis has to be argued at the time rather than assumed from this one.

**Deferred — the name.** `luma-core` or `luma-commons`, chosen at extraction so it describes what the package became rather than what was planned. The distinction is worth keeping for any future naming of this kind, because **the name sets the admission policy**:

- *Core* implies a coherent center with primacy. Things extend it, and adding an unrelated helper to something called core feels wrong. It makes you justify each addition.
- *Commons* implies shared resources held by independent peers with no organizing principle. Apache Commons is a junk drawer by design — language, input and output, collections, unrelated utilities that happen to be shared — and that is correct for Apache. For a small organization it licenses unbounded accretion: each *exclusion* has to be justified instead.

Pick for what the package will hold in three years, not what it holds on the day it is extracted.

## Project configuration lives in the standards tier

**Settled 2026-08-17.**

> *Superseded in part.* **The paths below are historical.** What this entry says about *what* configuration is, and where it does not go, still stands — only the locations changed, twice:
>
> | then | now |
> |---|---|
> | `.hq/standards/luma.toml` or `.standards/luma.toml` | `.luma/config/foreman.toml` |
> | `~/.config/luma/` | `~/.config/luma/luma-foreman/` |
>
> The tier became `.luma/config/` — see [`.luma/` holds bundles, not policy](#luma-holds-bundles-not-policy) — and machine-local directories nest as `<org>/<repo>`, recorded in `luma-foreman/docs/standards.md`. The operative rules now live in the `luma/luma-config` bundle.

A project's Luma configuration goes in the in-force tier — `.hq/standards/luma.toml` nested, `.standards/luma.toml` flat — not in a fourth tier of its own. Its lifecycle is live, edited, and currently in force, which is that tier exactly, and the tier already holds which standards a project claims and which exemptions it was granted. Configuration is the same statement in machine form. Global configuration is separate and lives at `~/.config/luma/`.

Two kinds of configuration were being conflated:

- **Declarations** — which standards apply, which exemptions were granted, which checks run, what *done* means. The project stating its own rules. Committed, in `standards/`.
- **Machine-local settings** — timeouts, log level, cache location, concurrency. These belong to the machine, not the project, and live under `~/.config/luma/`, keyed by repository identity rather than absolute path so they survive a move or a second checkout.

**Why a fourth tier fails:** under the flat layout it would have to be `.config/` at the project root, colliding with the convention half the tooling world already uses. A tier name has to be safe flat as well as nested — the same rule that decided `records/` over `history/`.

**Standing consequence — everything in `.luma/` is committed. No exceptions.** If uncommitted files can live in a tier, a reader cannot distinguish an authoritative rule from a local tweak, and two agents on two machines read different rules for the same project. That is a correctness failure in the one system whose job is to say what the rules here are.

**Apply it:**

- A committed value that expresses what the project **requires** cannot be overridden locally. A committed value that is only a **default** can be. *Corrected 2026-08-17: this was first written as "the committed declaration always wins," which was too broad — it forbids a project suggesting a starting value for newcomers. The invariant that needs protecting is that local settings cannot switch off a standard the project claims, not that committed always beats local.*
- Name configuration files for the tool that reads them — `standards/foreman.toml`, `standards/luma.toml` — so several tools coexist without negotiating one format.
- Secrets stay out of `.luma/`, so configuration here references environment variables and never holds values.
- Vendor configuration is unaffected: `.claude/settings.json` and its kind stay where the vendor looks and are generated from the canonical version in `standards/`.

**Deferred alternative:** `standards/luma.local.toml`, gitignored, as Claude Code itself does. More ergonomic, since everything then sits in one place. Not taken because it puts an uncommitted file inside a directory whose entire promise is that its contents are durable and shared.

**Re-open trigger:** if keying machine-local settings by repository identity proves too awkward in practice to actually get used.

## Per-user project settings live outside `.luma/`

**Settled 2026-08-17.**

The first real case is foreman's *modes*: a command changes the mode for one repository, behavior changes on the next run, and no other user is affected. That is operator state, not a project declaration, so it lives outside `.luma/` entirely.

```
~/.config/luma/luma-foreman/projects/<id>.toml   a user's choices — must survive
~/.cache/luma/luma-foreman/projects/<id>/        derived data — safe to delete at any time
```

**Configuration and cache stay apart.** The test: if deleting it loses a decision the user made, it is not cache. Putting mode under a cache path means clearing caches silently resets behavior with no trace of why.

**`<id>` is generated once and committed** in `.luma/config/foreman.toml`. It is the only durable key — checkout paths break on a move or a second clone, and remote URLs break for repositories that have none or that get migrated. Committing an identifier is not committing the mode.

**Precedence, designed once because committed settings are coming:**

```
1. built-in defaults                          in code
2. ~/.config/luma/luma-foreman/config.toml            global, all projects
3. .luma/config/foreman.toml  [defaults]              project suggests — overridable
4. ~/.config/luma/luma-foreman/projects/<id>.toml     this user, this project
5. .luma/config/foreman.toml  [require]               project mandates — not overridable
6. environment variables and flags            this invocation only
```

The committed file appears twice at different priorities because it holds two kinds of statement. Splitting it into `[defaults]` and `[require]` tables puts the precedence in the file rather than in a rule someone has to remember.

**Apply it:**

- Build layer 4 only for now, but put the lookup behind one resolution function so the rest slot in at a single site. Reserving the shape is free; building the chain before there is anything to resolve is premature extraction in miniature.
- Read the file per invocation and never memoize across runs. "Instantly changes" is free for a short-lived process and stops being free the moment anything long-lived exists, which must then re-read or watch.
- Keep `luma mode set strict` and `luma --mode strict` distinct. The first writes the file; the second affects one invocation. A flag that quietly persists produces state nobody remembers setting.

**Standing consequence: mode must never change pass or fail.** If it can, two developers on the same commit get different verdicts and an audit in `records/` means "passed in whatever mode that person happened to be in." Mode may change how much foreman says, how much it runs, and whether it blocks locally — not what conforming means. Two guardrails follow: authoritative audits ignore mode and run in the project's declared configuration, and every audit record states the mode it ran under, so a convenience run can never be mistaken for an authoritative one.

**Re-open trigger:** if a mode is ever wanted that legitimately changes what conforming means, it is not a mode — it is a project declaration, and it belongs committed in `.luma/policy/`.

## Shared types travel inside bundles, and a catalog publishes them

**Settled 2026-08-17.**

The question was where non-built-in Luma Knowledge Format types shared across many projects should live — a new repository, `luma-foreman`, or `luma-leader`. It rests on a premise worth rejecting: that a type is a unit of distribution. It is not. A bundle is.

A Type Definition is vendored — copied into a bundle, never resolved remotely. So wherever it lives is a source you copy from and nothing breaks when it is offline. That makes this a curation and publication question rather than an architectural one, and lowers the stakes accordingly.

A type is also inert alone. A shared `incident` type is wanted because something *runs* incidents — a workflow, templates, a skill. **The type ships inside the bundle that gives it meaning**, and lives wherever that bundle lives.

**The residual case is real but small:** cross-cutting types with no natural owner — `person`, `decision`. Several bundles want them and none is home. Those earn a **foundation bundle**. Collisions resolve themselves when types are namespaced by publisher and versioned: two bundles vendoring the same type at the same version produce identical files, so the duplicate is a no-op. Different versions is a genuine conflict and one worth surfacing rather than silently merging.

**Four roles, which is why three repositories could not hold it:**

| role | job | why it cannot absorb the others |
|---|---|---|
| format | defines what a type *is* | its authority comes from staying minimal |
| catalog | publishes versioned bundles | it is the artifact; needs its own cadence and trust boundary |
| governance | argues what belongs in the catalog | it is what an organization forks and replaces |
| applier | vendors, pins, detects drift | runs where none of the others are checked out |

Catalog and governance each exist twice — a public one and an organization's internal one — and they are the same shape, so the applier needs one mechanism rather than two.

**Why not each candidate:**

- **Not the applier (`luma-foreman`).** Content inside the tool welds two version numbers that must move independently. A project could not pin a type set without pinning the tool, and a patch release would ship data-model changes. That is exactly the adopt-and-pin property the catalog exists to provide.
- **Not governance (`luma-leader`).** It is the layer organizations are invited to replace wholesale. Content that should survive a fork of the governance layer cannot live inside it — and the applier reading it at any point turns an optional repository into a required one, breaking the standing consequence recorded above.
- **Not the format.** The moment the specification ships a `person` type it is making domain claims, and "do not redefine the built-ins" goes fuzzy: `person` is not built in, but it arrived in the same box, so it will be treated as though it were. A fixed set of built-ins is what keeps a specification a specification instead of a starter kit.

**Apply it:** put each type in the bundle that uses it and let the duplication happen. Extraction is a file move, cheap once the shape is known and a guess before then.

> *Corrected 2026-08-23 — the facts moved, the position did not.* This said *"nothing is extracted yet"*, and something is now: `luma/catalog` and `luma/project` live in the `luma/luma-types` bundle. That is **this decision being applied rather than reversed** — the foundation bundle it names, published by the catalog as it says. Both extraction triggers below have been met: two universal bundles exist that are not the applier's own, and more than one bundle needs the same type identically. The reasoning is untouched, and all three rejected homes were re-derived independently before anybody reread this record.
>
> `luma/catalog` was promoted on the *built explicitly to be shared* exemption rather than on a count. Waiting for a first outside user before centralising the type that **defines a catalog** would have produced copies to reconcile, not evidence.

**Extraction triggers — the foundation bundle:** a second bundle genuinely needs a type the first already defines, and the two copies must stay identical. One bundle wanting a type is not a shared type.

**Extraction triggers — the catalog repository:** two universal bundles exist that are not the applier's own. Until then the catalog is a directory, not a repository.

**Deferred alternative: the catalog as an index over many repositories** rather than a single one. Probably right in the end state, since three buckets of publishers means multiple publishers by definition. Not taken now because a single repository converts into an index without breaking anything — the index's first entry is that repository — while the reverse is a migration. *Re-open when a publisher outside this organization wants to list a bundle.*

**Standing consequence:** the catalog is copied from, never resolved against. The moment anything queries it at apply time, the self-containment property every bundle rests on is gone, and the naming decision below changes with it.

## The catalog is a catalog, not a registry

**Settled 2026-08-17.**

Prior art was surveyed for mechanism, and the industry's word for the catalog-shaped thing is *registry*. It was not taken.

**Registry imports the one expectation the design rejects.** Every registry people know — npm, Docker, Terraform, crates.io — is a service resolved against at install time. The word carries a client, an API, a version solver, and a dependency graph. The settled design is the opposite on all four: vendoring, no remote lookup, no dependencies, self-contained bundles. Under that name every newcomer spends a week looking for the resolver, and whoever eventually adds one will feel they are completing the design rather than reversing a decision.

*One of those four properties is what [bundle-dependencies.md](bundle-dependencies.md) proposes changing. **That draft is not adopted.** Were it adopted, the other three would still hold, this decision's own re-open trigger is remote resolution and has not fired, and the naming argument would be unaffected — there would still be no client, no API and no solver.*

**Admission policy.** By the lens recorded above, *registry* is permissive — anyone registers, and the registry holds no opinion. *Catalog* implies an editor decided an entry belongs. Given that material travels project → organization → universal by promotion, and that entries can be mandated downward, curation is the actual semantics. A registry that mandates is strange; a catalog with an editor is ordinary.

**A local collision, small but free to avoid.** `luma-foreman` already has `inspect/registry.py` for its rules. Two registries meaning different things in one codebase is a permanent tax.

**Apply it:** *registry* stays the right word when pointing at prior art — "the catalog is the registry-shaped thing, minus resolution" is useful when borrowing the manifest and lockfile patterns. It is not the name of the thing.

**Deferred alternative:** *registry* as the name, revived if the mechanism changes to match it.

**Re-open trigger:** if resolution ever goes remote — if the catalog becomes an index tools query at apply time rather than a place they copy from — it has become a registry and should be called one. The word tracks the mechanism.

## Only the engines are never forked; everything else is yours

**Settled 2026-08-18. Clarified 2026-08-21** — the original title said *nothing is forked*, which read as a ban on forking anything. Almost everything is forkable. **Two repositories are not.**

**Never forked: the engines.** `luma-leader` and `luma-foreman`. Install them, pin them, never edit them. **If anyone ever needs a private foreman, foreman has failed to be configurable**, and the same holds one level up.

**Everything else is yours to fork, copy and edit** — bundles, catalogs, an hq, and **the knowledge format itself.**

**Copying a bundle into your own catalog and changing it is the supported path**, not a workaround. `acme/migrate-ideas` beside `luma/migrate-ideas` is two bundles from two publishers, no collision, and a project adopts whichever it wants. If yours turns out better, promotion back upstream is the designed path: project → organization catalog → universal.

**Forking the format is encouraged, with one constraint.** An organization that needs its own types, fields and conventions should fork `luma-knowledge-format` and build them. **Extend rather than redefine** — a fork that *adds* still reads every universal bundle, so you keep the catalog and gain your own; a fork that changes what an existing field means makes those bundles unreadable, and you have traded a shared library of best practice for a private dialect. **The point of forking it is to get both**, and only redefinition costs you one of them.

An organization's internal hq is therefore *not* a fork of `luma-leader`. It is a separate repository that `luma-leader` operates on — exactly as a project is not a fork of foreman. The fractal, one level up:

|  | engine — installed, pinned, never edited | content — yours |
|---|---|---|
| organization | `luma-leader` | `acme-hq` |
| project | `luma-foreman` | the project's `.luma/` |

**Why it matters:** the question that produced this was "how do bundles get imported without being pushed into the upstream forked code." Under a fork-the-engine model that question has no clean answer and every solution is git surgery. Under this one it does not arise. **A design step that requires forking an engine is the bug, not the plan.**

**Forking content has one real cost, and it is worth naming.** A copied bundle stops receiving upstream improvements, and **nothing tells you** — a renamed bundle is not drift, it is a different bundle, so drift-checking has nothing to compare. That is fine for one bundle you deliberately own. It does not scale to the same edit across forty of them, which is a cross-cutting concern and wants a different mechanism.

**The repository set:**

```
luma-leader            engine     public    argue standards into existence
luma-foreman           engine     public    apply them, one repository at a time
luma-knowledge-format  format     public    the format everything is written in — fork to extend
luma-catalog           content    public    universal bundles
acme-hq                content    private   an organization's governance, learnings, analysis
acme-catalog           content    private   its bundles, and what it mandates
<project>/.luma/       content    per-repo  adopted bundles, vendored
```

An adopting organization adds two of them, not seven.

**The organization's catalog is a separate repository from its hq, and this is the load-bearing one.** Foreman pulls the catalog into continuous integration, onto contractors' laptops, and into every project checkout. An hq holds competitive analysis, boundaries, and learnings. One repository means the artifact distributed widely and the material kept close share a single permission bit, and the first continuous-integration runner to clone it is the incident. **Access control is the boundary.** It is also the runtime-location rule again: the catalog is fetched at apply time, an hq never is.

**Apply it — how a bundle lands in a project.** Manifest, vendored copy, no resolver:

```
.luma/policy/
  foreman.toml                     adopted bundles and pinned versions
  bundles/
    luma/conventional-commits/     vendored, byte-for-byte
    acme/incident-response/
```

The vendored directory *is* the lockfile — a checksum over it is the entire verification story. Comparing manifest, vendored content and catalog yields three failures that are genuinely different and must not be collapsed:

- **local drift** — the vendored copy was edited. Revert it, or rename it into a project bundle and own it.
- **upstream drift** — a newer version exists. Informational. This is the "do not change under me" guarantee working, not a fault.
- **mandate drift** — a version was mandated, the deadline passed, the project is behind. Hard failure.

The third is why a catalog describes itself at its root the way a bundle does: what it publishes, what it mandates, and by when.

**Apply it — promotion is copy-then-adopt, one step at a time.**

```
project bundle  ──promote──>  acme-catalog  ──promote──>  luma-catalog
acme-web/deploy               acme/deploy                 luma/deploy
```

Because bundles are self-contained and depend on nothing, promotion is a directory copy plus a namespace rewrite — no subtree, no submodule, no git surgery. It is deliberately two steps: promotion copies *out*, and the originating project then separately adopts the promoted copy and drops its local one. Promotion that silently rewrote the project would break the exact property the model exists to protect. No skipping a level either — the organization's catalog is where vouching happens, and a project publishing straight to universal means bundles arrive that nobody stood behind.

**Standing consequence:** enforcement is pull-based and fails closed. Nothing pushes into a repository it does not control; an unmet mandate past its date fails the check, which works everywhere without write access. Since foreman must run in a bare clone with no network, mandates cannot be read from the catalog at check time — a sync writes them into the committed manifest, and a stale sync is itself a finding.

**Deferred alternative:** adopting a bundle by reference rather than vendoring it — a symlink or a pointer. Cheaper to update and it would make upstream drift impossible. Not taken because it breaks self-containment: a checkout would no longer carry what it needs, which is the property the bare-clone constraint depends on. *Re-open if vendored copies prove unmanageable at scale.*

**Open, and the harder one:** what happens when an organization wants to modify a universal bundle rather than take it whole. Every answer so far reintroduces forking in some form, which is why it is recorded as open rather than guessed at.

## Bundles are versioned; the catalog is not

**Settled 2026-08-18.**

A bundle carries a version and a project pins it. A catalog carries no version of its own.

**Why:** a catalog version implies its entries move together and are guaranteed against one another. That is what a distribution release buys, and it buys it because entries have dependencies. Bundles do not, so a catalog version would change without conveying anything — the same second-version-number trap as putting types inside the tool.

**The real need behind the question is already met.** "What did the catalog contain on date X" is answered by the catalog being a git repository: a commit identifier is that answer, for free. Where a mandate needs a stable referent, tag it — a tag labels a snapshot without implying compatibility, which is the weaker and more honest claim.

**Re-open trigger:** if bundles ever gain dependencies on each other, entries do need co-guarantees and a catalog version starts meaning something.

*A draft design would fire this trigger — see [bundle-dependencies.md](bundle-dependencies.md), which proposes that bundles may depend on one another and that a catalog guarantees its entries against each other at publication. That is exactly the co-guarantee a version was said to imply. **Nothing is settled and this decision stands as written.***

*That draft also answers the question the trigger raises, and the answer is still no — every commit of the catalog is already a consistent set, nothing takes a catalog version as an input, a project's manifest holds reproducibility more precisely, and the "tag a snapshot" answer above gets stronger rather than weaker. **The reason recorded here would be void; the conclusion would not.** Written down so it is not re-derived by whoever reads this trigger next.*

## Bundles have two axes: reach and obligation

**Settled 2026-08-18.**

**`reach`** — which catalog a bundle came from: universal, organization, or project. Chosen over *tier*, which names a position in a hierarchy where what matters is the consequence of that position. It is **never declared.** A bundle has universal reach because it sits in the universal catalog, so promotion is a directory move with nothing to edit and no way for a bundle to misstate how far it travels.

> *Retired 2026-08-18.* The mechanism above stands — a bundle still declares nothing about its origin — but the **word** is gone: nothing ever read it, and it collided with the more useful reading of "reaches projects". Say "a bundle from the universal catalog". See [Location carries only what is single-valued and permanent](#location-carries-only-what-is-single-valued-and-permanent).

**`obligation`** — how strongly the publishing catalog expects a project to adopt it, declared per bundle in that catalog's `catalog.md` with an optional `by` date:

```yaml
requires:
  - bundle: luma/conventional-commits
    obligation: mandatory
    version: ">= 2.0.0"
    by: 2026-10-01
  - bundle: acme/deploy-checks
    obligation: recommended
```

| value | effect when foreman checks a project |
|---|---|
| `mandatory` | must be adopted — a warning and countdown until `by`, a failure after; with no date, a failure immediately |
| `recommended` | reported as a gap, never fails |
| `optional` | a curated shortlist; never reported as missing |
| `deprecated` | reported if still adopted — the retirement path |

**This is the Luma Knowledge Format's own field ladder, reused deliberately.** It already defines these four for fields. This is the same question — how strongly is this expected — asked about a bundle instead of a field, so a parallel vocabulary would be two words for one idea.

**Obligation cannot be a property of the bundle.** The same bundle is mandatory at one organization and merely available everywhere else. The publisher declares it; the artifact does not carry it.

**The distinction that has to survive:** *obligation governs whether you must adopt; it does not govern how hard conformance is checked once you have.* A recommended bundle a project chose to adopt is checked exactly as strictly as a mandated one — drift is drift. What `recommended` buys is the freedom not to adopt it at all.

**Why it matters:** this preserves the earlier position that nothing in the catalog is advisory, which rested on graded severity being the setting everyone quietly turns down until failures stop happening. That danger lives entirely in the second question. Grading the first costs nothing, because a project that declined a recommendation is not failing — it simply has not adopted it.

**Standing consequence — location carries the metadata, when it can.** Three further fields were proposed and dissolved the same way. Reach is which catalog you found it in. Who to ask for an exemption is whichever catalog declared the obligation. **Before adding a field, check whether the fact is already implied by where the thing lives.**

*Corrected 2026-08-18: this originally claimed all three dissolved, citing the `project/` and `organization/` directories as the third. That one was wrong, and the correction sharpened the rule rather than weakening it — see [Location carries only what is single-valued and permanent](#location-carries-only-what-is-single-valued-and-permanent).*

**Deferred alternative:** `authority`, naming who may remove a bundle — project, organization, or universal — instead of how strongly it binds. It answered exemption routing in one field, but it described a consequence rather than the thing itself, and the routing answer turned out to be derivable. *Re-open if a bundle can ever be mandated by a party other than the catalog that published the requirement.*

**Deferred alternative:** a graded enforcement level in the manner of Terraform Sentinel's advisory / soft-mandatory / hard-mandatory, applied to conformance rather than adoption. *Re-open only with a concrete case that a hard failure genuinely cannot serve, since the failure mode is the whole estate sitting at the softest setting.*

**Re-open trigger:** if `optional` and *absent from the catalog* prove indistinguishable in practice, `optional` is doing no work and the ladder is three rungs.

## Starters seed new things, and are never retroactive

**Settled 2026-08-18.**

A **starter** is what a new project or a new hq begins with — a named list of bundles declared in a catalog's `catalog.md`. It is a separate concept from obligation, and the test that separates them is sharp: *obligation is ongoing and applies to everything; a starter applies once, to something new.* A five-year-old repository is expected to satisfy a recommended bundle. It is not expected to retroactively acquire whatever a new repository happens to be seeded with today.

**Starters are never retroactive, and this is the load-bearing half.** Changing a starter changes what the *next* thing begins with and touches nothing that already exists. That is what lets an organization evolve its defaults freely — experimenting without every edit becoming a fleet-wide migration. Anything meant to reach existing projects is an obligation, and the fact that it is a different field is the point.

**Layering makes inherited and chosen defaults readable side by side**, which was the requirement — an organization must be able to see what it was given for free and what it decided:

```yaml
# acme-catalog/catalog.md
starters:
  project:
    extends: luma/project
    adds:
      - bundle: acme/deploy-checks
        version: "0.2.3"          # optional pin
      - bundle: acme/incident-response
    excludes:
      - luma/adr-workflow
```

**`excludes` is deliberate, against the tidier add-only rule.** An organization that cannot drop one thing its upstream seeds will stop extending that starter and write its own from scratch — and then silently stops receiving any upstream improvement forever. An explicit exclusion is a visible, arguable decision; a from-scratch copy is an invisible one.

**Pins are optional, and unpinned is both the default and the common case.** An entry with no version takes the latest at the moment of bootstrap, and the project's own manifest records what it got. Pinning uses the same constraint syntax as `requires:`, because the motivating case wants a ceiling rather than a freeze.

An earlier draft forbade pinning outright, on the grounds that a stale pin would freeze new projects at whatever was current when someone last edited the list. That was overreach, and one case settles it: **a bad upstream release.** Without pinning, an organization's only lever against a regression it does not control is to exclude the bundle entirely — so every new repository gets nothing instead of the last good version. The workaround is strictly worse than the problem the rule prevented. The validated-combination and deliberate-lag cases are legitimate on their own.

**Version ranges here do not reintroduce a resolver.** With no inter-bundle dependencies there is nothing to satisfy jointly, so "highest version matching this expression" is one independent lookup per bundle — a filter, not a constraint system. This borrows the ergonomics of ranges without buying the resolution problem, and only because bundles cannot depend on one another.

*The stated reason depends on bundles having no dependencies, which [bundle-dependencies.md](bundle-dependencies.md) proposes changing. **That draft is not adopted and this decision stands as written.** Were it adopted, the conclusion would survive on a different reason: resolution would still never choose between candidates.*

**Standing consequence — the catalog needs its own doctor.** A pin in a project manifest is drift-checked on every run, so falling behind is loud. A pin in a starter is checked by nothing, and because starters are not retroactive, no existing project ever surfaces the rot. `hq catalog doctor` reports every pin with a newer version available and how far behind. Same reasoning that made `foreman policy doctor` worth more than `install`: "is it wired up" and "is it still right" are different questions, and only the second catches quiet decay.

**Re-open trigger:** if starters proliferate past the two named ones — a set per project kind, per team, per language — the naming scheme is doing work a tag should be doing.

## Projects declare tags; catalogs key on them

**Settled 2026-08-18.**

Different kinds of project want different defaults and different mandates. **The project declares what it is, and the catalog says what each kind gets.** The catalog never holds patterns matched against repository names.

```toml
# <project>/.luma/config/foreman.toml
tags = ["service", "infrastructure"]
```

**Why the inversion matters:** a catalog holding patterns — `applies: repos matching ^infra-` — is where policy systems die. Patterns go stale, nobody can reconstruct why a repository matched, and exemptions accumulate as expressions no one dares edit. A repository knows what it is; a catalog does not. This is the same principle that dissolved three proposed fields — put the fact where it is already true.

**The vocabulary is published, not free-form.** A catalog declares the available tags and an organization extends that list exactly as it extends a starter. A tag outside the vocabulary is an error.

Free-form tagging fails in one specific and unacceptable way: one repository tags `infra`, another `infrastructure`, and a mandate silently does not apply to the second. **A mandate that fails to fire is invisible** — everything reports green. That is the worst failure this system can produce, and a closed vocabulary removes it for the cost of one list.

**A tag list means *any*, and that is the entire selector language.** `tags: [design, infrastructure]` matches a project tagged either one. Not both. "Design projects and infrastructure projects both need this" is overwhelmingly the common case, and a list that means different things in different contexts is how configuration becomes unreadable.

**When both are genuinely required, the answer is a new tag, not a boolean.** "Mandatory for repositories that are public *and* hold personal data" becomes a `public-pii` tag the repository declares. That is better than the expression it replaces on three counts: the composite category acquires a name, someone must claim it in a committed file, and it can be argued with. A boolean in the catalog is none of those.

**This is the line to hold hardest.** Every policy system that collapsed did so by growing one more operator at a time, each individually reasonable.

**Standing consequence — tags are an exemption backdoor, and the mitigation ships with the feature.** A repository facing an unwelcome mandate can retag itself out of scope in a one-line diff nobody reads closely. So when tags change, foreman reports the cost: *"removing `infrastructure` drops 3 mandates: change-review, terraform-checks, secret-rotation."* That turns a quiet edit into a claim someone has to stand behind — the same shape as making an exemption a sentence rather than a pattern.

**Apply it:** the *New repo survey* already captured in `luma-foreman/docs/IDEAS.md` is where tags come from. Most of those answers are already tag-shaped, and the ones that are not — "changes are expensive here" — probably want to be.

**Deferred alternative:** exceptions, as a single flat `except: [archived]` list. Bounded, probably needed eventually, and not the thing that kills selector languages — composability is. Not taken now because the positive form usually works. *Re-open when a real mandate needs an exclusion that cannot be expressed as a tag someone is willing to claim.*

## Obligations resolve most-restrictive-wins

**Settled 2026-08-18.**

A bundle may appear in `requires:` as many times as it needs to. Every entry whose tags match the project applies, and the strongest obligation among them is the one in force.

```yaml
requires:
  - bundle: luma/change-review
    obligation: recommended
  - bundle: luma/change-review
    obligation: mandatory
    tags: [infrastructure]
    by: 2026-10-01
```

"Mandatory for infrastructure, recommended for everyone else" cannot be one entry, and multiple entries plus a precedence rule is cheaper than a conditional syntax.

**The rule is already in use elsewhere** — permissions resolve deny over ask over allow — so this is one rule in a second place rather than a second thing to learn.

**It also settles cross-catalog conflict with no additional mechanism.** When an organization's catalog and the universal catalog both speak about the same bundle, the same rule applies: an organization may raise a `recommended` to `mandatory`, and may not lower a universal mandate. No precedence table, no notion of catalog priority.

**`deprecated` is not on this ladder.** It states something about the bundle's future rather than its strength, so a bundle that is both mandated and deprecated is not a precedence puzzle — it is a broken catalog.

**Standing consequence — a catalog is rejected at publish for contradictions only it can see.** Two are known: a bundle both mandated and deprecated, and a starter pinning a version the same catalog's own mandate forbids, which would make every new repository born failing. This is the one error class the catalog can commit that no individual project could ever detect, so it must be caught where the catalog is written rather than where it is applied.

**Re-open trigger:** if an organization ever has a legitimate need to *lower* an inherited mandate, most-restrictive-wins is the wrong rule and the relationship between catalogs is delegation rather than inheritance.

## Catalogs do not inherit; only starters do

**Settled 2026-08-18.**

There is **no catalog-level `extends`.** The question was whether an organization's catalog inherits its upstream wholesale or list by list, and asking it per list dissolves two thirds of it.

| list | when two catalogs speak about it | needs declaring |
|---|---|---|
| `tags` | union | no |
| `requires` | most-restrictive-wins | no |
| `starters` | explicit `extends` / `adds` / `excludes` | **yes** |

**`requires` needs nothing** — cross-catalog resolution is already settled as most-restrictive-wins, so entries merge at read time. An `extends` would only permit an organization to *drop* an inherited requirement, which the same decision forbids.

**`tags` needs nothing** — a vocabulary is a flat set, so two catalogs union. A tag nobody uses is inert, so subtracting one buys nothing.

**`starters` need it**, and are the only list that does. Both catalogs define one named `project`, so the name collides — and unlike the other two, subtracting is a legitimate act, which is the whole reason `excludes` exists.

**The principle, which is the reusable part:** *merge additively where more is safe; require explicit inheritance where subtraction is legitimate.* Extra tags are inert. Extra requirements are more restrictive, which is the safe direction. Extra seeded bundles are neither — a starter fires once at bootstrap with no ongoing check to catch a bad inclusion, so it needs an owner rather than a merge rule.

**A catalog-level relationship does exist, and it is not inheritance.** Something must say that an organization's projects also read the universal catalog, and the alternative — every project listing every catalog — is repeated in every repository and drifts. So a catalog names its upstream:

```yaml
upstream: https://github.com/LumaStack/luma-catalog
```

**`upstream` is a source pointer: where else to look, not content to inherit.** The distinct word is deliberate, because `extends` would be read as the thing this decision rules out. Single-valued and acyclic, matching single inheritance on types — a linear chain is cheap to walk, while a graph is the resolution problem bundles were designed to avoid, arriving through a side door.

**Apply it:** a project names only its organization's catalog and gets the chain.

**Re-open trigger:** if a catalog ever needs two upstreams — an organization drawing on the universal catalog and an industry or consortium one — single-valued is wrong, and the merge rules above have to be checked for order-dependence before allowing it.

## Location carries only what is single-valued and permanent

**Settled 2026-08-18.**

**A bundle's path is its identity for adoption.** Anything encoded in the path becomes unchangeable without breaking every pin, manifest entry and adopt command that refers to it. So the test for whether a fact belongs in a directory or a field is whether it is **single-valued and permanent** — and only then.

| fact | single-valued? | permanent? | where |
|---|---|---|---|
| which catalog it is in | yes | moving it *is* promotion | directory |
| which kinds of consumer may adopt it | **no** — often both | yes | field |
| what kind of bundle it is | no | no — reclassifying is normal | field, if ever needed |

**This corrects an error.** An earlier decision sorted bundles into `project/` and `organization/` directories and recorded that as a third instance of "location is the metadata." It was wrong twice over. Some bundles genuinely belong at either level depending on the adopter — a decision record or an incident process is wanted centrally by one organization and per-repository by another — so the fact is not single-valued and a directory can only ever say one. And it made the **publisher** answer a question belonging to the **adopter**, permanently and with no override.

**Bundles are therefore flat, and declare `consumers`:**

```yaml
type: bundle
version: 1.0.0
consumers: [project, organization]
```

`project` means foreman installs it into a repository; `organization` means hq installs it into a headquarters; both means the adopter chooses. Most bundles name one. The ones naming both are exactly the case the directories could not express.

Everything the directories were buying survives. Adopting at an unsupported level is still impossible — the engine refuses — and the declaration is strictly more accurate, because directories also blocked the legitimate case. Routing is unchanged: the adopter states the level. Cardinality is unchanged: adoption records where it landed. The only thing lost is having no field to maintain, which is not worth forcing a wrong answer on a whole class of bundles.

**Categories would fail the same test.** Sorting bundles into `workflows/` and `standards/` fails on both columns: an incident-response bundle is a workflow *and* ships a Type Definition *and* carries templates, and reclassifying one later moves something whose content never changed. Nothing reads the category in any case — a bundle declares where its own contents go, so it is browsing metadata, not routing. If browsing gets hard, that is a multi-valued field, never a directory, and never `tags`, which is taken by project self-declaration.

**No index of bundles in `catalog.md`.** The directory listing is the list. A second source that can disagree with the filesystem only raises the question of which is right, and `starters` and `requires` already name bundles, so a catalog is authoritative about obligations without also keeping a census.

**Standing consequence — `reach` is retired as a term.** It named which catalog a bundle came from, but nothing ever read it: promotion is moving a directory, origin after vendoring is carried by the namespace, obligation resolution walks the upstream chain, and mandate ownership is whichever catalog declared it. It was a label for a fact always visible anyway, and it collided with the more useful reading — a bundle "reaching" projects. Say "a bundle from the universal catalog" instead. `consumers` and `tags` are the declared vocabulary; there is no third.

**Gap closed 2026-08-18.** This originally recorded that `consumers` was declared by no Type Definition and rode as an undeclared extra key, and asserted the fix depended on where `_types/` resolves. That was wrong — the two questions are unrelated. `consumers` was simply added to the built-in `bundle` type in the Luma Knowledge Format: `optional`, `list of text`, **open vocabulary with no values defined by the format**. Naming `project` and `organization` in a specification would have it declare a governance hierarchy it has no business knowing about, so the values belong here rather than there, exactly as `tags` is left loose. Drafted on the format's `develop`; unratified until a release is tagged.

The general lesson is worth more than the fix: when a field has nowhere to be declared, check whether the type it belongs on should simply carry it, before concluding that resolution is the blocker.

**Re-open trigger:** if a third level appears — a workstation, say — check that it is a level rather than a different axis wearing the same clothes. `consumers` accepts one for free, which is the point of it being a field.

## Preload is declared by whoever holds the knowledge

**Settled 2026-08-18.**

**There is only one kind of preload.** It applies to documents and to bundles alike — the same operation on different objects, each relative to whatever contains it. A document's preload is relative to its bundle; a bundle's is relative to whatever adopted it. What differs between the two is not the meaning but **who is entitled to declare it**, and that produces opposite override rules.

| | the question | declared by | may the other side override? |
|---|---|---|---|
| **document** — `preload` | which of this bundle's documents do I need ahead of work? | the bundle author | adopter may **raise**, never lower |
| **bundle** — `preload_default` | should this bundle be loaded ahead of work at all? | the adopter, in their manifest | **adopter always wins**; the author only suggests |

**Say "session" in prose, never in a name.** Describing preload as *what loads at the start of a session* is the clearest explanation available today and should be used freely. Putting `session` in a field name is different: names are the part that cannot be revised without breaking every document carrying them, and sessions are a feature of how assistants happen to work right now. A name has to survive an architecture that may not have them; a sentence explaining the name does not.

**Which documents inside a bundle matter is a structural fact about the bundle**, and only its author knows it. An adopter who has never read the bundle cannot say which document is the spine and which is reference material; one who overrides downward is claiming to know the internals better than whoever wrote them, which is how a bundle silently stops working. So those overrides are permitted, discouraged, and one-directional.

**Whether a bundle is relevant to a session is a fact about the adopter's work**, which the author cannot know — incident response is ambient at one organization and situational at another. The adopter winning outright here is correct rather than a compromise.

**The test that settles which is which: whose bug is it if the value is wrong?** A mandatory document that was never needed is the author's mistake, in their bundle. A bundle loaded every session that should not be is the project's. Preload failures are author bugs; session-loading failures are adopter bugs.

**A bundle may suggest a session default, and the onboarding case justifies it.** Adopting a starter set of eight bundles and being asked to decide session-loading eight times is friction nobody pays — the result is that everything loads or nothing does. An author who knows their content is ambient rather than situational should be able to say so.

**`preload_default` carries the session question, not a document inheritance default.** The name permits both readings — on a bundle manifest sitting beside a `_types/` directory, "default preload" can be read as *what my documents inherit when they do not declare one* — and that reading is deliberately **not** what this field means. It was chosen over `session_preload` on two counts: it reads better and it sorts adjacent to any future `preload_*` key, and `session` does not belong in a name that has to outlive sessions. The residual ambiguity is accepted as a known cost. If the inheritance meaning ever earns a field, it needs a different name rather than this one being split.

**Apply it:** nothing is built at the bundle level yet, deliberately. No manifest with per-bundle entries exists, so a suggestion field has nothing to suggest *to* and no way to show whether its shape is right. It is also arguably foreman's rather than the format's, since *what my project loads at session start* is not a statement about knowledge. It stays out of the specification until the manifest exists.

**Deferred alternative:** letting adopters override document-level `preload` per document, keyed by Document ID in their manifest. Not taken now, because such an override reaches into another bundle's internals by path — the author renames a file in the next version and the override silently stops applying, which is the invisible-failure class this design fights everywhere else. *Re-open when a real bundle needs overriding. If it is built: an override naming a Document that does not exist MUST be an error rather than a no-op, and overrides should be reported rather than honoured quietly, since a project accumulating them is evidence the bundle is mis-specified for everyone.*

**Re-open trigger:** if authors routinely set `preload_default` and adopters routinely keep it, the suggestion is doing real work and may deserve to bind. If adopters routinely override it, it is decoration and should be removed rather than kept.

## The project directory is `.luma/`

**Settled 2026-08-18.** Supersedes the `.hq/` layout and the `standards/` tier name. Reasoned greenfield, deliberately ignoring the earlier conclusions.

```
.luma/
  backlog/     what we intend            churns; items created and destroyed
  policy/      what is in force          live, constantly edited
  records/     what happened, and why    append-only, dated, never edited
```

> *Tier list superseded the same day — see [`.luma/` holds bundles, not policy](#luma-holds-bundles-not-policy).* `policy/` is gone, `bundles/` and `config/` replace it. Everything below about **why the directory is vendor-named, why one root, and why hidden** is untouched, and so is the argument that freed `standard` for the organization level — `policy` remains the word for a course of action, it is simply a document type rather than a project tier.

### Why a vendor-named directory

The earlier objection was that naming the directory after the tool implies the tool owns the content, when decisions, guardrails and glossaries belong to the repository and outlive any tooling.

**That argument proves too much.** `.github/` holds your workflows, your issue templates and your CODEOWNERS — content you wrote, that outlives GitHub — and nobody experiences it as dispossession. `.claude/` is closer still: an assistant putting *user-authored* skills and settings in a vendor dotdir. Tool-named directories holding user-owned content is the dominant convention, not an edge case.

**What decided it: whose layout is this?** Nothing in the Luma Knowledge Format defines these tiers. The format defines documents, bundles and types, and says nothing about backlogs or records. The tier structure is entirely this organization's opinion about how a project organizes itself — so a generic name claims universality the design has not earned, while `.luma/` is simply accurate.

It also keeps the layers clean: **the format stays vendor-neutral and adoptable by anyone, while an opinionated layout sits under a name only we can claim.** Those were tangled while the directory had a generic name.

Three things follow for free. It is **collision-proof by construction**, where `.hq/` was an attractive name someone else might take — the *unclaimable* criterion satisfied by ownership rather than by picking an unappealing word. **Multiple tools coexist**, because each namespaces rather than fighting over a generic root. And `rm -rf .luma/` is a **clean uninstall**, where a generic root would require knowing which parts were ours.

**Costs, accepted:** the fractal weakens — `.hq/` was the same word at organization and project scope — and a product name is a visible dependency in the repository tree, which some organizations mind more than the thing it names.

### Why one root, hidden

**One root, not flat siblings.** The flat layout was justified almost entirely by adoption cost: add one tier now, another later, never move anything. Greenfield deletes that argument — the tiers are created together. What remains favours one root: a repository root is contested space, an agent arriving cold does one lookup rather than three and cannot half-find it, and there is one namespace to defend instead of three.

**Hidden, not visible.** The opposite call was made for a bundle's `_types/`, where hiding content meant to be read was wrong. This differs: `.luma/` is not the product. It is how the project is run, sitting beside what the project *is*, and a visible `luma/` next to `src/` reads as a source module. The dot says *infrastructure, not shipped output*, which is true here.

### Why `policy` over `standards`

**The definition covers the whole tier.** *A course or principle of action adopted by a business or individual.* That includes adopting this vocabulary rather than that one, releasing this way, never doing that, reading this first. `standards` also covers the tier, but by being vague enough not to object; `policy` covers it by being accurate.

An earlier objection — that a glossary or an architecture map cannot be policy — does not survive. Choosing a controlled vocabulary is a course of action a business adopted, and architectural invariants are obligations. Nor does the register objection: house style is routinely policy, and a newspaper's style guide is exactly that.

**It binds.** The tier holds *what an agent must never do*. Policy is something you are bound by; a standard is something you are measured against. For material whose job is to constrain behaviour, the stronger word is the honest one.

**It leaves `standard` free where nothing replaces it — and this is the deciding argument.** hq publishes **standards**; a project claims conformance to them and records exemptions. Spending `standard` on the project tier makes one word straddle both levels, which the earlier `.hq/` reasoning flagged as a risk and accepted reluctantly. Now it does not have to:

| level | word | meaning |
|---|---|---|
| organization | **standard** | an external norm, argued into existence, claimed by projects |
| project | **policy** | this project's own adopted course of action |

*Standards conformance* then reads correctly as **the project's policy about which standards it claims.**

**Why `standards` lost.** It means two opposite things — *standards we uphold* and *standard issue*, a baseline you get by default — and `starters` are exactly baselines you get by default, so the wrong reading names a real concept in the same system. It is also the weaker claim: a guardrail described as a standard sounds like a benchmark rather than a boundary. Its only advantage was being the recorded decision, which is a reason to be sure rather than a reason to keep it.

**Singular, not `policies/`.** Pluralising re-narrows the word: `policies/` implies each file *is* a policy, discrete and countable, which re-breaks the glossary. The mass-noun reading — *against policy*, *policy requires two approvals* — is what lets the word cover descriptive material at all. `records/` stays plural because a record genuinely is a discrete, dated, countable thing.

### Apply it

- Two items lose a suffix the container now supplies: *verification policy* → **verification**, *secrets policy* → **data handling**.
- Configuration stays **in the tier**, not at the root — `.luma/config/foreman.toml`, named for the tool that reads it. That part of the superseded entry survives untouched: configuration is a declaration with the same lifecycle as everything else in force.
- Generated adapters — `.claude/`, `AGENTS.md`, `CLAUDE.md` — stay outside `.luma/`, live wherever their tool looks, are generated from `policy/`, and are disposable. Nothing generated is ever the source.

### Deferred

**A `cache/` tier for derived material** — indexes, embeddings, rolled-up digests. It passes the test for its own tier (deleting it loses no decision anyone made) and would keep one `.gitignore` line instead of scattered ignores. Not taken now because nothing generates any of it yet, and a tier with no contents is a shape guessed at rather than found. *Re-open when the first derived artifact exists.*

**Re-open trigger:** if a second tool — ours or someone else's — ever needs to write into these tiers, the namespace is wrong, because the tiers would then belong to a shared convention rather than to one product.

## `.luma/` holds bundles, not policy

**Settled 2026-08-18.** Supersedes the tier list in [The project directory is `.luma/`](#the-project-directory-is-luma), hours after it was recorded. Everything that entry says about vendor-naming, one root, and hiding it stands.

```
.luma/
  backlog/            what we intend
  bundles/            what is in force — adopted, or written here
    adopted.toml      what this project took, and proof of what it looked like
    luma/git-secrets/
  config/
    foreman.toml      how a tool behaves here
  records/            what happened, and why
```

### Tiers are cut by lifecycle, then by audience

**First by lifecycle.** `backlog/` churns, `records/` is append-only, and what is in force sits between them. That separates intent from history and does nothing else.

**Then by who it is addressed to.** `bundles/` is read by anyone acting in the project; `config/` is read by one named tool — which is why configuration files are named for their reader and bundles are not. **Enforcement follows from this rather than causing it:** what everyone here must abide by is the kind of thing a check can fail on, and parameters consumed by a single tool are not.

**One axis is well tested and the other is not, and that asymmetry is the useful part.** Lifecycle separates three groups. Audience has done exactly one job — splitting `config/` off from `bundles/` — so it is the half most likely to be named wrong, and the next tier anybody proposes is the real test of it.

**Subject matter rarely creates a tier.** There is no `security/`, no `architecture/`, no `onboarding/` — each is material with an existing lifecycle *and* an existing audience, filed by what it is about instead. One such directory is harmless; the fourth makes every question *which folder does this go in* rather than *is this in force, or is it history*.

**Rarely, not never.** A topic earns a tier only by turning out to differ on one of the two axes, at which point it was never a topic tier. Generated material is the worked example: it looked like a subject, and what actually distinguished it was being machine-written and disposable. **A proposal that differs on audience deserves more attention than one that differs on lifecycle**, because that is where the model is thinnest.

**Before adding a tier, name both.** Its lifecycle, and its reader. If both match a tier already there, it is not a tier — it is content belonging in that one.

*Recovered 2026-08-21 from the superseded design entry in `IDEAS.md`, which was the only place it was written down — though it was recorded there as lifecycle alone, which could not explain why `bundles/` and `config/` are separate tiers when both are in force and both are edited. The rest of that entry's summary was either already recorded here or was a war story about wrong turns, and went with the file.*

### Why `policy/` went

**Almost everything in force arrives as a bundle.** A tier named `policy/` holding two loose documents while `bundles/` holds forty is a name pointing at the wrong thing.

The realization that forced it: bundles turned out to fit *everything* — rules, procedures, types, templates, scripts — so the tier that was supposed to hold what is in force was left holding only the residue.

**A project's own content becomes a small local bundle.** It costs four lines of manifest and buys three things: the audit rule checks it, `apply` finds its workflows, and **promotion becomes a directory copy rather than a packaging step** — which is the entire first leg of the promotion path.

*The friction is real at the smallest scale.* One house rule now needs a `bundle.md`, and that is the part worth disagreeing with later if it grates.

### Why `bundles/` and not `vendor/`

`vendor/` means *third-party, not yours*, and a project can write its own bundle — that is where promotion starts. Half the contents would make the name false.

**The ours-versus-theirs line is carried by the namespace instead**, which is more reliable than a directory:

```
.luma/bundles/luma/git-secrets/    adopted — never edit
.luma/bundles/acme-web/deploy/     ours — written here
```

And `adopted.toml` is authoritative anyway, since only adopted bundles have a source and a checksum. Putting the same fact in the path as well would be a second copy that can disagree with the first.

### `adopted.toml`, not `bundles.toml`

**`bundles.toml` is one letter from `bundle.md`**, and they are unrelated jobs — `bundle.md` is a bundle describing *itself*; this is a project recording what it *took*. Near-identical names for unrelated things read fine to whoever wrote them and confuse everyone else.

*A second reason was recorded here and has been withdrawn: that `bundles/bundles.toml` stutters. Repetition is not a defect on its own — vendor tooling often prefers it, and it earns its place when a name has to survive being read out of context.*

**It is written by the tool and never by hand.** The checksum is the point: drift-checking compares it against the vendored files to detect an edited copy, so **a hand-edited checksum makes the check silently start passing.** That value must never sit in a file anyone is invited to edit — which is also why it is not in `config/`.

*It is not a lockfile*, though it looks like one. Bundles are committed, so nothing is ever restored from it. It answers two questions only: has anyone edited this copy, and is there a newer version.

### Why `config/` is a directory

Several tools will have configuration, and an existing decision already says to **name configuration files for the tool that reads them** so they coexist without negotiating one format. A single `config.toml` would force exactly that negotiation.

A directory also keeps `.luma/` to four entries with no loose files, and **mirrors the machine-local side in shape** — `~/.config/luma/luma-foreman/config.toml` against `.luma/config/foreman.toml`. Each level names the tool exactly once: on the machine the directory does it, in the project the filename does, because there the directory is shared. *An earlier version claimed the two carried the same filename. The path move made that false; the symmetry that survives is structural rather than nominal.*

### Deferred: `policy/` may return

For content that is **definitionally never distributed** — an architecture invariant, glue describing how two adopted bundles interact here, a rule that dies when a migration lands. Wrapping something undistributable in the unit of distribution is a category error, if a cheap one.

Not taken now because **the direction is asymmetric**: adding a tier later moves nothing and breaks nothing, while removing one means migrating everything in it. It also costs no design debt to leave open — a loose document is an ordinary Document, so `apply` reads it the same way, the audit skips it for having no `bundle.md`, and drift-checking ignores it for having no checksum.

*Re-open when someone writes local content and the manifest is visibly ceremony.* Somebody complaining about writing a manifest for one sentence is the observable trigger.

**Re-open trigger for the whole layout:** nothing has adopted anything yet. The first real adoption will teach more than further reasoning will, and being wrong in the direction of fewer tiers is the cheap mistake.
