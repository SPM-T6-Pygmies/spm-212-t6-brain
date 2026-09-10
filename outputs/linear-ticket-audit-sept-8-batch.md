---
title: "Linear Ticket Audit — Sept 8 Batch"
type: review
sources:
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [review, linear, backlog, tickets, scrutiny, release-1]
---

# Linear Ticket Audit — Sept 8 Batch

An audit of the `SPM - IS212` Linear board against [[discussions-sept-8-critical-diffs]]. It answers one question: **the wiki absorbed the 2026-09-08 answers — did the tickets?**

Mostly not. Around 48 of the ~60 audited tickets needed an edit, and **nine acceptance criteria said the opposite of what the customer had said**. This is the expected failure mode of an ingestion pass that stops at the knowledge layer: the concept and domain pages were reconciled days earlier, the backlog was not, and the backlog is what people build from.

> [!warning] One contradiction was in active development when it was found
> [[event-request]] submission — the ticket implementing core feature 2 — had no acceptance criterion for the post-submission edit lock, and was assigned and in progress. A draft-then-submit form leaves the Organiser's edit affordance in place by default, which is exactly what [#102](https://github.com/SinYang13/IS212-2026/discussions/102) forbids. Caught before merge, not after.

## What was done

| Action | Count | Tickets |
|---|---|---|
| Contradictions corrected | 9 | SPM-23, 26, 31, 33, 39, 41, 56, 37, 96 |
| Closed as declined by the customer | 2 | SPM-37, SPM-56 |
| Stale open questions cleared | 2 | SPM-29, SPM-23 |
| Empty descriptions written (active sprint) | 4 | SPM-78, 95, 96, 97 |
| New tickets created | 14 | SPM-98 – SPM-111 |
| Wiki pages corrected | 1 | [[access-control]] |
| Tier 2 edits applied (2026-09-10 follow-up) | 22 | SPM-13, 15, 17, 18, 42, 44, 45, 46, 47, 50, 52, 54, 65, 67, 68, 69, 70, 71, 72, 73, 76, 87 |

Tier 2 edits — the stale-but-not-contradictory cards — are **catalogued below but not applied**. See [Not yet applied](#not-yet-applied).

## The nine contradictions

Each of these had an acceptance criterion that a customer answer had since reversed. Ranked by blast radius.

| Ticket | Was | Now | Ref |
|---|---|---|---|
| **SPM-31** Submit an event request | Silent on edit access at submission — in active development | Two ACs added: the request is read-only to the Organiser after submission, and the submit action *removes* the edit affordance rather than leaving it | [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| **SPM-56** Notify Ops Manager of a new request | "The EOM receives a notification" on every submission | **Closed.** The customer requires discoverability, not notification. Capability preserved by SPM-95 | [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |
| **SPM-37** Register an account | Self-service registration gated by email domain | **Closed.** Onboarding and self-registration are not required for Release 1; pre-loaded accounts are the sanctioned substitute. Replaced by SPM-98 | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| **SPM-23** Venue turnaround buffer | Three of four ACs computed a timestamp-level padded occupancy window | Descoped to recording two values; occupancy is a set of whole slots, so conflict detection is set intersection. Re-estimated 5 → 2 points | [#108](https://github.com/SinYang13/IS212-2026/discussions/108), [#107](https://github.com/SinYang13/IS212-2026/discussions/107) |
| **SPM-26** Set registration capacity | *Warns* when capacity exceeds the venue's | **Rejects.** The ceiling is hard, and survives a manually added VIP on a full event. Also flagged as blocked — "the venue's capacity" is no longer a single number | [#99](https://github.com/SinYang13/IS212-2026/discussions/99), [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| **SPM-39** View my organisation's events | "Cannot edit unless assigned as responsible Organiser" — implying the responsible one can | Edit is `Draft`-only; after submission no Organiser has edit access. In progress, so flagged for implementation check | [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| **SPM-41** Specify equipment requirements | A post-reservation change "is routed through the change-request process" | No change-request gate is mandated; the change flags the reservation lines for Technical Support Staff re-review | [#114](https://github.com/SinYang13/IS212-2026/discussions/114) |
| **SPM-33** Request clarification | "The Organiser resubmits after amendment" | The Coordinator records the amendment; `Returned` gets no edit-rights carve-out. Alternative reading recorded as a team decision | [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| **SPM-96** EOM sees "available" coordinators | "Available" implies computed eligibility | Retitled to "all Event Coordinators"; the system lists, the human chooses under an out-of-system SOP | [#93](https://github.com/SinYang13/IS212-2026/discussions/93) |

### Why SPM-41 was the subtlest

Its old acceptance criterion routed a Coordinator's equipment-requirement change "through the change-request process". That was not merely unmandated — it was **self-defeating**. [#102](https://github.com/SinYang13/IS212-2026/discussions/102) makes the Coordinator the person change requests are routed *to*. The card required the Coordinator to raise a change request with themselves.

This is the shape to watch for: two answers that are individually reasonable combine into an impossible workflow. Neither [#102](https://github.com/SinYang13/IS212-2026/discussions/102) nor [#114](https://github.com/SinYang13/IS212-2026/discussions/114) mentions the other.

## Three systemic defects

These are not per-card mistakes. Fixing them one ticket at a time will miss instances.

### 1. Capacity as a single number

[#112](https://github.com/SinYang13/IS212-2026/discussions/112) moved capacity from an attribute *of a venue* to an attribute *of a (venue, layout) pair*. Four venue tickets and one registration ticket still compare against one number.

The deeper problem is finding S3 from [[discussions-sept-8-critical-diffs]]: **[[booking]] records no chosen room layout.** So per-layout capacity can filter a venue search but cannot validate a booking. SPM-104 was created for it; SPM-26 and SPM-45 are blocked on it.

### 2. Attendee reachability

Every attendee-facing notification card carries "open the event from the notification" and "mark the notification as read" criteria. Attendees hold no account and no credential ([#49](https://github.com/SinYang13/IS212-2026/discussions/49)), and are not required to use the app at all ([#97](https://github.com/SinYang13/IS212-2026/discussions/97)). Fix at the epic (SPM-47) and strike the affordances per card, rather than patching three cards independently.

### 3. Two partial-outcome patterns where one belongs

[#90](https://github.com/SinYang13/IS212-2026/discussions/90) (equipment) and [#104](https://github.com/SinYang13/IS212-2026/discussions/104) (change requests) describe the same structure: a parent record, per-line outcomes, a mandatory reason on each rejected line. [[schema]] now has `change_request_item` for one of them. SPM-18, SPM-52, SPM-65, SPM-100 and SPM-110 should share the pattern.

## The missing verb

The most serious finding is not a wrong ticket. It is an absent one.

> [!warning] "View my registration status" had no ticket anywhere on the board
> Core feature 18 names four attendee verbs — view event information, register, **view status**, and withdraw. SPM-24 and SPM-82 show status once at the moment of registering. SPM-28 and SPM-84 withdraw. **Nothing let an attendee return and look.** The [#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up re-confirms all four as Release 1 obligations while ruling out attendee accounts, so it cannot be satisfied by a login. Now carded as SPM-102.

Compounding it: SPM-28 (withdraw) is `Done` and correct, but reachable only through a `registration_reference` link displayed once on a confirmation panel. Close the tab and the registration is unreachable. Two Release 1 verbs therefore rest on an undocumented bearer-token design with no expiry, entropy requirement, rate limit or revocation rule — and its one delivery channel (SPM-87) is unbuilt and unprioritised. SPM-103 was raised to ratify it.

## The fourteen new tickets

| Ticket | Why | Ref |
|---|---|---|
| SPM-98 Seed pre-loaded staff and client accounts | Replaces SPM-37; now the only route by which any account exists | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| SPM-99 Event Operations Manager as an authorised system role | SPM-29 is in progress with no ticket defining who may perform assignment | [#101](https://github.com/SinYang13/IS212-2026/discussions/101) |
| SPM-100 Decide a change request, field by field | The Coordinator side of SPM-52 was uncarded; outcomes are per-field | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| SPM-101 Record the withdrawal of a submitted request | `Withdrawn` is grounded again and Coordinator-actioned | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |
| SPM-102 View my registration status | A required feature 18 verb with no card | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| SPM-103 Spike: ratify guest-attendee authorisation | Two Release 1 verbs rest on an unratified bearer reference | [#49](https://github.com/SinYang13/IS212-2026/discussions/49), [#97](https://github.com/SinYang13/IS212-2026/discussions/97) |
| SPM-104 Record the chosen room layout on a booking | Per-layout capacity cannot validate a booking without it | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| SPM-105 Flag affected bookings on a venue attribute change | Required behaviour, with auto-revert explicitly forbidden | [#120](https://github.com/SinYang13/IS212-2026/discussions/120) |
| SPM-106 Resolve `venue.capacity` precedence | Capacity now lives in two writable places with no precedence rule | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| SPM-107 Customer question: over-capacity behaviour | The brief's "not normally" hedge survives [#112](https://github.com/SinYang13/IS212-2026/discussions/112) intact | [[event-planning-venue-booking-system\|brief §4]] |
| SPM-108 Revise an existing equipment reservation | The customer names release/replace/add; nothing acts on the re-review | [#114](https://github.com/SinYang13/IS212-2026/discussions/114) |
| SPM-109 Mark an equipment requirement as essential | The confirmation gate has a rule with no data behind it | [#106](https://github.com/SinYang13/IS212-2026/discussions/106), [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |
| SPM-110 Notify the organiser of the change-request outcome | Mandated, and the Organiser's only channel now they cannot self-serve | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| SPM-111 Notify the organiser that an event is no longer confirmed | A third de-confirmation path with no card | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) |

## What the batch did *not* break

> [!success] No completed ticket needs reopening
> All eight `Done` tickets — SPM-24, SPM-28 and subtasks SPM-79 to SPM-86 — deliver behaviour consistent with the new answers, and several are actively vindicated by them. Account-less registration was an implementation choice; [#49](https://github.com/SinYang13/IS212-2026/discussions/49) now mandates it. The hard capacity refusal was an inference from "prevent **or appropriately handle**"; [#99](https://github.com/SinYang13/IS212-2026/discussions/99) now cites it. The absence of a waiting-list queue is reconfirmed by [#116](https://github.com/SinYang13/IS212-2026/discussions/116). **The exposure is coverage, not defect** — fix it with SPM-102 and SPM-103, not with reopens.

One caveat worth verifying against the code rather than the tickets: if SPM-24 and SPM-28 were built against an authenticated attendee session rather than a bearer reference, both *would* need reopening. SPM-87's description implies they were not.

## Wiki correction

Per the Review workflow, a correction to standing knowledge is fixed at source rather than stranded in the artefact.

[#102](https://github.com/SinYang13/IS212-2026/discussions/102) had reached [[change-impact-management]] during ingestion but **never propagated to [[access-control]]**, whose Client Organisation Visibility matrix still granted the responsible Event Organiser an unqualified `Edit ✅`. That matrix is the page a developer reads to build the permission check — so the wiki was, quietly, still specifying the behaviour the customer forbade.

Corrected: the matrix cell is now scoped to `Draft`, and a fourth consequence records that edit rights end at submission and that the significant/non-significant distinction no longer governs *who may edit*.

## Not yet applied

> [!success] Resolved — the seven largest Tier 2 groups (22 tickets) were applied 2026-09-10
> All seven groups below are now applied directly to the tickets. Per-ticket rationale is recorded as a Linear comment on each card (not inlined into the body, to avoid bloat) rather than repeated here. The corrected wording lives in the ticket bodies themselves; the table below is kept as a historical record of what the audit found and now links to what changed it.

Roughly 35 Tier 2 edits were originally identified as unapplied — stale rather than contradictory. The largest groups (22 tickets):

| Group | Tickets | What they needed |
|---|---|---|
| Capacity as a scalar | SPM-42, 44, 45, 46 | Per-layout capacity wording; SPM-45 keeps its over-capacity open question |
| Attendee reachability | SPM-47, 69, 70, 73 | Strike session-dependent ACs; SPM-70's "opens" half has no addressable recipient set |
| Partial outcomes | SPM-18, 52, 65, 67 | Per-line outcome shape and mandatory decline reasons |
| Equipment return date | SPM-17, 18 | Capture `return_date` on return; fail-closed when unset ([#113](https://github.com/SinYang13/IS212-2026/discussions/113)) |
| Answered open questions | SPM-50, 54, 68, 69, 71, 72, 76 | Delete questions closed by [#106](https://github.com/SinYang13/IS212-2026/discussions/106), [#115](https://github.com/SinYang13/IS212-2026/discussions/115), [#121](https://github.com/SinYang13/IS212-2026/discussions/121), [#120](https://github.com/SinYang13/IS212-2026/discussions/120), [#99](https://github.com/SinYang13/IS212-2026/discussions/99), [#38](https://github.com/SinYang13/IS212-2026/discussions/38) |
| Dashboard wording | SPM-13, 15 | [#118](https://github.com/SinYang13/IS212-2026/discussions/118) removes the dashboard, not role-filtered navigation |
| Promotion | SPM-87 | Now a Release 1 blocker for SPM-102 and SPM-28, currently Backlog/no priority |

Applying these also surfaced two corrections to the citations above, worth recording since the table is kept as a record: SPM-54's open question was actually closed by [#108](https://github.com/SinYang13/IS212-2026/discussions/108)/[#107](https://github.com/SinYang13/IS212-2026/discussions/107) (the M1 finding), not by any discussion in this row's abbreviated list — the list was indicative, not an exhaustive per-ticket mapping. SPM-71's open question is only *partly* closed by [#120](https://github.com/SinYang13/IS212-2026/discussions/120): that answer covers the venue-attribute-change half of the trigger, not the equipment-status-change half, and leaves the resulting booking status transition undefined (see S2 above).

**Still genuinely outstanding:** roughly 13 smaller Tier 2 edits beyond these seven groups were never itemised in this audit and remain unapplied. A follow-up pass should re-run the ticket-by-ticket comparison against [[discussions-sept-8-critical-diffs]] to find them, rather than assuming this table was exhaustive.

## Two process findings

**Empty descriptions in an active sprint.** SPM-78, 95, 96 and 97 were carrying sprint work with no acceptance criteria at all, and SPM-96's title (*"available"* coordinators) was the whole specification. All four are now written. This is how SPM-31 lost its edit lock: an unwritten criterion cannot be contradicted by a new answer, so no audit catches it.

**SPM-52 at 13 points is two cards.** Submitting a change request and deciding one are different actors and different acceptance criteria. The decision half is now SPM-100.

## Tier 3 — what's missing

The most serious finding isn't a wrong ticket, it's an absent one: **"view my registration status" has no ticket anywhere on the board** — I searched to confirm. It's one of feature 18's four required verbs, re-confirmed by [#49](https://github.com/SinYang13/IS212-2026/discussions/49). Related: SPM-28 (withdraw) is _Done_ and correct, but reachable only via a link shown once on a confirmation panel — so two Release 1 verbs currently rest on an undocumented bearer-token design with no expiry, no rate limit, and no delivery channel built. SPM-87 is the unblocker and sits at Backlog/no-priority.

About 15 new tickets are needed. The load-bearing ones: find-unassigned-requests (replacing SPM-56), Coordinator-decides-change-request-per-field, Coordinator-records-withdrawal, record-layout-on-booking, seed pre-loaded accounts, view-registration-status.

## Related pages

- [[discussions-sept-8-critical-diffs]] — the diff report this audit applies to the backlog
- [[release-1-scope]] — the register every scope marker above is checked against
- [[access-control]] — corrected by this pass
- [[schema-migration-02]] — the schema half of the same batch
- [[user-story-coverage-stocktake]] — the previous Linear-versus-wiki cross-reference
- [[backlog-candidate-user-stories]] — the backlog drafts revised against the same batch
