---
type: policy
title: What we retired
description: Every idea this estate has retired, one line each — what it was, what replaced it. The list an author needs before writing, not after.
matches: always
---

# What we retired

**Read this before reaching for the obvious word.** Every line below is an idea
somebody already decided against. The point of loading it up front is that a
retired idea comes back by being *reinvented* — an author reaches for the natural
word and it reads as a fresh choice rather than a revival, which no check running
afterwards can prevent.

**Generated from `retirements/` in this bundle.** Do not edit it by hand — a
hand-maintained copy of a list is exactly how the last one went stale. Each row
below is one record; open it for the recognizers, the exemptions, and where the
decision that retired it lives.

## The list

| id | was | now |
| --- | --- | --- |
| `RET-0001` | the command `outfit` | `apply` |
| `RET-0002` | `projection`, for what `apply` writes | the **adapters**, or `apply` itself |
| `RET-0003` | `applies_to`, the field naming what surfaces a Document | `matches` |
| `RET-0004` | `preload`, a document declaring when it is loaded | delivery is derived from `matches`; `matches: always` is the only route up front |
| `RET-0005` | a catalog **declares** its namespace | it **derives** from where the catalog lives; a fork gets its own |
| `RET-0006` | audit independence means a different **party** or model | a different **session** — carried context is what compromises a record |

**Six lines is the whole budget.** This document is loaded into every session
that touches this bundle, so it earns that by staying a list. The reasoning lives
in [[retiring-a-concept]] and in each record; **nothing here explains itself.**

## If a line here surprises you

**You are the case this document exists for.** Read the record — it carries
`was`, `now`, a one-line `why`, and where the decision that retired it lives.

**And if you think a line is wrong**, that is a legitimate finding against the
strategy rather than against your repository. [[sweep-retirements]] says how to
file it.
