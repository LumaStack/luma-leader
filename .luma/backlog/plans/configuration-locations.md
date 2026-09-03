---
type: document
title: Configuration locations
description: Every candidate location for application, project, catalog and bundle configuration, what each costs, and where the argument lands — written to be chosen from rather than agreed with.
stage: draft
created: { by: agent:claude-opus-5, at: 2026-09-03T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-09-03T00:00:00Z }
---

# Configuration locations

**Draft. Nothing here is chosen.** Options against the criteria in
[[configuration-kinds]], which holds the sketch, the four kinds and the open
questions. Read that first; this does not repeat it.

**Leans are arguments, not decisions.** Where the choice is genuinely open it
says so rather than manufacturing a preference.

> Rendered as a page while it was being written. **The markdown is the
> document** — the rendering carries no content this does not, and `.luma/`
> holds prose an agent reads rather than a stylesheet.

## Cross-cutting, and they constrain the rest

### Serialization format

The sketch says YAML. Every config file in the estate is TOML today.

**What the sketch does not price in.** `lkf.py` is a deliberate 131-line subset
reading top-level `key: value` only, and its own docstring refuses to grow:
*"Anything that needs real YAML is a job that belongs somewhere else, not a
reason to grow this."*

So YAML config means one of three things, and all three cost something:

| option | cost |
| --- | --- |
| **TOML** *(lean)* | Two syntaxes in the estate — YAML frontmatter, TOML config |
| YAML with a real parser | A dependency in every tool, or a parser written per language |
| YAML in the existing subset | Every config file flat forever |

`tomllib` is standard library from Python 3.11, which is foreman's floor, and
TOML has no significant whitespace — a misindented file cannot silently change
meaning. Against it: the estate reads two syntaxes.

**The third option is already ruled out by the one config that exists.**
`[[retired]]` — with `use`, `decided`, and an `except` list carrying a comment
per entry — does not fit flat keys.

**Lean: TOML**, unless the estate wants a YAML dependency. The two-syntax cost
is paid once, by readers. The parser cost is paid by every tool, in every
language, forever.

### Organization segment

```
~/.config/luma/luma-foreman/config.toml        current
~/.config/lumastack/luma-foreman/config.toml   sketch
```

**This one has a hazard attached.** `where-configuration-lives` argues the
organization segment exists precisely so the agent deny rule
`Edit(~/.config/luma/**)` is writable once — **and states that the rule fails
open.** A pattern matching nothing produces no error and no warning, so moving
the segment without moving the rule disarms the gate silently. The first sign
would be an agent editing the permissions that govern it.

*Not verified against the installed gate. That is the bundle's claim, and it is
worth checking rather than trusting.*

Otherwise this is a naming call and either answer works. `luma` is shorter and
already deployed; `lumastack` matches the organization exactly, which is the
same argument that settled nesting. If it moves, the deny rule moves in the same
commit, and the bundle already carries the required order: install, apply the
printed settings changes, **then** delete the old directory.

**Open.**

## Application

Schema owned by the binary. One version per machine, which is what makes this
the least contested kind.

```
.luma/config/applications/luma-foreman.toml    committed
~/.config/<org>/luma-foreman/config.toml       yours, every project
~/.config/<org>/luma-foreman/projects/<id>.toml   yours, this project
```

**Take the sketch.** The directory per kind is the strongest idea in it: it
removes the flat-namespace collision entirely, so a bundle named `sessions` and
a tool named `sessions` can no longer contend for `.luma/config/sessions.toml`.

The alternative — stay flat and prefix by kind, `app-luma-foreman.toml` —
avoids a migration by replacing a directory boundary with a naming convention.
That is the weaker of the two, and **the estate has already rejected a
convention-dependent rule once**, when it dropped the `luma-*` wildcard for
exactly this reason.

The machine-local side is unchanged from today apart from the segment above.

*This makes `.luma/config/luma-foreman.toml` a migration. The estate is already
split between `foreman.toml` and `luma-foreman.toml` — worth settling in the
same move rather than carrying both through it.*

## Project

Schema owned by nobody external. No version, because the project is describing
itself.

The sketch offers `PROJECT.md` or `project.toml`. **There is a third reading,
and it describes what is already true.**

- **All in `PROJECT.md`.** Its frontmatter *already* carries `title`,
  `description` and `disclosure_level`, so this is partly the status quo rather
  than a proposal. Against it: a document written to be read acquires a second
  audience that parses it, and `[[retired]]` does not read like prose.
- **All in `project.toml`.** Clean separation. Against it: `PROJECT.md`'s
  existing frontmatter has to move or be duplicated, and identity gets split
  from the document that exists to state it.
- **Split by what it answers** *(lean)*. `PROJECT.md` keeps **identity** — what
  this project is, who may see it. `project.toml` takes **behaviour** — retired
  vocabulary, what *done* means, which checks run.

The split formalizes the line `PROJECT.md` already sits on rather than moving
anything across it.

### Two keys look misfiled today

`[[retired]]` lives in `luma-foreman.toml` but is the project declaring its own
vocabulary. It would still be true if foreman did not exist; foreman merely
enforces it.

`[catalog."<name>"]` is the same shape — the project declaring where its
knowledge comes from. **Any** tool should be able to read that, not only
foreman.

**The test that sorts them: would this still be true with no luma tool
installed?** If yes it belongs to the project, not to the application. Both are
candidates to move while the file is open.

## Catalog

Schema owned by the catalog, versioned at fetch.

**The uncertainty in the sketch is correct — no key needs this today.**

- **Reserve the path, build nothing** *(lean)*. Name `.luma/config/catalogs/` so
  nothing else claims it, and leave it empty. This is the estate's own instinct,
  from `future-hooks`: wait for *"the first real evidence about what the schema
  should hold"* rather than guessing at it.
- **One `catalogs.toml`.** Catalogs are few. A single file shows them all at
  once and matches the shape the registry already has. Diverges from the
  per-item form used for bundles.
- **One file per catalog.** Consistent with bundles, so one shape is learned
  once. Overbuilt for a set that rarely exceeds three, and a directory holding
  one file reads as an unfinished idea.

If a key does appear it will be a credential *reference*, a refresh policy, or a
trust pin — and **which of those arrives first should decide the shape.**
Meanwhile the registry moves to project config, where the test above puts it.

## Bundle

Schema owned by the bundle, versioned by a vendored copy that can be any age.
This is the kind nothing else can be copied for.

### Committed shape

```
.luma/config/bundles/<namespace>/<bundle>/config.toml   directory
.luma/config/bundles/<namespace>/<bundle>.toml          flat file
```

**Lean: flat file.** `.luma/bundles/<ns>/<name>/` is a directory because a
bundle *has many files*; its config is one, and a directory holding exactly one
file forever is ceremony. What buys the shared mental model is mirroring the
**namespace path**, which the flat form does.

The directory form's argument is room for a bundle to carry more than one config
file later. Worth taking only if that is expected.

### Untracked home

XDG says `<org>/<application>/`. **A bundle is not an application, so there is
nothing obvious to nest it under** — this is the problem the sketch spotted with
`~/.config/bundles/`.

- **Under the reading application** — `~/.config/<org>/luma-foreman/bundles/…`.
  Wrong owner. A bundle is read by agents following its procedures and
  potentially by several tools, so the settings duplicate under every tool that
  reads them, and then disagree.
- **A knowledge segment** *(lean)* — `~/.config/<org>/knowledge/bundles/<ns>/<bundle>.toml`.
  Keeps the `<org>/<segment>/` shape intact, so the deny rule still matches with
  nothing to widen, and it needs no reserved-word rule: it says what the segment
  is rather than relying on no application ever taking the name.
- **Kinds as siblings of applications** — `~/.config/<org>/bundles/…`. Shortest
  and reads naturally. Requires reserving `bundles` and `catalogs` as names no
  application may ever take, which is a convention that has to be enforced —
  the exact weakness that killed the `luma-*` wildcard.
- **An `lkf` segment** — same structure as the knowledge segment, with a name
  already in use. Against it: LKF is a format specification, not a thing that
  reads configuration, so the segment names a spec rather than an owner.

**A `knowledge/` segment is the only option needing neither a fiction nor a
reserved-word rule**, and it keeps the one invariant the deny rule depends on.

### What a fork inherits

Adopting `acme/git-workflow` after `lumastack/git-workflow`.

- **Key by namespace and name, never share.** Safest: config is keyed to a
  schema, the schema belongs to the bundle, and a fork can diverge at any time.
  Costs friction exactly where the estate has worked to remove it — forks are
  meant to be cheap.
- **Key by name only, always share.** Frictionless. Also feeds config written
  against one schema to a different one, silently — and **a fork is the case
  most likely to have changed its keys.**
- **Key by namespace and name, offer to copy** *(lean)*. Adoption notices a
  bundle of the same *name* is already configured, reports it, and offers to
  copy the values across.

The lean is the convenience of sharing without the silence, and it is the same
instinct the estate already applies to a vendored copy somebody has edited:
never silently replace their work — say what you found and let them choose.

## The shape if every lean is taken

```
committed
  .luma/PROJECT.md                              identity
  .luma/config/project.toml                     behaviour, retired words, catalog registry
  .luma/config/applications/luma-foreman.toml
  .luma/config/bundles/lumastack/luma-catalog/github-release.toml
  .luma/config/catalogs/                        reserved, empty

untracked
  ~/.config/<org>/luma-foreman/config.toml      yours, every project
  ~/.config/<org>/luma-foreman/projects/<id>.toml   yours, this project
  ~/.config/<org>/knowledge/bundles/lumastack/luma-catalog/github-release.toml
```

## What none of this settles

**These are location decisions.** Three questions from [[configuration-kinds]]
are about behaviour, and no option above touches them:

- What a reader does with a key it does not recognise.
- Whether a bundle **declares** its settings, so `inspect` can validate them and
  `bundle show` can list them.
- How precedence resolves when a bundle and an application both speak.

**The second is the one worth blocking on.** Today the only way to find a
bundle's config is to grep its prose, which is how `sessions.toml` stayed
invisible until somebody went looking for it.
