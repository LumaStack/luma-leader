---
type: finding
title: An agent working in the catalog cannot see the bundles the catalog publishes
finding_id: F-007
severity: low
location: luma-catalog
---

# F-007: An agent working in the catalog cannot see the bundles the catalog publishes

## Condition

**`luma-catalog` adopts nothing**, so `outfit` projects nothing: no skills, no
index in `CLAUDE.md`, no `routing.toml`. An agent working there is told about
none of the nineteen bundles sitting in `catalog/bundles/`, including the
policies that govern how to write them — `organizing-a-bundle`,
`writing-findings`, `create-bundle`.

## Criteria

**This is a stated and accepted consequence, not a defect discovered here.**
`the-estate` says so directly: *"An agent working in a catalog gets no generated
index and no skills, because `outfit` projects what a project adopted and a
catalog adopted nothing. It has to be pointed at `catalog/bundles/` by hand."*

**And the rule that produces it is deliberate:** *"A repository does not vendor
its own bundles into itself"* — adopting them would put two byte-identical trees
in one repository with nothing keeping them in step.

## Cause

**Projection is defined over what a project *adopted*, and publishing is a
different relationship.** The estate names the missing capability precisely:
*"Projecting what a repository publishes is the missing feature, and it is not as
simple as it looks: this catalog publishes seventeen bundles and works by about
three, so projecting all of them would be a worse problem than the duplication
it replaced."*

## Effect

**It is a live cost rather than a theoretical one.** F-004 — a published policy
teaching a removed field — sits in a bundle no agent working in that repository
is shown. **The document most in need of maintenance is the one the maintainer's
tools do not surface**, and it went two releases without anybody noticing.

**Rated low because the cause is understood and the fix is known to be
non-trivial**, not because the effect is small.

## Recommendation

**Do not adopt, and do not project everything.** Both are already argued against
in `the-estate`, and the second would be worse than the problem.

**The missing piece is the adopter's own selection** — which bundles this
repository actually works by. That is a real feature with a real design question
behind it, and it belongs in `luma-foreman`'s backlog rather than in this audit.

**In the meantime the cheap mitigation is a pointer**, since the estate policy
already says an agent "has to be pointed at `catalog/bundles/` by hand" and
nothing does the pointing. A hand-written line in the catalog's own `CLAUDE.md`
outside the generated block costs nothing and is not a substitute for the
feature.
