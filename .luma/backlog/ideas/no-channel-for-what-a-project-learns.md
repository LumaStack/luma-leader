---
type: luma/idea
title: Nothing carries what a project learned back up to hq
created: { by: agent:claude-opus-5, at: 2026-08-26T00:00:00Z }
contributors: [agent:claude-opus-5, human:benlinton]
horizon: next
scope: organization
lifecycle_status: draft
---

# Nothing carries what a project learned back up

**The estate has a direction and it works.** hq settles a standard, foreman makes
it executable, changes flow one way. `the-estate` says so and `luma-foreman`'s
own README repeats it: *hq settles, foreman ships.*

**The return path is asserted and does not exist.** `luma-foreman/docs/scope.md`
says knowledge *"can travel both ways. Plenty of it starts in one project and
outgrows it, and foreman should make promoting it upward easy."* Nothing does.
There is `publish-to-the-catalog` for content that becomes a bundle, and nothing
at all for a finding, a measurement, or a thing that turned out to be already
built.

## What one day cost

On 2026-08-26, work in `luma-foreman` produced four things this repository
wanted and did not receive. They reached `loading-mechanisms.md` only because
somebody happened to open it afterwards.

**Something listed as unbuilt was built.** *A trigger that matches nothing is
silently inert* is ranked first under *Never discussed* and called *"probably
the next thing to work on."* `inspect --rule bundles` had implemented it, in
almost the same words. Anybody acting on that ranking would have built it twice.

**A measurement that a duty was explicitly waiting for.** *Budget — derivable,
but only by measuring.* Seventeen bundles adopted, index measured at about 5,300
tokens a session, 40% of it redundant against the same harness. The number
existed for hours before anything connected it to the duty that asked for it.

**A decision re-derived from scratch.** The verb for what `apply` does was
argued out in foreman across an afternoon — the collision with `project` as a
noun, the candidates, `install` ruled out for the reason ADR-0002 gives. This
repository had already done that work, listed three candidates and estimated the
cost at *twenty-five occurrences in foreman and nineteen across six bundles*.
The estimate was accurate. Foreman never saw it, and landed on a fourth
candidate hq had not considered.

**A prediction confirmed, with nobody told.** Two of the *five pressures* bind
at seventeen bundles rather than at the two hundred the document imagines.

## Why the obvious answers are not it

**"Read hq first" is not a mechanism.** It is what should have happened and it
depends on somebody thinking to. Every failure above is a case of not thinking
to, by an agent that had the repository on disk.

**A bundle is the wrong shape.** Bundles carry knowledge that is ready to be
adopted. A measurement, a confirmation, and *this is already built* are none of
those — they are reports, they are dated, and most have no audience beyond one
document.

**An audit nearly is it, and is not.** `audit-records` gives findings, responses
and dispositions, and the estate audit of the same day pinned every repository
before examining anything. But an audit is somebody deciding to look, at a
moment, from outside. This is the opposite: a byproduct of ordinary work, from
inside, with nobody looking.

## What it might be

Unresolved, and the shape matters more than the mechanism.

**A `luma-leader` inbox** — a directory where any project drops a dated note.
Cheap, no tooling, and it rots if nobody sweeps it.

**A foreman command.** `luma-foreman report`, writing where the config says hq
is. Makes the act cheap at the moment somebody notices something, which is the
only moment they will do it. Costs foreman a dependency on hq existing, which
`the-estate` says it must not require — though *may use when present* is already
the rule.

**Nothing, and hq reads down instead.** Defensible: hq is where cross-project
reasoning lives, so hq going to look is its job rather than a project's. It
fails the same way *read hq first* fails, in the other direction.

## What to notice either way

**Every failure here was silent.** Nothing errored, nothing was reported, and the
day's work looked complete from inside foreman. The cost was invisible until the
two documents were read side by side — which is the same failure class this
estate keeps naming: *cannot tell nothing-applies from I-never-looked.*
