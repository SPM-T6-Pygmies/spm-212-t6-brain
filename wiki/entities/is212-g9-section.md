---
title: IS212 Section G9
type: entity
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 26.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "Week 7 Customer Changes.md"
  - "Managing Changes – A Guide.md"
tags: [entity, course, process, is212, g9]
---

# IS212 Section G9

The course section, its customer personas, and the clarification channel governing this project.

## Course context

| Attribute | Value |
|---|---|
| Course | IS212, AY 2026/27 Term 1 |
| Section | G9 |
| Deliverable | First release of a system built using the **scrum process** |
| Submission | Week 12 |
| Clarification repo | `SinYang13/IS212-2026` (GitHub Discussions) |

Grading rubrics emphasise **scrum process, requirements discovery, software quality, and documentation of those activities** more than the final product alone (Source: customer-brief-no-1).

## People

| Handle | Role |
|---|---|
| `SinYang13` | Customer persona / instructor; author of most answers and the board announcement |
| `josephsungls` | Customer persona; answered part of the 2026-08-25 batch |

Each section has a **different** customer. Clarifications from other sections may be inconsistent and must not be relied on.

## Groups active on the board

Questions are prefixed with a group tag. Observed: T1, T3, T6.

**T6 is this vault's team.** T6 contributors on the board are `Isaidchia`, `jerricknsc` and `WilliamsSmithM`. T6 questions cluster on technical support operations ([#85](https://github.com/SinYang13/IS212-2026/discussions/85)–[#90](https://github.com/SinYang13/IS212-2026/discussions/90)), coordinator assignment mechanics ([#92](https://github.com/SinYang13/IS212-2026/discussions/92)–[#95](https://github.com/SinYang13/IS212-2026/discussions/95)), venue suitability ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)) and protected information ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)) — and T6 raised the equipment-availability contradiction resolved in [#2](https://github.com/SinYang13/IS212-2026/discussions/2).

## Board rules

1. Search before posting; detected duplicates may be closed.
2. Post under the relevant category, or General if none fits.
3. Prefix titles with the group number, e.g. `[T6] ...`.
4. Upvote questions your group also wants asked — top-upvoted questions are taken during in-person clarification sessions, the rest ad-hoc.

Answers may be terse or slow by design; the stated aim is to build the skill of asking **clear, concise, specific questions**.

## Categories

Announcements · Calendar · Equipment & Technical Support · Events · General · Non-Functional Requirements · Notifications · Registration & Attendees · Reporting & Analytics · Users · Venues

## Timeline

| When | Milestone |
|---|---|
| Weeks 2–4 | Understand customer needs; build product backlog of epics and user stories |
| Week 3, Friday 6pm | Project instructions and grading rubrics released |
| Week 4 | Customer identifies core Release 1 functionality; sprinting may begin |
| Week 7 | Customer change notice: six mandatory changes to Release 1, no further changes expected ([[week-7-customer-changes]]) |
| Week 12 | First release submitted |

Snapshots of the board are archived as [[is212-discussions-2026-08-26]], [[is212-discussions-2026-08-31]], [[is212-discussions-2026-09-08]], [[is212-discussions-2026-09-16]] and [[is212-discussions-2026-10-01]].

## Change management

The course guide `Managing Changes – A Guide.md` defines how a Scrum team handles a customer change. It is process, not domain content, so it is summarised here rather than ingested as a source page.

A **change** is any new, revised or corrected requirement that affects the Product Backlog or planned work. Handle it in six steps: describe the change (what, why, and whether it is new, a clarification or a correction); update the backlog; update acceptance criteria; re-estimate; update the design documentation (C4, class and sequence diagrams, data model, API specs); and update the test cases (unit, functional, integration, end-to-end, regression).

Its three worked scenarios decide what happens to a story already marked Done:

| Scenario | Situation | Action |
|---|---|---|
| 1 | An original acceptance criterion was missed | Reopen the story; estimate only the **remaining work**. Not a new requirement |
| 2 | A genuinely new requirement | Keep the story Done; add a **new story** and estimate it |
| 3 | A new requirement **and** an original criterion that was incorrect | Do both: a new story for the new requirement, and reopen the old story for the correction |

Re-estimate an existing item only when new information materially changes its size, complexity, effort or uncertainty. The guide was applied to [[week-7-customer-changes]] in [[week-7-change-impact-review]].
