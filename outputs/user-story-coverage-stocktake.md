---
title: User Story Coverage Stocktake — Linear vs. Wiki vs. GitHub Discussions
type: review
sources:
  - "Linear (SPM - IS212 team, 76 issues, pulled 2026-09-06)"
  - "wiki/entities/event-planning-venue-booking-system.md"
  - "wiki/concepts/release-1-scope.md"
  - "wiki/concepts/open-questions.md"
  - "outputs/release-1-candidate-user-stories.md"
  - "outputs/user-stories-proposed-rewrites.md"
  - "IS212 2026 Discussions Aug 26.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 User Story Standard — v1.0.md"
  - "outputs/backlog-candidate-user-stories.md"
tags: [review, user-stories, backlog, stocktake, linear, github-discussions]
---

# User Story Coverage Stocktake — Linear vs. Wiki vs. GitHub Discussions

A point-in-time cross-reference of the **76 issues in the Linear `SPM - IS212` team** against the two candidate backlogs already drafted in this wiki ([[release-1-candidate-user-stories]], [[user-stories-proposed-rewrites]]) and the ~60 functionality areas catalogued in [[event-planning-venue-booking-system]] and scoped in [[release-1-scope]]. The goal: find user stories that **exist as a discussion topic or a scoped functionality area but exist nowhere as a card** — in Linear or in the wiki's own output backlogs — regardless of whether they are `🎯 Release 1`.

## Method

1. Pulled all 76 Linear issues (55 carry the `User Stories` label; the rest are coursework/admin tickets).
2. Matched each against [[release-1-candidate-user-stories]]'s coverage table, which already records a Linear ID for every `🎯`/`🧩` card it drafted (including all 21 notification cards, NT1–NT21 → SPM-47, SPM-56–75).
3. Matched the remainder against [[user-stories-proposed-rewrites]]' cards (U1–U4, E1–E2, V1/V2/V4/V5/V6, Reg1–Reg6, EV1–EV3c).
4. Walked [[event-planning-venue-booking-system]]'s functionality-areas table — the closest thing this wiki has to a full enumeration of everything the customer's brief named — and checked every `📋 Backlog` and `🧩 Adjacent` row for a card anywhere, ticketed or not.

## 1. Release 1 (🎯 / 🧩) coverage is complete — both anomalies confirmed and closed

Every `🎯 Release 1` and `🧩 Adjacent` card drafted in [[release-1-candidate-user-stories]] and [[user-stories-proposed-rewrites]] has a matching Linear issue. The two exceptions this stocktake originally flagged were taken back to the team and both turned out to be deliberate restructuring rather than lost scope; the resolutions are recorded below so the next stocktake doesn't re-raise them.

> [!success] Resolved — SPM-30 folded into SPM-29
> Coordinator reassignment is explicitly inside core feature 5 ("coordinator assignment... including reassignment", [[release-1-scope]]). It was **not** lost when SPM-30 was closed as a Duplicate: it became an acceptance criterion on SPM-29, which now reads "Be able to assign **and reassign** an Event Coordinator to an event; the assignment takes effect immediately with no acceptance step ([#95](https://github.com/SinYang13/IS212-2026/discussions/95))". The `🎯 Release 1` sub-requirement is tracked. Note the AC set no longer carries [#94](https://github.com/SinYang13/IS212-2026/discussions/94)'s point that the system may assume a reassignment has already been approved outside it — worth folding in when SPM-29 is next touched.

> [!success] Resolved — the "return" outcome decoupled to SPM-33
> The wiki's source card (EV3c in [[user-stories-proposed-rewrites]]) has three outcomes — approve, reject, **or return to the Organiser for amendment**. SPM-34 legitimately carries only two: the return outcome was decoupled into SPM-33 "Request clarification on an event request", whose AC set now ends "The status of the event should be marked as *returned*". Approve/reject and return-for-amendment are two decisions with different follow-on behaviour, so splitting them is the better slice, not a dropped requirement. Core feature 4 is covered across SPM-33 and SPM-34 together. [#67](https://github.com/SinYang13/IS212-2026/discussions/67)'s open question — whether a rejected request can be resubmitted — stays on SPM-34 and remains unanswered.

## 2. Functionality with a citation but no card anywhere (Linear or wiki)

These are `📋 Backlog` or genuinely undecided areas — correctly *not* built for Release 1 — but per [[release-1-scope]]'s own rule ("out of Release 1 ≠ out of the project... keep deferred features in your product backlog and C4 models"), they should still exist as a backlog item somewhere. Right now several exist only as prose on a wiki concept page, with no story card — in Linear or in an output backlog — representing them at all.

| Area | GH discussion(s) | Wiki page | Status |
|---|---|---|---|
| **Check-in / attendance recording** | [#38](https://github.com/SinYang13/IS212-2026/discussions/38) | [[attendee-registration]] | ✅ Ticketed as **SPM-76** — Reg6 promoted as-is, priority and estimate left unset. |
| **VIP / non-self-registering attendee** (Organiser can't add attendees directly; list goes to the Coordinator) | [#33](https://github.com/SinYang13/IS212-2026/discussions/33) | [[reporting]] | ✅ Ticketed as **SPM-77**, a spike with three questions and no timebox proposed. |
| **Technical support requests** (general, beyond equipment reservation) | [#25](https://github.com/SinYang13/IS212-2026/discussions/25), [#87](https://github.com/SinYang13/IS212-2026/discussions/87) | [[technical-support]], [[support-request]] | Has its own concept page and domain object; **zero cards** in either output backlog or Linear. 📝 Now drafted as **TS1–TS3** in [[backlog-candidate-user-stories]]. |
| **Technical staff assignment and rostering** | [#88](https://github.com/SinYang13/IS212-2026/discussions/88), [#89](https://github.com/SinYang13/IS212-2026/discussions/89), [#94](https://github.com/SinYang13/IS212-2026/discussions/94), [#95](https://github.com/SinYang13/IS212-2026/discussions/95) | [[technical-support]] | Same gap as above — the Technical Support Staff *role* is ticketed via E1/E2 (SPM-17/18), but nothing about assigning or rostering the staff themselves. 📝 Now drafted as **TS4–TS5** in [[backlog-candidate-user-stories]]. |
| **Tentative venue holds** | [#39](https://github.com/SinYang13/IS212-2026/discussions/39), [#41](https://github.com/SinYang13/IS212-2026/discussions/41) | [[venue-booking]] | No card. VN5/SPM-46 covers a direct booking request only, per [[release-1-scope]]'s deliberate cut of the tentative-hold half. 📝 Now drafted as **VH1–VH2** in [[backlog-candidate-user-stories]]. |
| **Event categories** | Named only in [[event-request-workflow]]'s request-intake table, no discussion number | [[event-request-workflow]] | No card. 📝 Now drafted as **EC1** in [[backlog-candidate-user-stories]]. |
| **Recurring / reuse of previous event information** | [#75](https://github.com/SinYang13/IS212-2026/discussions/75) | [[event-request-workflow]] | No card — the "what does reuse copy" question is open and undrafted. 📝 Now drafted as **EC2** in [[backlog-candidate-user-stories]]. |
| **Multi-session events** | [#74](https://github.com/SinYang13/IS212-2026/discussions/74) | [[session]] | No card — [[session]] is a full domain object with no story representing it. 📝 Now drafted as **EC3–EC5** in [[backlog-candidate-user-stories]]. |
| **Supporting documents on an event** | [#15](https://github.com/SinYang13/IS212-2026/discussions/15), [#18](https://github.com/SinYang13/IS212-2026/discussions/18), [#21](https://github.com/SinYang13/IS212-2026/discussions/21) | [[event-request-workflow]] | No card. 📝 Now drafted as **EC6** in [[backlog-candidate-user-stories]]. |
| **Outstanding action tracking** (list-and-chase surface beyond the confirmation gate) | Arises from [#80](https://github.com/SinYang13/IS212-2026/discussions/80) but the dashboard itself was deliberately deferred | [[event-readiness]] | No card. RA3/SPM-50 covers the block only, by design. 📝 Now drafted as **RC1–RC2** in [[backlog-candidate-user-stories]]. |
| **General reminder system** (beyond incomplete-arrangement reminders) | [#40](https://github.com/SinYang13/IS212-2026/discussions/40) | [[notifications]] | Partially covered — NT21/SPM-75 handles the one confirmed reminder category. A general, configurable reminder system is untouched. 📝 Now drafted as **RM1–RM3** in [[backlog-candidate-user-stories]]. |
| **Event calendar** (event-level, distinct from the venue availability calendar) | Named only in the brief's discovery table | [[reporting]] | No card. VN2/SPM-43 is the **venue** calendar (core feature 9); nothing represents an event-level calendar. 📝 Now drafted as **CV1–CV2** in [[backlog-candidate-user-stories]]. |
| **Event search and filtering** (event-level, distinct from venue search) | Named only in the brief's discovery table | [[reporting]] | No card. VN3/SPM-44 is **venue** search (core feature 10). 📝 Now drafted as **CV3** in [[backlog-candidate-user-stories]]. |
| **Role-based dashboard** | Named only in the brief's discovery table | [[access-control]] | No card — distinct from RA1/SPM-48 (a single event's own status) and U3/SPM-15 (which functions a role can see), neither of which is a cross-event overview. 📝 Now drafted as **CV4** in [[backlog-candidate-user-stories]]. |
| **Activity history / change history** (browsable UI) | The underlying audit record is bound by [#14](https://github.com/SinYang13/IS212-2026/discussions/14), but the browsing surface is not | [[non-functional-requirements]] | No card — only the write-side auditability NFR is acknowledged anywhere. 📝 Now drafted as **HB1–HB2** in [[backlog-candidate-user-stories]]. |
| **Event / venue usage / registration reports, and report generation & export** | [#29](https://github.com/SinYang13/IS212-2026/discussions/29), [#32](https://github.com/SinYang13/IS212-2026/discussions/32) | [[reporting]] | No card anywhere — the entire Output group in [[event-planning-venue-booking-system]]'s functionality table has no representation in either backlog. 📝 Now drafted as **RP1–RP4** in [[backlog-candidate-user-stories]]. |
| **Primary contact per client organisation** | [#65](https://github.com/SinYang13/IS212-2026/discussions/65) | [[client-organisation]] | Open question with no card either way — smaller in scope than the rest of this table, but genuinely undrafted. 📝 Now drafted as **CO1** in [[backlog-candidate-user-stories]]. |

## 3. Explicitly denied — not gaps, but worth one line each

These were **answered "not a requirement"**, not deferred (see [[event-planning-venue-booking-system]]'s "Explicitly out of scope" table) — they don't belong on a backlog. Two of them are flagged elsewhere in the wiki as operational risks despite being denied, which is a different kind of finding than "missing story":

- **Account recovery / locked-account handling** ([#64](https://github.com/SinYang13/IS212-2026/discussions/64)) — denied, but [[release-1-scope]] and U1/U2 both note that shipping authentication with no recovery path is operationally incomplete.
- **Admin role for system configuration / user administration** ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)) — denied, but the unlisted Event Operations Manager role ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#93](https://github.com/SinYang13/IS212-2026/discussions/93)) already exercises admin-like power (coordinator assignment) with no formal role definition.

Not a call to build either — just noting the two aren't silent gaps, they're customer decisions the team has already surfaced.

## Recommendations

*Status as of 2026-09-06 — all five actioned. Recorded rather than deleted so the trail from finding to action stays readable.*

1. ✅ **Promote Reg6 (check-in) to Linear as-is** — done, **SPM-76**. Ticketed with the role left unnamed and priority/estimate unset, exactly as drafted.
2. ✅ **Open a spike ticket for the VIP / bulk-attendee-add case** ([#33](https://github.com/SinYang13/IS212-2026/discussions/33)) — done, **SPM-77**, under a new `Spike` label. Three questions, no timebox proposed.
3. ✅ **Confirm the two Release 1 anomalies** — both closed; see the resolutions in §1. Neither was lost scope.
4. ⏳ **Log a backlog item per area in §2** — the drafting half is done: all seventeen areas are now carded in [[backlog-candidate-user-stories]] as nine epics, 30 stories and one spike. **The Linear half is deliberately not done.** The standard's workflow is `AI drafts → team decides → customer confirms` (§11), and pushing 30 unreviewed cards into Linear would skip the middle step and produce exactly the bulk-generated backlog §11 warns against. Review the document, then push what survives.
5. ✅ **Start the next drafting pass with Reporting and Technical Support** — done. They became **EP-B** (RP1–RP4) and **EP-A** (TS1–TS5), the two largest epics in the new document.

## What the drafting pass found

Worth carrying forward beyond the cards themselves:

- **Every one of the 30 cards has a blocking unknown.** Not a drafting failure — it is why these areas went uncarded. The cards make the unknowns enumerable; [[backlog-candidate-user-stories]] lists the ten that need the *customer* rather than the team.
- **Six cards name no role**, because the customer never named one: who checks attendees in ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)), who allocates Technical Support Staff ([#88](https://github.com/SinYang13/IS212-2026/discussions/88)), who may place a tentative venue hold, who may browse history, who configures reminder frequency, and who counts as an "appropriate user" for an event report.
- **[#52](https://github.com/SinYang13/IS212-2026/discussions/52) and [#47](https://github.com/SinYang13/IS212-2026/discussions/47) are in tension.** The customer requires reminder frequency to be configurable while denying any admin role for system configuration. §3 above lists the missing admin role as an operational risk; this is a concrete case where it bites.
- **Two areas have no scope marker anywhere.** The VIP path sits under the `🎯` attendee-registration row with nothing in core feature 18 covering it, and primary contact has no row in [[release-1-scope]]'s register at all despite that register claiming to hold every functionality area exactly once.
- **This document's own count was off by one** — the prose said sixteen §2 areas, the table has seventeen. Corrected in [[backlog-candidate-user-stories]] and here.

## Related pages

[[backlog-candidate-user-stories]] · [[release-1-scope]] · [[release-1-candidate-user-stories]] · [[user-stories-proposed-rewrites]] · [[open-questions]] · [[event-planning-venue-booking-system]] · [[technical-support]] · [[support-request]] · [[reporting]] · [[attendee-registration]] · [[event-readiness]] · [[session]]
