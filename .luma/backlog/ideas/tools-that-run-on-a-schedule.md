---
type: luma/idea
title: Tools that run on a schedule
created: { by: human:benlinton, at: 2026-08-19T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: someday
scope: project
lifecycle_status: draft
modified: { by: agent:claude-opus-5, at: 2026-08-21T00:00:00Z }
---

# Tools that run on a schedule

Or better yet the foreman or hq sweeper should run every day/week and look up
what happens on a schedule and make sure we're doing it.

This basically introduces a new concept that some portion of our tools need to be
able to run on a schedule, so they can rover, sweep, audit, and govern policy and
also eliminate rot.

## Notes

Migrated from `IDEAS.md` on 2026-08-21. `created.at` is a day-level estimate.

**Split at migration** from [a survey of how an organization is
divided](organization-division-survey.md), which is where the observation
originally appeared. It was separated because it is independently valuable and
independently buildable — the survey works without it as a quarterly ceremony,
and a scheduler serves things that have nothing to do with the survey.

**Several existing decisions already want this and have no way to get it.** Drift
between a vendored bundle and its catalog is informational and nobody is
watching. `by:` dates on obligations pass silently until something happens to
run. `verify-headquarters` exists precisely because visibility can change with no
announcement, and it only helps if somebody remembers to run it. And foreman's
`refit` — returning to a project to confirm the latest learnings were actually
applied — is a recurring job by definition, currently a stub with no trigger.
Each of those is a recurring check with no recurrence.

**Absorbed on 2026-08-21** from `luma-foreman/docs/IDEAS.md`, the one-line entry
*"Return periodically to confirm the latest learnings were actually applied."*
Only `refit` as a fourth recurring consumer was new; the rest was already here in
different words. The foreman entry was pruned rather than filed.

**Which tool owns it is open.** The entry says *foreman or hq sweeper*, and both
have a claim: foreman runs inside projects where most checks apply, while hq is
where organization-wide sweeps would start. It is kept here because an unowned
cross-cutting capability is argued at the organization level before anybody
builds it.
