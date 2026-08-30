---
type: luma/project
title: luma-leader
disclosure_level: public
description: The general engine any organization can use to decide how it works — the shape of the job, the conventions for arguing a standard into existence, and defaults worth starting from. Not any particular organization's headquarters.
---

## Why it exists

An organization deciding how it works needs somewhere to argue a standard into
existence and record why it holds. This is the tooling and the conventions for
doing that — **general, reusable, and belonging to nobody in particular.**

**It is the product, not an instance of it.** An organization using this has its
own internal headquarters holding what is specific to them. That repository is
configuration; this one does not name it.

## No organization-internal content, ever

`disclosure_level: public` is the operative fact, and **it does not depend on
this repository's current visibility.** This repository is published, so
anything written here is readable immediately and cannot be recalled.

**This is the one boundary worth being rigid about.** There is no directory here
in which an organization's internal content would be valid, and anything
unexpected should fail rather than sit quietly.

**It has been crossed once.** A private repository index was written here while
this repository was still unpublished. It was removed from the tree, but the
residue survived in the pull request that added it — a merged pull request keeps
its diff whether or not the commit stays reachable. So publication started this
repository fresh from the working tree, and the history carrying that residue
was discarded rather than published.

**Removing a file is not removing the disclosure.** That is the part worth
remembering: the fix was not deleting the file, which had already been done and
had not worked. It was refusing to publish the history that still held it.

## Boundaries

**Public gets the tool.** All source, all commands, example configuration with
no real values, and the extension points.

**Private gets the values.** An organization's real configuration, its
org-specific extensions, and *references* to secrets — never the secrets
themselves.

**The dependency edge runs one way.** Private depends on public at a pinned
commit. Public has no code path that opens anything beneath a private root —
not by policy, by absence of capability.
