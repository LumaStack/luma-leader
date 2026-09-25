---
type: work-item
type_version: "0.0.2"
key: LEAD-0046
title: 'Roadmap ideas'
description: 'A roadmap of HQ capabilities: gather every project into a registry, own the adding of new projects and their steering committees, answer questions about the fleet, and set default branding and voice.'
workflow_status: captured
rank: 010.0460.000
kind: idea
stage: draft
created: {by: 'human:benlinton', at: '2026-08-30T21:24:09-05:00'}
# created derived from the file's first commit — the source had no frontmatter
---

# Roadmap ideas

> **Migrated from `.luma/backlog/ideas/` — read this first.** This was not an idea record — it had no frontmatter at all, so there is no author or date to preserve beyond the day it was first committed, and the title is derived from the file rather than declared. Its body is reproduced verbatim below. **I am not sure this belongs in the backlog as one work item**: it is a collection covering several separable things, and probably wants splitting. It is here as an idea so that decision gets made rather than lost.

## Gather repos

HQ needs to be able to collect all projects into a registry/manifest/whatever.
For each project it should:
- understand what each one does, why it exists, what it's value is
- audit to see if it is following branding, usability, coding guidelines
- understand what policies are being applied
- determine what policies should get mandated or recommended 
- audit if projects are within compliance
- (maybe) confirm foreman is running when/where it should

## Add project

Since HQ knows where all the projects live, it should be the one to add new projects.
It should also be in charge of the steering committees.
What routes something to architecture review, product review, legal review, etc.
Apply our organization logic engine for helping determine:
- language choices
- infrastructure framework
- teams involved
- security posture
- compliance requirements, concerns, and predictions

## Fleet Q&A

Help users ask questions about their fleet.

## Set default branding and voice

Determine a default branding strategy and voice for the organization.
Router logic for determining branding strategies.

## Policy change management

Setup a pipeline for policy change.
Who needs to be informed and who needs to sign off.

## Policy tags and enforcement

Policies are usually applied by team, department, technology stack, security posture, system criticallity, uptime requirements, etc.  So help users manage policy tags that can apply policy or recommend policy based on fluid tag criteria.

## Related work

- Born from the idea it replaces: [`roadmap-ideas`](../../ideas/roadmap-ideas.md)
