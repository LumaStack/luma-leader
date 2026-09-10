---
type: luma/idea
title: Rethinking the luma-prefixed bundles
created: { by: human:luma-foundry, at: 2026-09-10T07:20:00Z }
contributors: [human:luma-foundry, agent:claude-opus-5]
scope: organization
stage: draft
---

# Rethinking the luma-prefixed bundles

**Rethinking the luma-prefixed bundles — what do we actually need?**

**What we have so far is not well thought out and will need to be reworked. It
is all experimental so far.** See `luma-catalog/catalog/bundles` for the current
set.

---

## As proposed

*Kept as written — spelling corrected, wording untouched.*

I think we will need shared types. But whether it should run in its own bundle
or not, I'm not sure yet.

I can't prove it, but I think we will need:

- **luma-user** — stuff to help users navigate what users need to know to use
  and install the tools. Probably, maybe not. Maybe this is called
  **luma-ecosystem** and it's what you need to embrace the full ecosystem of
  tools, if you want to have everything integrate together.
- **luma-maintainer** — has to be additive to luma-user, never contradicting or
  a replacement.
- **luma-core** — stuff that is always present, what you would need to use a
  single tool.
- **luma-essentials** — the bare minimum?
- **luma-types** — these are shared type definitions or schemas, that can't live
  in a single bundle because many bundles reference them.

---

## Status

**Captured mid-discussion, deliberately before any commentary.** The thinking-
through was requested and is still in progress; findings and analysis get
appended below this line once there is something worth keeping.
