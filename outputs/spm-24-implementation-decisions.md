---
title: SPM-24 Implementation Decisions — Register for a Confirmed Event
type: review
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "Week 4 - Customer Briefing.md"
tags: [review, registration, attendee, decisions, spm-24]
---

# SPM-24 Implementation Decisions — Register for a Confirmed Event

The decisions taken while building [SPM-24](https://linear.app/pygmies/issue/SPM-24) and its five sub-tasks in the `connectsphere` repository, and one correction this work forced back into the wiki. This page is a review artefact, `type: review`, living in `outputs/` rather than `wiki/` — it records what the team chose on a particular ticket, not standing knowledge about ConnectSphere. It is linked from [[index]] so it isn't an orphan.

> [!info] Reconciled with the schema work that landed in parallel
> [[schema]] reached the same account-less conclusion independently, from the modelling side, while this ticket reached it from the build. [[schema]] is the physical model of record; where this page and it disagree, the disagreement is named in §9 rather than silently resolved.

Every decision below is the **team's proposal**, not a customer statement. Where the customer was asked and declined to answer, that is said explicitly.

---

## 1. The correction: attendee authentication was never a requirement

This is the one item that changed standing wiki pages rather than just the code.

Three pages asserted, in the customer's voice, that an Attendee must authenticate:

| Page | What it said | Why it was wrong |
|---|---|---|
| [[access-control]] | "Is authentication required? **Yes**", citing [#62](https://github.com/SinYang13/IS212-2026/discussions/62) | [#62](https://github.com/SinYang13/IS212-2026/discussions/62) is about the authentication *method* being unspecified. It does not establish who must authenticate |
| [[release-1-scope]] | Feature 18's actions are "**all authenticated actions under feature 1**" | The Week 4 briefing says access "depends on" the user's role — not that every role holds a login |
| [[registration]] | Modelled the Attendee attribute as a required [[user-account]] reference | Followed from the two above. Independently corrected by the [[schema]] work, which made the account optional |

Compounded, they produced a `> [!warning] Attendees can register but cannot be created` callout claiming Release 1 shipped an undemonstrable feature. That callout has since been narrowed rather than withdrawn — see the caveat below.

What the customer actually said, in full ([#53](https://github.com/SinYang13/IS212-2026/discussions/53)):

> [!quote] Customer answer — [#53](https://github.com/SinYang13/IS212-2026/discussions/53)
> The system supports Attendees as **external users** who can register for events and view their registration status. External user onboarding features are not listed in the brief yet

Read plainly, that describes external users who register — and puts onboarding outside the brief precisely *because* no account is needed. Together with [#49](https://github.com/SinYang13/IS212-2026/discussions/49) (onboarding not a feature) and [#52](https://github.com/SinYang13/IS212-2026/discussions/52) (no admin role), the absence of a provisioning route is not a gap in the release; it is consistent with attendees never having accounts.

**Decision: the attendee-facing view is public and unauthenticated.** [[access-control]], [[release-1-scope]], [[registration]], [[open-questions]], [[domain-model]] and [[schema]] now reflect it. ([[supabase-schema-ddl]] is superseded and was deliberately left alone.)

**One claim from this ticket did not survive review.** An earlier draft withdrew [[release-1-scope]]'s risk callout outright. That was too strong: feature 18 has four verbs, and going account-less closes two of them — *register*, and *view events* via the public list and detail pages (§6, §7). *View status* and *withdraw* still need a way to identify a guest, which is why [[schema]] mints a `registration_reference`. The callout is narrowed, not gone.

Consequences the team owns, and should not discover later:

- There is no identity check. Anyone may register any email address.
- Withdrawal ([SPM-28](https://linear.app/pygmies/issue/SPM-28)) has no logged-in user to scope to, so it will need a per-registration token in the confirmation. [[schema]]'s `registration_reference` UUID exists for exactly this, but nothing yet delivers it to the attendee.
- "View my registration status" (core feature 18) has the same problem and the same answer.
- Staff-facing features are untouched: internal roles still authenticate, and [#62](https://github.com/SinYang13/IS212-2026/discussions/62)'s open question about the method still stands.

## 2. Registration fields: full name and email

[#22](https://github.com/SinYang13/IS212-2026/discussions/22) returned the question in full — "Please propose an appropriate set of information based on the event needs." [[attendee-registration]] read "based on the event needs" as hinting at a per-event configurable field set.

**Decision, for Release 1: full name and email, fixed, for every event.** Per-event configurable fields are deferred — nothing in the core 20 names them, and they would need an organiser-facing editor that is not in Release 1. The email doubles as the attendee's identity (§1), so it is not optional. [#22](https://github.com/SinYang13/IS212-2026/discussions/22) stays open as a question for later releases; it is the *Release 1* field set that is settled.

## 3. Duplicate registration is refused

**No customer source addresses this at all.** The only prior artefact was the superseded [[supabase-schema-ddl]]'s `registration_unique_per_event_uidx`, itself an invention; [[schema]] now carries it as `registration_one_live_per_attendee_email_uidx`.

**Decision: one live registration per (event, email).** A withdrawn registration releases the place, so it does not block a fresh one — consistent with [[event-planning-venue-booking-system|brief §4]]'s "the released place may subsequently become available". Matching is case-insensitive, since the email is normalised on the way in.

**Assumption, flagged for the customer:** an attendee who registers twice by accident sees an error rather than a second place being taken. If ConnectSphere wants silent idempotency instead, that is a one-line change.

## 4. A full event is refused outright

[[event-planning-venue-booking-system|brief §4]] hedges: the system "should prevent **or appropriately handle**" registrations beyond capacity. [[release-1-scope]] puts the waiting list in `📋 Backlog`, including its committed 3-day acceptance window ([#44](https://github.com/SinYang13/IS212-2026/discussions/44)).

**Decision: prevent.** With no queue to join, "appropriately handle" has no other meaning in Release 1. The attendee is told the event is full.

**Known limitation:** the capacity check reads the current count and then writes, so two simultaneous submissions for one remaining place can both succeed. Closing that needs a database-side constraint or a serialised transaction; it is not worth the complexity before the release has a real database. The duplicate rule *is* closed at the database, by a partial unique index.

## 5. Schema deviations from the candidate DDL

The candidate DDL states it is "a candidate physical data model, not a customer deliverable" and that names are "illustrative and freely renameable". The implementation took that invitation. The comparison below is against [[schema]], which superseded [[supabase-schema-ddl]] while this ticket was in flight:

| Deviation | Reason |
|---|---|
| `uuid` primary keys, not `bigint generated always as identity` | Matches the tables already in the repository, and the application allocates an id before the row exists |
| Lowercase status values (`confirmed`), not Title Case (`Confirmed`) | Matches the existing `connections` table |
| Registration window as `timestamptz`, not `date` | A `date` would force the core to expand "closes on 30 Sep" into a Singapore instant, putting timezone logic inside business rules |
| Registration keyed on `lower(attendee_email)` | Follows from §1. [[schema]] adopted the same index |

The event-status defect this ticket flagged and declined to fix — the candidate DDL's `event_status_chk` allowing `'In Progress'` while omitting `Submitted`, `Under review`, `Planning`, `Awaiting arrangements`, `Rejected` and `Returned` — has since been **resolved upstream**. [[current-schema-scrutiny]] found it independently and [[schema]] fixes it, allocating [[event-planning-venue-booking-system\|brief §4]]'s ten example statuses one per table across `event_request` and `event`. SPM-24 only needed `Confirmed`, which survives the split unchanged, so the build needs no follow-up.

## 6. Scope: how much browsing was built

[[release-1-scope]] marks **event search and filtering** `📋 Backlog`, noting that feature 10's search is *venue* search. But core feature 18 gives Attendees "view appropriate event information", and [#10](https://github.com/SinYang13/IS212-2026/discussions/10) confirms they may view "events open for registration".

**Decision: a list of the events open for registration, plus a search box on event name. Nothing else.** No category filters, no date-range filters, no calendar — [[release-1-scope]]'s own warning against over-reading adjacency applies directly. The search narrows what is already on screen; which events exist to be searched is decided by the same rule that governs the detail page.

## 7. What an unavailable event returns

An event that is not confirmed, has registration disabled, or sits outside its window is **absent from the list and answers 404 when addressed directly** — indistinguishable from an event that does not exist.

That is deliberate: [[non-functional-requirements|brief §8b]] forbids showing Attendees internal planning information, and a page that said "this event exists but is not confirmed yet" would leak exactly that.

## 8. Registration confirmation content

Fourteen notification triggers are recorded from [[notifications|brief §6]]. **"An attendee successfully registers" is not among them**, and [[notification]] marks message content ⚠️ Partial throughout.

**Decision: an on-screen confirmation showing event name, date, time and venue**, as [[release-1-candidate-user-stories]]'s card RG1 proposes. No email or in-app notification was built — that belongs with the notification tickets, and the customer has not specified the content of one.

The venue is read through the event's confirmed [[booking]], since [[domain-model]] draws no direct Event↔Venue edge. An event confirmed without a booking yet shows no venue rather than being hidden.

## 9. Open items this ticket did not settle

| Item | Status | Ref |
|---|---|---|
| Where the registration ceiling comes from — coordinator-set or derived from the booked venue | Still open; the implementation reads a ceiling stored on the event | [[attendee-registration]] |
| Whether a closed registration period can be reopened outright, or only extended | Still open | [#42](https://github.com/SinYang13/IS212-2026/discussions/42) |
| Withdrawal cut-off before the event date | Still open — "raise with the customer rather than assume a deadline" | [[event-planning-venue-booking-system\|brief §4]] |
| Row-level security policies for `registrations` | **Not written.** Attendee names and emails are protected information ([[non-functional-requirements\|brief §8b]], [#91](https://github.com/SinYang13/IS212-2026/discussions/91)), and the client key is publishable. Policies must land before this schema meets real attendee data | [#56](https://github.com/SinYang13/IS212-2026/discussions/56) |
| Coordinator-entered registrations for attendees who do not self-register | Not built | [#33](https://github.com/SinYang13/IS212-2026/discussions/33) |
| **Attendee identity: XOR vs always-present.** [[schema]]'s `registration_attendee_identity_chk` requires **either** an account **or** a name-and-email pair, never both. The build treats name and email as always present, with the account link optional on top. The two agree for every Release 1 row, since no attendee has an account; they diverge the moment an authenticated attendee exists, because under the XOR that row carries no email and so falls outside the duplicate index | **Open — needs one decision.** [[schema]] is the model of record, so the XOR stands unless the team changes it; recorded here so the divergence is not discovered at the migration | [#53](https://github.com/SinYang13/IS212-2026/discussions/53); [[schema]] |
| How a guest identifies itself to *view status* or *withdraw* | Open. [[schema]] mints a `registration_reference` UUID for the purpose, but no delivery route (link in a confirmation, reference-plus-email lookup) is chosen or built | [[release-1-scope]]; [#53](https://github.com/SinYang13/IS212-2026/discussions/53) |
