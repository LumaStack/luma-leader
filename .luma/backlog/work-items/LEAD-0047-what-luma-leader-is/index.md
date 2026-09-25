---
type: work-item
type_version: "0.0.2"
key: LEAD-0047
title: 'What luma-leader is'
description: 'luma-leader is the engine. acme-hq is your organization''s internal data. The engine is general, shared, and the same for every organization.'
workflow_status: captured
rank: 010.0470.000
kind: idea
stage: draft
created: {by: 'human:benlinton', at: '2026-08-30T21:24:09-05:00'}
# created derived from the file's first commit — the source had no frontmatter
---

# What luma-leader is

> **Migrated from `.luma/backlog/ideas/` — read this first.** This was not an idea record — it had no frontmatter at all, so there is no author or date to preserve beyond the day it was first committed, and the title is derived from the file rather than declared. Its body is reproduced verbatim below. **I am not sure this belongs in the backlog as one work item**: it is a collection covering several separable things, and probably wants splitting. It is here as an idea so that decision gets made rather than lost.

**luma-leader is the engine. `acme-hq` is your organization's internal data.**

The engine is general, shared, and the same for every organization. The knowledge is internal, specific, and yours — your projects, your strategies, your trade secrets, your custom logic. The engine reads and writes that knowledge. It never contains it.

## The test

An organization's hq holds **what no single project can know**.

That sentence is a test, not a description. If one project could know it on its own, it does not belong at the organization level.

## What lives in your organization's hq

- **The map** — where all project overview lives, what each does, why it exists, what it solves, where its edges are.
- **The relationships** — which projects are about to collide, and what must not be built twice.
- **The radar** — which technologies are go-to, which are open to trial, which are experiments, and which are to be avoided.
- **The strategies** — designed, drafted, reviewed, steered, approved, distributed.
- **The mandates** — what projects must adopt, and by when.
- **The architecture, security, and compliance calls** — organization-level decisions about how projects are shaped.
- **New project logic** — feeding complete organization context into the decision to start something, so a new project is defined against everything already known rather than in isolation.
- **Whatever else is yours** — custom logic, custom code, any organizatonal need that is not meant for public eyes.

## What the engine provides

- **The method** — how to argue a standard into existence and record it so that it survives being argued once.
- **The tooling** — commands that curate the radar, draft and review a strategy, generate the project index, and promote what has earned it.
- **The universal catalog** — standards and strategies that any organization can adopt as a starting point, edit, or ignore.
- **The conventions** — the decision format, deferring rather than rejecting, re-open triggers, and the rule that a standard without its reasoning is not finished.

## What the engine is not

- **Not the knowledge.** Nothing organization-specific lives here. It lives in your own hq, which is internal.
- **Not the executor.** The engine acts on organization context — this repository and the internal organization repository it pairs with. Acting on a project is foreman's job, permanently.
- **Not the owner of any project's own facts.** A project's purpose, goals, and backlog belong to that project. An hq references them; it never restates them.
- **Not the project catalog.** Foreman owns the project catalog. An organization's hq owns the organization catalog.
- **Not a task tracker, and not documentation for consumers.**

## Held, not restated

The map is the part most likely to rot, because a hand-maintained list of what every project does is stale within a quarter. The rule that prevents it:

**Hold only what cannot be derived.**

- **Derived** — what a project does, why it exists, its goals, its status. Each project describes itself in its own `.luma/`, and the engine generates an index from those descriptions. A project's self-description going stale is then a local problem with a local owner, rather than an organization-level chore nobody does.
- **Held** — relationships, boundaries, mandates, the radar, strategies. No single project can know these, so they are maintained by hand. That set is small, which is exactly why it stays current.

What remains hand-maintained at the organization level is close to a single list of repositories. Everything else about a project is generated from the project.

Staleness then becomes enforceable rather than aspirational: foreman already runs inside every project, so it can check that a project's self-description exists, is current, and has not drifted from what the repository actually does. Rot becomes a check that fails.

## Where the line with foreman falls

Two halves of one job, split because they run in different places.

- **The engine runs where organization knowledge is available.** You are in your own hq, thinking.
- **[luma-foreman](https://github.com/LumaStack/luma-foreman) runs where it is not** — inside a project, in continuous integration, in a repository with no access to organization knowledge at all.

Standards are argued and settled in an hq, and travel to foreman as executable checks. Foreman never reads an hq at runtime. If a check ever needs organization context in order to run, the boundary has been broken.

## Related work

- Born from the idea it replaces: [`scope-ideas`](../../ideas/scope-ideas.md)
