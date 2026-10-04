---
title: "Release 1 Label Audit — Linear Backlog vs. Scope Register"
type: review
sources:
  - "Linear (SPM - IS212 team, full export via API, pulled 2026-10-04)"
  - "Week 7 Customer Changes.md"
  - "Week 4 - Customer Briefing.md"
tags: [review, linear, backlog, release-1, scope, labels]
---

# Release 1 Label Audit — Linear Backlog vs. Scope Register

Every issue on the `SPM - IS212` team (about 190, including sub-tasks, bugs, spikes and coursework) was checked against [[release-1-scope]] and each ticket's own `**Scope:**` line. The aim was to find tickets that are `🎯 Release 1` or `🧩 Adjacent` but lacked the Linear `Release 1` label. The SPM Product Backlog view is only the `User Stories` label, so the audit went wider than the view.

**Convention confirmed:** the `Release 1` label covers `🎯` and `🧩`, not just the core 20. Already-labelled `🧩` tickets include SPM-25, 48, 49, 53, 55, 71, 17 and 18. The 2026-10-04 Week 7 apply ([[week-7-user-stories]]) labelled SPM-23, 21, 105, 219, 71 and 198 but did not revisit the older `🧩` tickets below.

**Result:** 15 tickets were missing the label and were labelled on 2026-10-04. Labels were added only (`User Stories`, `Spike` and others kept); no description, status or estimate changed.

## Labelled — stories (13)

| Ticket | Status | Register row / reason |
|---|---|---|
| SPM-20 Cancel venue booking | Backlog | Cancellation management `🧩` (card CH4 builds on it). Modified by [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C2]] and [[week-7-customer-changes\|Week 7 C3]] |
| SPM-54 Reschedule an event | Backlog | Rescheduling `🧩`. Modified by [[week-7-customer-changes\|Week 7 C1]] and [[week-7-customer-changes\|Week 7 C3]] |
| SPM-50 Confirm an event | Done | Event confirmation `🧩`, required directly by [#98](https://github.com/SinYang13/IS212-2026/discussions/98). [[week-7-customer-changes\|Week 7 C6]] adds a gate in front of it (SPM-264) |
| SPM-33 Request clarification | Done | Comments and discussion `🧩`, a core feature 4 outcome |
| SPM-39 View my organisation's events | Done | Client management `🧩` (card ID1) |
| SPM-40 Maintain the equipment catalogue | Done | Equipment catalogue `🧩` (card EQ1) |
| SPM-101 Record withdrawal of a request | Done | Request withdrawal `🧩`, [#103](https://github.com/SinYang13/IS212-2026/discussions/103). Ticket text says `🧩 Adjacent` |
| SPM-104 Record room layout on a booking | Done | Room layout `🧩`, pulled in by core feature 11. Ticket text says `🧩 Adjacent` |
| SPM-121 View event requests assigned to me | Done | `🧩`, pulled in by core feature 4. Scope line is in a ticket comment |
| SPM-72 Notify coordinator at registration capacity | Todo | Registration capacity `🧩` (card NT18). Ticket text says `🧩 Adjacent` |
| SPM-109 Mark an equipment requirement essential | Backlog | `🧩`. Supplies the data for SPM-50's gate ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), [#106](https://github.com/SinYang13/IS212-2026/discussions/106)) |
| SPM-164 Notify coordinator of a clarification response | Backlog | `🧩`, the return leg of SPM-33. Ticket text says `🧩 Adjacent` |
| SPM-165 Amend a returned event request | Backlog | `🧩` per its own scope line. Has no ACs and a team decision outstanding (overlap with SPM-52 and SPM-100) — labelled, but not ready |

## Labelled — non-stories (2)

| Ticket | Type | Reason |
|---|---|---|
| SPM-103 Spike: guest-attendee authorisation | Spike | Scope line says `🎯 Release 1` — decision record for two core feature 18 verbs. The clearest miss |
| SPM-144 Decide how arrangement essentiality is recorded | Task, sub-task of SPM-50 | Scope line says `🧩 Adjacent`; supplies SPM-50's gate input. SPM-98 (a task) already carries the label |

## Considered and not labelled

| Ticket(s) | Why |
|---|---|
| SPM-106, SPM-107 (spikes) | Both are `🧩`-adjacent to core feature 11, but they only block or inform that feature rather than state a Release 1 obligation (SPM-103, by contrast, says `🎯` outright). **Open for the team** — add the label if spikes should carry it |
| SPM-143 Refuse reassignment once Approved | No scope line; left alone |
| SPM-74, 73, 27, 75, 76, 77 and epics SPM-193 to 201 with children SPM-203 to 230 | `📋 Backlog` by register or ticket text. SPM-74 says so explicitly |
| SPM-26, 30, 37, 56, 137 | Canceled or Duplicate. SPM-26's work moved into SPM-81 |
| Sub-tasks and Bug tickets | Sub-tasks are unlabelled by convention; bugs sit on the Defect view |

## Labelled but arguably wrong

None. SPM-102 and SPM-99 are Canceled yet labelled (harmless). SPM-51 is labelled; its description limits Release 1 to the manual action, consistent with [[release-1-scope]].

## Not a wiki correction

[[release-1-scope]] already marks every row above `🧩` or `🎯`; the gap was only in Linear, so no wiki page changed.

## Related

[[release-1-scope]] · [[week-7-user-stories]] · [[week-7-change-impact-review]] · [[release-1-candidate-user-stories]] · [[user-story-coverage-stocktake]]
