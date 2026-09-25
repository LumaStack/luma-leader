---
type: work-item
type_version: "0.0.2"
key: LEAD-0045
title: 'No competitor names in committed docs'
description: 'Committed luma-backlog output must never name other projects/competitors; positioning goes in as unattributed prior art.'
workflow_status: captured
rank: 010.0450.000
kind: idea
stage: draft
created: {by: 'human:benlinton', at: '2026-08-30T21:24:09-05:00'}
# created derived from the file's first commit — the source had no frontmatter
---

# No competitor names in committed docs

> **Migrated from `.luma/backlog/ideas/` — read this first.** This was not an idea record. It arrived in the corpus shaped as an agent-memory file — `name`, `description`, `metadata.type` — and its body is reproduced verbatim below. **I am not sure this belongs in the backlog as a work item**; it reads as a standing convention rather than work to do, and may want to live in agent memory or as a project convention. It is here as an idea so that decision gets made rather than lost.

No file committed to `luma-backlog` may mention another project by name — not in docs, comments, commit messages, or the README. This includes competing or adjacent tools discovered during research.

**Why:** Stated as a standing rule for the project's final output (2026-08-04). Naming competitors in a specification repository dates the docs, invites comparison-shopping by readers, and reads as defensive positioning rather than a confident statement of what the project is.

**How to apply:** Research prior art freely and discuss it by name in conversation — the constraint is on committed output only. When a competitive insight needs to survive into the docs, write it as an unattributed design position ("the substrate is an auditable record, not a task list") rather than a contrast ("unlike X, we ..."). See [[work-items/LEAD-0044-design-first-working-mode]].

**Carve-out (2026-08-09, revised 2026-08-20):** the rule does not apply in an organization's internal headquarters. Holding competitive analysis is part of what that repository is for, and its audience is the organization rather than the public.

**The exemption ends at promotion.** An insight travelling outward — into a bundle, into a catalog, into anything published — is rewritten as an unattributed design position first, and the copy to inspect is the promoted one. *This carve-out previously named one specific repository as the exempt one, and that repository has since been published — which is exactly why an exemption has to rest on a stated property rather than on a name.*

## Related work

- Born from the idea it replaces: [`no-competitor-names-in-committed-docs`](../../ideas/no-competitor-names-in-committed-docs.md)
