---
type: luma/idea
title: A workflow for selecting a technology stack
created: { by: human:benlinton, at: 2026-08-19T00:00:00Z }
contributors: [human:benlinton]
horizon: someday
scope: project
lifecycle: draft
---

# A workflow for selecting a technology stack

I want a workflow for selecting technology stack for a new project in my
organization.

## Notes

Migrated from `IDEAS.md` on 2026-08-20. `created.at` is a day-level estimate.

**Added at migration, not part of the original entry.** In the conversation that
produced the migration, this was tied to a **technology radar** in the
ThoughtWorks sense — a per-organization matrix placing each technology in one of
four rings: go-to, open to trial, an experiment, or to be avoided. That framing
is now reflected in `docs/scope.md`, which lists *the radar* among what an
organization's headquarters holds.

Two properties make it fit this repository's conventions unusually well. **Each
ring placement is a decision with reasoning and a re-open trigger** — *avoid,
because X; re-open if Y* — which is the shape already in use here. And it is the
clearest example available of the promotion path: **the radar is argued in an hq
and travels to foreman as an executable check**, where a project using something
in the avoid ring without an exemption becomes a finding.

**The workflow is built here; an organization's actual radar is not.** Which
technologies a particular organization endorses is that organization's content
and belongs in its own headquarters — it reveals internal capability and
direction. The capability is not sensitive; its output is.
