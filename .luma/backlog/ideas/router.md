---
type: luma/idea
title: The router — a user-authored table from what was said to where it lands
created: { by: human:benlinton, at: 2026-09-02T00:00:00Z }
contributors: [human:benlinton, agent:claude-fable-5]
horizon: later
scope: organization
lifecycle: draft
---

# The router

**When a person says *I have an idea*, something should already know where ideas
go.** Which organization, which project, which bundle or tool — decided once, by
the person, ahead of time, instead of re-decided by an agent every session. Say
*I have a health entry* and the entry lands where health entries live, without
the person naming the destination or the agent guessing it.

**The router is authored on the user's or adopter's end.** That is the core of
the idea and the part that distinguishes it from everything already written
about routing. It is not a vendor's dispatch logic and not something a bundle
ships fully formed: the person designs which phrases or triggers invoke which
bundles or tools, the way they would lay out drawers. A bundle might *offer*
routes — "this bundle accepts health entries" — but the person decides whether
that offer is wired in, and where.

## What it routes

**Utterances, not work.** [[loading-mechanisms]] and [[how-knowledge-arrives]]
route knowledge *toward* a session — which bundle loads when the work matches
its line. The router runs the opposite direction: it takes something the person
produced — an idea, an entry, a note, a decision-shaped thought — and routes it
*outward* to a destination. The two might eventually be one table read from both
ends, but they are not the same problem and should not be conflated at the
start.

**The destination is a full address, not a filename.** *I have an idea* said in
this repository lands in this repository's ideas backlog. *I have a health
entry* said in this repository should still land in the health project — a
different project, possibly a different organization. The router therefore
spans the estate: it cannot live only inside one project's `.luma/`, because
its whole point is knowing about destinations the current project has never
heard of.

## The two examples that prompted this

- **"I have an idea."** The router holds logic for deciding where the idea
  goes — which project's backlog, or a triage point when the target is
  ambiguous. This very document was created by an agent reconstructing that
  routing by hand: find the ideas directory, infer the format, write the file.
  The router is that reconstruction, done once and kept.
- **"I have a health entry."** A domain the current repository knows nothing
  about. The router knows which organization, project, and/or bundle a health
  entry belongs to, and what to do with one — which is a phrase-to-destination
  mapping *and* possibly a phrase-to-tool mapping, if capturing an entry is a
  workflow rather than a file write.

## What a route might be

A route pairs a trigger with a disposition, and both halves are open:

| | options |
| --- | --- |
| **Trigger** | a literal phrase, a pattern, an intent an agent recognizes ("this is a health entry" however it was phrased) |
| **Destination** | an organization, a project, a directory, a bundle |
| **Action** | file it, invoke a tool or skill, open a workflow, ask which of two candidates |

**Intent recognition is probably the trigger, not string matching.** Nobody
says the magic words consistently. But an intent is a guess, and a router built
on guesses needs the same honesty [[loading-mechanisms]] demands of knowledge
routing: a miss should fail loudly, not silently file the thing somewhere
plausible.

## What is open

- **Where the router lives.** It spans projects, so a single repository's
  `.luma/` is the wrong home. Per-user, per-machine, per-organization, or a
  repository of its own — and how an agent in any project comes to know it
  exists.
- **Who authors a route, exactly.** The user's framing was *a user or
  adopter* — which suggests routes exist at more than one level, and that
  levels can layer: an organization routes health entries to the health
  project; a person overrides where their own ideas go.
- **Whether bundles declare routable intents.** A bundle saying "I accept
  health entries" would make wiring a route an act of adoption rather than
  free-hand authorship — which rhymes with what [[conditional-preload]] wants
  bundle metadata to carry.
- **What happens on a miss.** No route matches *I have a thought about
  pricing* — does the router refuse, ask, or hold it in an inbox for later
  filing? An inbox is itself a destination, which may be the honest default.
- **Whether this is the same table as knowledge routing read backwards.** The
  line in an entrypoint says *when the work looks like this, open me*; a route
  says *when the person says this, send it here*. Same shape, opposite
  direction, and unifying them too early would force one design onto both.
- **What this is.** A tool, a type plus a tool that reads it, or a convention
  a strong model follows from a plain file. Named last on purpose, and per
  [thesis-stronger-models-absorb-tools](../../../docs/thesis-stronger-models-absorb-tools.md)
  the plain-file answer deserves a real hearing before anything gets built.

## Notes

**Raised by the maintainer on 2026-09-02**, from wanting *I have an idea* and
*I have a health entry* to land in the right place without ceremony: the
routing exists today only as judgment an agent re-exercises every time, and it
should be something the person designs once, on their end.
