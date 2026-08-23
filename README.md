# luma-leader

> **Your organization, as code.**<br>
> With governance you can prove.

A thinking partner, a standardization engine, and everything your organization knows — all turning decisions into reality in days, not quarters.

It is where an organization decides what good looks like and why: what each project owns, what standards it holds itself to, what it has already learned, and what to build next. It ships a starting set of standards and examples that most organizations can adopt as they are, and it expects to be extended, overridden, or largely replaced.

It is the place you go to plan, not a tool that runs anywhere.

> **Status:** seed. Nothing here yet but the shape of the job.

## The three repositories

- **`lumastack/luma-leader`** — this repository. The general engine: the shape of the job, the conventions for arguing a standard into existence, and defaults worth starting from.
- **`lumastack/luma-foreman`** — where standards become executable, one repository at a time. Universal workflows and checks with sensible defaults, flexible enough that an organization taking a completely different approach can still use it.
- **Your organization's own hq** — an internal repository holding what is specific to you: your standards, your boundaries, your learnings, your competitive analysis. Its location is configuration; this repository does not name it.

The split is the same one that separates hq from foreman, applied again: what is general is shared, what is yours stays yours. A pattern that would help any organization belongs upstream here. Anything naming your customers, your competitors, your internal systems, or your people stays in your own hq.

Not to be confused with `.luma/` — the per-project directory holding a single repository's backlog, policy, and records. The metaphor is deliberately fractal: an organization has a headquarters, and so does each project.

## What it is responsible for

- **Project boundaries.** What each repository owns, what it must not own, and where two projects are about to collide.
- **Standards.** What a well-formed project looks like, and — more importantly — *why* each standard exists and what would justify dropping it.
- **Learnings.** What has been discovered once and should never be rediscovered. Settled positions, and the reasoning that settled them.
- **What to build next.** Which repository the organization needs, why, and in what order.

Where a project's own goals are recorded in its `.luma/backlog/`, they are derived from there rather than restated here.

## Relationship to luma-foreman

Two halves of one job, split because they run in different places.

- **luma-leader decides what good looks like, and why.** It runs where your organization's knowledge is — in your own headquarters, alongside the decisions it helps you argue into existence.
- **[luma-foreman](https://github.com/LumaStack/luma-foreman) makes it true, repository by repository.** It runs where that knowledge is not: inside a project, in continuous integration, in a checkout with no access to any of it.

The consequence: standards are argued and settled here, and travel to foreman as executable checks. Foreman never reads this repository at runtime. If a check ever needs organization context to run, the boundary has been broken.

## Extending it

Nothing here is a mandate. The standards this repository ships are defaults — argued, with their reasoning attached, so you can disagree with them on the merits rather than guessing at intent. Adopt them, edit them, or replace them wholesale; the conventions for recording *why* matter more than the specific answers.

## Conventions

- Record a path not taken as **deferred with a re-open trigger**, never as rejected.
- Never abbreviate terminology to initials in prose. **The one exception is `-hq`**, the suffix reserved for an organization's own headquarters — `acme-hq`. This repository no longer needs the carve-out, since `luma-leader` is spelled out.
- **`acme` is the example organization, everywhere.** One name across every document means a reader recognizes an example on sight instead of wondering whether `your-org` and `example-co` are the same thing.
- A standard without its reasoning is not finished. The answer is perishable; the argument is what survives.
