---
type: document
title: Configuration kinds
description: Application, project, catalog and bundle configuration are four different things sharing one directory and one set of rules written for the first of them. The sketch, the open questions, and what any answer has to survive.
stage: draft
created: { by: human:benlinton, at: 2026-09-03T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-09-03T00:00:00Z }
---

# Configuration kinds

**Draft. Nothing here is settled, and the options have not been written up
yet** — that is the next step, and it is deliberately not started here. This
file exists so the starting point is not lost.

Raised while cutting `luma-foreman` v0.1.0. A release-notes policy needed a
setting somebody could override, and there was no rule saying where a *bundle's*
setting goes — only rules written for a tool's.

## What the current model settles, and what it does not

[[decision-ideas]] settled it on 2026-08-17 with the words *"two kinds of
configuration were being conflated"* — **declarations** versus **machine-local
settings**. The `luma-config` bundle carries that: `.luma/config/` versus
`~/.config/`, the precedence chain, and the `[defaults]`/`[require]` split.

**That axis is who owns the machine. It is settled and it is not the problem.**

The unsettled axis is **who owns the meaning of a key** — who defines it, who
versions it, and what a reader does with a key they do not recognise. Four
pieces of evidence that nothing answers this:

- **The naming rule is already false.** `where-configuration-lives` says *"name
  files for the tool that reads them"*, but two shipped bundles reference config
  no tool reads: `.luma/config/decision-records.toml` and
  `.luma/config/sessions.toml`. Both are named for a bundle's domain. Practice
  ran ahead of the rule.
- **The two bundle configs disagree with each other.** `decision-records` uses
  `[require]` and says *"absent means this procedure does not run"* — an
  explicit refusal to default. `session-manager` says *"the defaults carry every
  case"* and falls through. Same catalog, same release, opposite rules.
- **`luma-foreman.toml` holds two kinds today.** `[catalog] source` is a tool
  setting — foreman defines it, foreman reads it. `[[retired]]` is the project
  declaring its own vocabulary; it would still be true if foreman did not
  exist. One file, no marking.
- **A bundle cannot declare that it has settings.** No manifest field, no
  schema. Its config is discovered by reading its prose.

**`.luma/config/` is also a flat namespace**, so a bundle named `sessions` and a
future tool named `sessions` collide with nothing to arbitrate.

## The kinds

| kind | schema owned by | version of the schema |
| --- | --- | --- |
| application | the binary | the installed binary — one per machine |
| project | nobody external | none; the project describes itself |
| catalog | the catalog | the catalog, at fetch time |
| bundle | the bundle | **the vendored copy, which may be any age** |

**The bundle row is the hard one and the reason this cannot copy the
application shape.** Application config assumes one version — whatever is
installed. A project holds many bundles at many versions at once, each defining
its own keys, each a copy that can be arbitrarily old. It is the only kind whose
schema version varies per key-namespace *within a single project*.

## The sketch

Proposed by `human:benlinton`, recorded as given.

```
# application  (luma-foreman, luma-backlog, ...)
committed    .luma/config/applications/<application>.yaml
untracked    ~/.config/lumastack/luma-foreman/config.yaml

# project
committed    .luma/PROJECT.md  ...or  .luma/config/project.yaml
untracked    ~/.config/lumastack/luma-foreman/projects/<id>.yaml

# catalog   (may not be needed yet)
committed    .luma/PROJECT.md
             .luma/config/catalogs.yaml
             .luma/config/catalogs/<namespace+id>.yaml
untracked    ~/.config/lumastack/<catalog-name>/config.yaml
             ~/.config/lkf/catalogs/<catalog_id>/config.yaml

# bundle
committed    .luma/config/bundles/<catalog_namespace_or_local>/<bundle_id>/config.yaml
untracked    ~/.config/bundles/<catalog_namespace_or_local>/<bundle_id>/config.yaml
             ~/.config/lkf/bundles/<catalog_or_local>/<bundle>/config.yaml
```

**What the sketch gets right.** A directory per kind removes the flat-namespace
collision. Keying bundle config by namespace and bundle id mirrors
`.luma/bundles/<namespace>/<name>/`, so the config path parallels the content
path and there is one shape to learn rather than two.

## What is open

**Named by the author.**

- **`~/.config/bundles/` is not namespaced under the organization** and would
  collide with any other tool. Flagged in the sketch itself. The tension is real
  rather than cosmetic: the machine-local tree is organised by *application*,
  because XDG says `<org>/<application>/` — and a bundle is not an application.
  There is nothing obvious to nest it under.
- **Whether a fork shares its parent's config.** Adopting `acme/git-workflow`
  after `lumastack/git-workflow`: same config or a fresh one? Sharing saves
  reconfiguration; not sharing avoids feeding config written for one schema to
  another, which is the versioning problem above arriving quietly.
- **Whether catalog config is needed at all yet.**
- **Project config in `PROJECT.md` or its own file.** `PROJECT.md` is prose with
  frontmatter, written to be read. `[[retired]]` — with `use`, `decided`, and an
  `except` list carrying a comment per entry — is the test case, and it does not
  read like prose.

**Not named, and both are changes rather than choices.**

- **The sketch is YAML; every config file in the estate today is TOML.** LKF
  frontmatter is YAML, so YAML would mean one syntax across knowledge and
  config. Worth deciding out loud rather than inheriting from a sketch.
- **The sketch says `~/.config/lumastack/`; the current path is
  `~/.config/luma/`.** This one has a hazard attached: `where-configuration-lives`
  argues the organization segment is what makes the agent deny rule
  `Edit(~/.config/luma/**)` writable once, **and says that rule fails open** — a
  pattern matching nothing produces no error. Moving the segment without moving
  the rule disarms it silently. *Not yet verified against foreman's installed
  gate; the claim above is from the bundle.*

## What any answer has to survive

Criteria before options, so the options are judged rather than compared on
taste.

- **A key must be readable without any luma tool installed.** `decision-records`
  already refuses to depend on `luma-config` for exactly this reason. A bundle is
  prose an agent follows; if reading its settings needed a binary, the bundle
  would only work where that binary exists.
- **A reader must know what to do with a key it does not recognise** — because a
  vendored bundle and its config will disagree eventually. Ignore-and-report has
  precedent: receipts already ignore unknown sublines on read.
- **A bundle should declare its settings**, so `inspect` can validate them and
  `bundle show` can list them. Today the only way to find a bundle's config is to
  grep its prose. This is absent from the sketch and is the strongest available
  future-proofing.
- **Precedence has to be defined across kinds, not only within one.** The
  existing chain orders committed against machine-local. It does not say what
  happens when a bundle and an application both speak.
- **One path shape, learned once.** The committed and untracked trees should not
  need separate mental models.

## Next

Write up the options for each kind against those criteria, then choose. Nothing
above is a decision, and the sketch is a starting point rather than a proposal.
