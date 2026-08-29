---
type: luma/idea
title: A bundle-template-maker bundle — what a bundle template is, and what stops it rotting
created: { by: human:benlinton, at: 2026-08-29T00:00:00Z }
contributors: [human:benlinton, agent:claude-opus-5]
horizon: later
scope: organization
lifecycle: draft
---

# A `bundle-template-maker` bundle

**Twelve bundles ship templates and nothing says how to write one.** They are
the most-copied artifact in the estate and the least specified: no contract, no
conventions, no statement of what a template is for.

## The name says which templates

**`bundle-template-maker`, not `template-maker`.** A bundle template is a
specific thing — a file under a bundle's `templates/`, copied when creating a
document or shaping a message — and *template* alone is one of the most
overloaded words available.

**The estate already has another kind.** [[persona-templates]] proposes
templates for organization personas, which are content rather than an artifact
kind, and nothing about this bundle applies to them. **Any project adopting
this will also have its own templates** — issue templates, pull request
templates, scaffolding — and none of those are its business either.

**The prefix costs a word and removes the whole class of confusion.**

## What is observably missing

**A template never says where its output goes.** Some are copied into a file —
a charter, an idea, a decision record. Some are message shapes, spoken to a
reader and never written anywhere — a file presentation, a slice close, an idea
review, a decision review.

**Nothing marks the difference and a reader can only tell by opening one** and
noticing whether it describes a document or a conversation. **Frontmatter would
say it in a word**, and today no template in the catalog carries frontmatter at
all.

**Two values cover everything that exists** — written to disk, or shown to a
reader. **Resist inventing more** until something needs them.

## The failure that actually costs something

**A template is a second copy of a type**, and the copy goes stale silently.
`templates/charter.md` shows the frontmatter that `_types/sweep.md` defines; when
the type gains a field, nothing makes the template gain it.

**This is not hypothetical.** In `review-sweeps`, a workflow went on instructing
readers to write `reviewed` and `approved` as status values **four releases
after the type had replaced them with three columns.** It was found by reading,
not by any check. A template has exactly the same exposure and more copies of it.

**So the interesting content is not conventions, it is the relationship**:
which type a template instantiates, and what keeps them in step.
[[bundle-declares-the-types-it-uses]] is the same question from the bundle's
side.

## What it holds

**A workflow that makes one.** Templates are currently written by copying a
neighbouring template and editing it, which is how conventions spread and also
how mistakes do. **The estate has a workflow for creating a bundle, a record, an
idea — and none for creating the artifact all of those are copied from.**

**Policy on how a template is formatted.** Fenced block or bare prose,
placeholders or worked examples, how much argument belongs in it before it is
duplicating its policy, what the file is called.

**Policy on frontmatter, which is the hard part** — see below.

**And a template for templates**, which is the demonstration rather than a joke:
if the conventions cannot be shown in one, they are not conventions.

## The frontmatter problem is why none of them have any

**A template contains two frontmatters and only one of them travels.**

| | belongs to | goes where |
| --- | --- | --- |
| the **outer** block | the template itself — what it is for, disk or display, which type it instantiates | **nowhere.** Stripped on use |
| the **inner** block, inside the copied region | the document being created — its `type`, `title`, and the rest | **into the new file** |

**Put an outer block at the top of a file whose whole purpose is to show a
frontmatter block, and a reader has two candidates and no marker.** That is a
real hazard and it is the most likely reason twenty-five templates carry none:
**avoiding the ambiguity by avoiding the field.**

**So the bundle has to solve it rather than mandate the field and move on.**
Options exist — an explicit copy region, a naming convention, keeping the
template's own metadata out of frontmatter entirely — and picking one is most of
the work. **The convention every template already states in prose — *copy the
block, not this file* — is the same rule without a mechanism behind it.**

## What else it would settle

- **Copy the block, not the file.** Some templates say this, some do not, and
  the ones that do not get copied whole — prose, examples and all.
- **Worked examples or placeholders.** Both are in use. Examples read better and
  get pasted verbatim into real documents; placeholders are safe and teach
  nothing.
- **How much argument belongs in a template.** A template that explains itself
  duplicates its policy; one that explains nothing gets used wrongly.
- **Whether every type owes a template**, and whether a template with no type is
  a smell.

## A bundle of its own, not a section in `bundle-manager`

**The audience is different.** `bundle-manager` is opened when somebody creates,
repairs or retires a bundle. **Templates are written and edited far more often
than bundles are**, by people who are not doing bundle work at all — and a rule
buried in the bundle-authoring guide would be read by the wrong person at the
wrong time.

**`bundle-manager` keeps the layout claim.** That `templates/` exists and where
it sits is part of a bundle's shape and stays there. **What goes inside one is
this bundle's**, the same split as `records/` belonging to the layout while each
record kind belongs to its own bundle.

**Where it starts is open.** `bundle-manager`'s `where-a-bundle-belongs` prefers
a project when unsure, because that is the cheaper mistake — and **this one has
no obvious home project**, which is a real argument for the catalog rather than
an accident.

## Notes

**Found while running a review sweep**, where a status readout needed a shape
and it was unclear whether that belonged in `templates/`. The agent asserted a
rule — *a template is something written to disk* — that appears nowhere and is
contradicted by a template two files away. **A convention nobody wrote down gets
invented on demand and confidently.**
