---
type: luma/idea
title: Three frontmatter parsers, no reference implementation, no shared tests
created: { by: agent:claude-opus-5, at: 2026-08-23T00:00:00Z }
contributors: [agent:claude-opus-5, human:benlinton]
horizon: next
scope: organization
stage: draft
---

# Three frontmatter parsers, and nothing makes them agree

**A finding.** Raised as *two*; there are three, and they differ in kind rather
than merely in implementation.

| | what it is | fidelity |
| --- | --- | --- |
| `luma-foreman` | `inspect/rules/bundles.py`, Python | **a deliberate subset** — *"top-level `key: value` and enough of the block syntax to catch the nested-array trap. Anything needing real YAML is a check that belongs somewhere else."* |
| `luma-clarify` | `src/clarify/yamlio.py`, Python | **full YAML, round-trip** — `ruamel.yaml` in round-trip mode, deliberately not PyYAML, centralised so everything passes through one module |
| `luma-backlog` | Go, across `config`, `record` and `cli` | **unassessed** |

**None is a reference implementation, and no test tells any of them they
disagree.**

## Why this is worse than three copies of the same thing

**They are at three different levels of fidelity, so they will disagree about
real documents rather than about corner cases.** The clearest example is the
format's own most likely defect — an unquoted wikilink in frontmatter, which is
YAML flow-sequence syntax:

```yaml
superseded_by: [[ADR-0012-something]]
```

`foreman`'s subset exists **specifically to catch that** and reports it. A real
YAML parser does the correct thing instead — silently produces a nested list —
and never flags anything. **Both are behaving properly and they reach opposite
conclusions about the same file.**

## The part that is a conformance problem rather than a tidiness one

§4 places a **MUST** on consumers:

> Consumers **MUST preserve unrecognized keys when rewriting a file**, and MUST
> NOT reject a file for containing them.

**`luma-clarify` has clearly solved this** — round-trip mode is exactly what makes
preservation possible, and it says so. **Whether the other two honour it is
unknown**, and there is no way to find out short of reading the code.

That is a stated requirement of the format with three independent implementations
and nothing checking any of them.

## What would fix it, and what would not

**Not a shared library.** The implementations are in two languages, so there is
nothing to share — and *no shared package until two real consumers exist* applies
to the Python pair regardless.

**A conformance suite is the language-neutral answer**, and it is what formats
normally ship: a directory of fixture documents with expected parse results, each
implementation running it in its own language. Unquoted wikilinks, unknown keys
surviving a rewrite, absent-versus-empty, an `actor_event` in flow style, a
`stage` nobody declared.

**It also has a home already implied.** The format's roadmap makes *"it has been
exercised, more than once"* a condition for `v0.1.0` and warns that **resolution
fails quietly, because the wrong definition is still a definition**. A fixture
suite is how that stops being a hope.

## What it costs to leave

**Nothing visible, which is the problem.** Parsers that disagree do not error —
one tool reports a defect, another reports a clean file, and whoever is holding
both concludes one of them is broken. Nothing surfaces that they simply read
different subsets of the same grammar.

**Cheap now**: three implementations, none with users, one of which has already
worked out the hard part and could seed the fixtures. Expensive after any of them
does, because then the fixtures have to be written to match whatever each already
does rather than to say what is correct.

## Related

[[two-namespacing-conventions]] — same root cause, same window. Two conventions
arrived at independently because nothing said which was right, and the same
fixture suite would settle how a namespaced type name is written as a side
effect.
