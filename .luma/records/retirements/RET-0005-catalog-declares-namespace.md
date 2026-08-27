---
type: retirement
retirement_id: RET-0005
retired_at: 2026-08-26T23:52:31Z
origin: project
scope: estate
scope_evidence: "probed the seven repositories; the design draft in luma-leader was built on the old model and every bundle ID in the catalog carried the old form"
decided_in: "lumastack/luma-catalog#PR-93"
was: "a catalog **declares** the namespace it publishes bundles under"
now: "the namespace **derives from where the catalog lives** — github.com/LumaStack/luma-catalog publishes `lumastack/luma-catalog/<name>`. A `CATALOG.md` declaration still wins where one exists"
why: "a fork gets its own namespace without anybody arranging it, and can no longer publish under the name of the catalog it forked"
recognizers:
  - kind: shape
    value: "a bundle ID of the form `luma/<name>` rather than `<org>/<repo>/<name>`"
  - kind: claim
    value: "any document stating that a catalog declares, sets, chooses or owns its namespace as a thing it writes down"
except:
  - ".luma/records/"
lifecycle_status: stable
---

# RET-0005: a catalog declares its namespace

## What it was

The namespace was a **declaration**: a catalog wrote down the prefix it published
under, and adopters took bundles as `luma/<name>`.

## What replaced it

It **derives from where the catalog lives**. `github.com/LumaStack/luma-catalog`
publishes `lumastack/luma-catalog/<name>`. A `CATALOG.md` declaration still wins
where one exists, but nothing needs to declare anything for the common case.

## How to recognize it

**This is the entry with no retired word in it.** "Namespace" is current
vocabulary and appears on both sides of the change, so a term search finds
nothing at all.

What to look for instead: the *verb*. Prose saying a catalog **declares**, sets,
chooses or owns its namespace is asserting the old model. The one in
`luma-leader`'s `catalog-namespaces.md` read *"a catalog declares `namespace` —
the default local alias for everything it publishes"*, and no search would have
surfaced it.

The bundle-ID shape is the mechanical half and catches the citations.

## Where a hit is correct

Version histories, and any document explicitly recording the change from declared
to derived.
