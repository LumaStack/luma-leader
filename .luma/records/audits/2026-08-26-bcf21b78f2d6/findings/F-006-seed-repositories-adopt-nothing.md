---
type: finding
title: Two repositories in the estate have no .luma/ and are outside every check
finding_id: F-006
severity: low
location: luma-clarify, luma-backlog
---

# F-006: Two repositories in the estate have no `.luma/` and are outside every check

## Condition

**`luma-clarify` (41 markdown files) and `luma-backlog` (36) have no `.luma/`
directory.** They adopt nothing, project nothing, and `luma-foreman inspect`
reports *"2 check(s) that ran; 3 could not run"* in both — the adoption,
projection and layout checks all skip.

Both were last touched on 2026-08-23, before the `preload`, `compliance` and
`matches` changes, so neither has been near the migrations that swept the rest
of the estate.

## Criteria

**`the-estate` names both as full members** — six repositories, each defending a
boundary. Nothing in it marks them as exempt from the practices the others
follow.

**A skipped check is not a pass**, which `inspect` says in its own output every
time it runs.

## Cause

**Both are seeds.** `luma-leader`'s README says of itself *"Status: seed. Nothing
here yet but the shape of the job"*, and these two are at a similar stage. Nobody
adopted into them because there was nothing to govern yet.

## Effect

**Low, and stated as low deliberately.** Two repositories with no dependents and
almost no content are not costing anybody anything today.

**What it costs later is the reason to record it at all.** Every migration that
sweeps the estate will miss them silently — they contain markdown that no check
reads — and the gap widens with each one. The `matches` migration already passed
them by, and nobody noticed because nothing reported it.

## Recommendation

**Do not adopt anything into them yet.** Adopting bundles into a repository with
nothing to govern is the cost `adopt-knowledge` warns about: *"the cost of a
bundle nobody needed is paid in every session."*

**Record that they are deliberately outside**, so the next estate-wide sweep
excludes them by decision rather than by accident. **The distinction this whole
audit turns on applies here too: *excluded* and *forgotten* look identical from
the outside.**
