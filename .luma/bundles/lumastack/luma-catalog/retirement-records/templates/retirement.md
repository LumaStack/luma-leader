# Retirement template

Copy the blocks to `.luma/records/retirements/<RET-NNNN>-<slug>.md`, or into a
catalog bundle when it distributes. **Copy the blocks, not this file.**

## Frontmatter

```yaml
---
type: retirement
retirement_id: RET-0000          # 3–5 letters, dash, digits. Never reused
retired_at: YYYY-MM-DDTHH:MM:SSZ # a time — renames land minutes apart
origin: project | organization   # arose from work, or handed down
scope: project | peer | organization | estate | unknown
scope_evidence: <probed what, and found what — or the mandate that set it>
decided_in: <org>/<repo>#<id>    # where the decision lives, then which one
was: <the retired idea, one line>
now: <what replaced it, one line — or what is simply gone>
why: <one line, so this stands alone without the decision>
# enforced: YYYY-MM-DD           only for a grace period. Absent = immediate
recognizers:
  - kind: term | shape | claim
    value: <what to look for>
except:
  - <path where a hit is correct as it stands>
lifecycle_status: provisional
---
```

## Body

```markdown
# RET-0000: <what was retired>

## What it was

<The old idea as somebody following it would have stated it — in their words,
not in today's. A reader has to recognize their own document here.>

## What replaced it

<The current answer. If nothing replaced it, say that the referent is gone,
because that is what makes the entry releasable later.>

## How to recognize it

<Per recognizer, what it looks like in the wild. For a `claim`, this is the
work: what would a document asserting the old idea actually say, given that it
will not contain the retired word?>

## Where a hit is correct

<Records, `## Version` histories, and documents arguing about the retirement
itself. Say why each exemption was granted — an exemption nobody explained is
indistinguishable from an oversight.>
```

## What to think hardest about

**`was` and `now` are loaded into every session.** They land in `what-we-retired`
and everybody pays for them forever. One line each.

**At least one recognizer that is not a `term`.** A word is the cheapest
recognizer and rarely the important one — the ideas that survive are the ones
whose vocabulary is intact. If you cannot state a `claim`, you may not have
finished working out what was retired.

**Do not retire ordinary English** without narrowing `scope`. Noise teaches
readers to skim past notices, and a skimmed check is one nobody runs.
