---
type: luma/idea
title: Departments, and how not to foreclose them
created: { by: human:benlinton, at: 2026-08-19T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: someday
scope: project
lifecycle_status: draft
---

# Departments, and how not to foreclose them

**Draft — not settled, not reviewed.** Recorded so the options survive, not to
pick one.

Some organizations have departments, and nothing here decides what that means. An
earlier pass assumed a department runs its own hq and catalog, which was already
too prescriptive — that is one shape among several, and the least tested.

**Shapes worth keeping on the table:**

- **A department is an organization.** Its own hq and catalog, with `upstream`
  chaining universal ← company ← department. Needs nothing new on the catalog
  side. The gap would be the hq, which has no upstream and no promotion path, so
  company-wide knowledge would be duplicated and drift.
- **A department is a tag on content.** One hq, one catalog, and department
  becomes a dimension of the material inside them. Tags already exist as a
  mechanism, projects already declare them, and this adds no repositories at all.
  Probably the cheaper shape, and it is not obvious what it fails at.
- **Something else entirely.** Neither has met a real organization.

**What is already true — as fact, not as an answer.** A catalog's `upstream` is
single-valued and acyclic, so a three-link chain works today with no change.
Obligations resolve most-restrictive-wins, which is associative and therefore
composes over any chain length. Neither was built for departments; both happen to
accommodate one shape of them.

**Constraints that keep every shape open**, worth holding regardless of which
wins:

- `upstream` stays single-valued and acyclic.
- Obligations stay most-restrictive-wins.
- Origin stays derived from location rather than declared.

**Unchecked:** starter `extends` / `adds` / `excludes` was reasoned about across
two catalogs. Over three links, a department-level `excludes` against a
universally-defined starter may be order-dependent, and order-dependence was only
ever ruled out for the two-catalog case. Worth verifying before a third link
exists under any shape.

**Why not now:** nothing has adopted anything. One real organization with real
departments will settle this faster than further reasoning will.

## Notes

Migrated from `IDEAS.md` on 2026-08-21. `created.at` is a day-level estimate.

**Kept in this repository** because the unbuilt part is the hq side: knowledge
has no `upstream` and no promotion path, while the catalog half already works
without change. If the answer turns out to be *department is a tag*, the work
moves to the catalog and this idea moves with it.
