---
type: luma/idea
title: Two type-namespacing conventions exist and neither knows about the other
created: { by: agent:claude-opus-5, at: 2026-08-23T00:00:00Z }
contributors: [agent:claude-opus-5, human:benlinton]
horizon: next
scope: organization
stage: draft
---

# Two type-namespacing conventions, neither aware of the other

**A finding rather than a proposal.** Surfaced three times during the session
that shipped the second convention, and never written down until asked for.

## The two

**Written out in full** — what the knowledge format's §10.4 recommends and what
`luma-catalog` shipped:

```yaml
type: luma/project
```

**Declared once and abbreviated** — what `luma-backlog` already does:

```yaml
# .backlog/config.yml
type_namespace: luma/backlog   # records write short type names; this resolves them
```

```yaml
# a record
type: outcome
```

with definitions under `.backlog/_types/luma/backlog/outcome.md`.

**Neither repository knows the other exists**, and both were written in the same
week.

## Why this is worth a real comparison rather than a correction

**The abbreviated form has genuine advantages.** Records stay short and
readable; the namespace is stated once instead of repeated in every file; and
moving a bundle to a different namespace is a one-line config edit rather than an
edit to every document in it. It is the shape most markup languages settled on —
declare a prefix, use the short name.

**The written-out form has one advantage that may outweigh all of that: a
Document says what it is, anywhere.** Copy it out of its bundle, receive it over
a wire, find it in a search index, and `type: luma/project` still identifies
itself. `type: outcome` does not — it needs a config file that may not have
travelled with it, and two records from different namespaces both saying
`type: outcome` are indistinguishable once separated from their context.

**That is not a small point given everything else the format believes.** *A
Bundle is self-contained; nothing is fetched in order to read it; the file is the
contract; there is no remote lookup.* A type name resolvable only by consulting a
second file is the same shape as the search paths and environment variables the
format has already rejected — the meaning of a Document depending on where it is
read rather than on what it says.

**But the counter is real too.** The config file is inside the same bundle, so
nothing is *fetched*; it is one more local file, which is what `_types/` already
is. If that holds, the objection above proves too much.

## What it costs to leave alone

**Nothing today, and that will not last.** The two vocabularies do not currently
meet — `luma-backlog` has its own types and consumes none of the catalog's. The
moment either reads the other's documents, they disagree about what a type name
is.

**The cheap moment is now.** `luma-backlog` has no adopters and the catalog's
namespaced types are days old. Whichever way it resolves, one side rewrites a
handful of files. After adoption it is a migration on both sides plus a
compatibility window.

## What resolving it looks like

Three outcomes, and they land in different places:

- **Written-out wins.** `luma-backlog` drops `type_namespace` and writes full
  names. No format change; one tool changes.
- **Declared-prefix wins.** The format gains a way to declare a namespace for a
  Bundle and short names resolve against it. That is an LKF proposal, and it
  needs an answer for what a Document means once separated from its Bundle.
- **Both are legal.** The format permits either and says how a consumer tells
  them apart. Cheapest to agree and the worst to live with — every reader has to
  handle both forever.

**Whoever picks this up should read `luma-backlog`'s design before assuming ours
is right.** It was arrived at independently by somebody solving the same problem,
which is evidence rather than noise.

## Related

**A second divergence of the same kind, worth checking at the same time:** there
are two partial LKF frontmatter parsers in the estate — one Python, one Go —
neither a reference implementation and nothing making them agree. Same root
cause, same cheap-now-expensive-later shape.
