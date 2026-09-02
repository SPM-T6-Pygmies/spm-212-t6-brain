---
title: User Stories Assignment — Scrutiny and Proposed Changes
type: review
sources:
  - "User Stories Assignment.md"
  - "IS212 User Story Standard — v1.0.md"
tags: [review, user-stories, standard-compliance, backlog, scrutiny]
---

# User Stories Assignment — Scrutiny and Proposed Changes

A scrutiny of `raw/User Stories Assignment.md` against `raw/IS212 User Story Standard — v1.0.md`, using the standard's own [Scrutiny Rubric (§13)](#) as the checklist. Every claimed rule in the assignment is additionally checked against what the customer actually said, as recorded in this wiki's domain and concept pages (which cite `Customer Brief no. 1` and the Aug 31 discussions directly). This page is a review artefact, not a domain/concept/source/entity page, so it sits outside the four-type taxonomy in `CLAUDE.md`; it is linked from [[index]] so it isn't an orphan.

> [!info] Note on page type
> This page doesn't fit `source-summary | concept | entity | domain-object`. It records a one-off scrutiny of a raw document, not standing knowledge about ConnectSphere. If the team wants a permanent "reviews" layer, that's a `CLAUDE.md` change to propose separately — not assumed here.

## How to read this

For each story: **Verdict** (`READY | NEEDS WORK | REJECT`), the failing Gate A/B/C/D checks, a **defects table** (check → quoted text → why it fails → fix), open **questions** the customer would need to answer, and a **split** recommendation where §7 applies. Every story that isn't already `READY` also gets a **✅ Proposed rewrite** — a complete replacement card (title/priority/estimate/story/AC), grounded only in what this wiki's sourced domain and concept pages actually establish, with genuine gaps left as an explicit open question rather than invented. Citations use `[[wikilink]]` for wiki pages and `#N` for GitHub discussion threads, exactly as the rest of this wiki does.

---

## 0. Top-level summary

| # | Story | Verdict | Headline defect |
|---|---|---|---|
| U1 | Log in to the system | NEEDS WORK | Invented, uncited session/UI rules (§5.5/5.6) |
| U2 | Log out of the system | NEEDS WORK | Invented inactivity-timeout number; role not in customer's vocabulary |
| U3 | Role-appropriate navigation | NEEDS WORK | "As a user" (A3); ignores multi-role users (#56) |
| U4 | Clear access-denied response | NEEDS WORK | "As a user" (A3); otherwise sound |
| E1 | Update status of returned equipment | NEEDS WORK | Status enum vague ("etc."); omits defect logging (#86) |
| E2 | Reserve suitable equipment for an event request | NEEDS WORK | WHY restates WHAT; omits transfer/prep time rule (#2) |
| E3 | Clarify requirements with Event Coordinator | READY (minor) | Solid; minor title/content drift |
| V1 | Automatic Venue Availability Update upon Booking Confirmation | NEEDS WORK | Wrong state name ("Unavailable" vs. "Confirmed"); date/time instead of slot |
| V2 | Cancel Venue Booking and Free Up Availability | **REJECT** | Contradicts the customer's hard rule that only the assigned Event Coordinator may cancel (#68) |
| V3 | Update Venue Specifications in Catalogue | READY (minor) | Solid, well-cited |
| V4 | Indicate venue is unavailable due to maintenance | NEEDS WORK | Blocks by date/time range, not by slot (#50) |
| V5 | Review and Decide on Venue Booking Requests | NEEDS WORK | Status enum typo ("Reject" not "Rejected") |
| V6 | Configure Venue Turnaround and Setup Buffer | **REJECT** | Card body is a copy-paste of V5; the actual feature is unwritten |
| Reg1 | Register for an event | NEEDS WORK | Assumes an undefined mandatory-field set (`#22`); no waiting-list branch |
| Reg2 | Enable registration for an event | NEEDS WORK | Per-card audit AC duplicates the global auditability NFR |
| Reg3 | Set registration capacity for an event | READY (minor) | Best-grounded card in the document; resolves two real open questions without citing them |
| EV1 | Assignment of event requests | NEEDS WORK | "Eligible" coordinators and assignment "failure" both contradict #93/#95 |
| EV2 | Submission of event requests | NEEDS WORK | Omits the customer's draft-save feature; assumes a mandatory-field set the customer hasn't fixed (#72) |
| EV3 | Review and clarification of event requirements | **REJECT** | Title promises clarification; only 2 ACs exist and neither covers it (fails B1) |

Also failing on **every single card, with no exception**: Gate A7 (no Priority, no Estimate, no Epic) and Gate D1 (no citation to any customer source anywhere in the document).

---

## 1. Cross-cutting defects (apply to most or all cards)

These are worth fixing once, as a policy, rather than 15 times individually.

| # | Finding | Standard ref | Real-requirement ref |
|---|---|---|---|
| X1 | **No card carries Priority, Estimate, or Epic.** The card anatomy (§3) requires all three; Gate A7 fails on every one of the 15 cards. | §3, Gate A7 | — |
| X2 | **Zero citations anywhere in the document.** Every rule invented by this backlog (session timeouts, "eligible" coordinators, arbitrary date/time blocking) sits next to rules the customer did state, with no way to tell which is which. Gate D1 requires every rule to trace to something the customer said or be flagged as an open question. This wiki's domain/concept pages already carry that traceability for the same domain — cards should cite them (or the underlying `#N` discussion) directly. | Gate D1 | [[open-questions]] |
| X3 | **Generic role usage.** "As a user" (U3, U4) and "As an internal staff member" (U2) are used where the customer's own vocabulary — Event Organiser, Event Coordinator, Venue Staff, Technical Support Staff, Attendee, Event Operations Manager — is available and well-documented. A3 and D3 both fail here. | A3, D3 | [[user-roles]] |
| X4 | **Invented rules presented as settled fact instead of flagged as open questions.** Session inactivity timeout, password masking, coordinator "eligibility", assignment "failure" — none of these trace to the customer. The standard's own worked example (§5.6, the TLS/2-second deviation) is the template for how to handle this: either the customer stated it, or it's deleted / moved to a Definition of Done / raised as a question. None of the four options was taken here. | §5.5, §5.6, B6 | [[access-control]], [[user-roles]] |
| X5 | **A structural copy-paste bug.** "Configure Venue Turnaround and Setup Buffer" (V6) is a verbatim copy of "Review and Decide on Venue Booking Requests" (V5) — same story sentence, same ACs, different title. Looks like a save/paste error rather than a drafting decision. See §4 for the detail and the proposed fix. *(An earlier draft of this document also flagged the entire `## Registration` section as a duplicate of `## Venue`; that has since been corrected in the source with real registration content — see §5, which now reviews the real cards.)* | — | — |

---

## 2. Users

### U1 — Log in to the system

**Verdict: NEEDS WORK** — Gate A: pass (borderline on A3) · Gate B: fail (B6) · Gate D: fail (D1)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| A3 (borderline) | "As a user, I need to log in..." | Login is the standard's own cited exception ("a genuinely universal capability where every role behaves identically") — but the story's own AC ("Is taken to the view appropriate to their role after logging in") proves behaviour *does* differ by role. If outcome differs by role, the role enumeration hasn't actually been done here, it's just deferred to a later AC. | Keep "As a user" only if the team explicitly records the exception and why (behaviour differs post-login, not during authentication itself); otherwise note the tension on the card. |
| B6 | "Password is masked as it is enter[ed]." | No source specifies UI-level password masking. Not wrong, but it's an invented UI rule dressed as an AC rather than flagged. Compare the standard's own TLS example (§5.6): global engineering/UI hygiene belongs in the Definition of Done, not repeated per card. | Move to Definition of Done, or delete. |
| B6 / D1 | "GIVEN a user enters an incorrect password WHEN they submit the login form THEN a message advises them to check their credentials and no session is created" | Mostly restates the two bullets above it (T3) — the only new information is "no session is created," which is worth keeping. | Compress: delete the restated bullets, keep the GWT for the one genuinely new fact. |

**Questions for the customer/team:** [[access-control]] confirms the authentication method itself is unspecified — email/password, SSO and MFA were all asked about and none was chosen (`#62`). This card should not assume password-based login as settled; it should say so explicitly as an open item, the way [[user-account]] does.

> [!success] Proposed rewrite
> **Title:** Log in to the system · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a user — invoking the §4.2 "genuinely universal capability" exception: the authentication mechanism itself is identical for every role; only the post-login destination varies, and that's expressed as a conditional AC below, not folded into the role — I need to log in with my credentials so that I can access the functions permitted for my role.
>
> **AC:**
> - Can log in with a registered email address
> - Cannot log in with an unregistered email address or an incorrect password; told only that the credentials are invalid, without indicating which field was wrong
> - Given invalid credentials are submitted, then no session is created
> - Is taken to the view appropriate to their role after logging in (see [[access-control]] "Role-Based Dashboards")
>
> **Open questions:**
> - Authentication method is unspecified by the customer — email/password, SSO, and MFA were all raised and none was chosen (`#62`, see [[user-account]]). This card assumes password-based login only because [[access-control]] treats it as the working default; confirm before build.
> - Account recovery and locked-account handling are unspecified (`#64`) — needs its own card once resolved, don't fold into this one.
>
> **Removed from the original:** "Password is masked as it is entered" (UI hygiene → Definition of Done, per the standard's own TLS example in §5.6); the GWT scenario was compressed into the "no session is created" bullet above since the rest of it restated existing bullets (T3).

---

### U2 — Log out of the system

**Verdict: NEEDS WORK** — Gate A: fail (A3/D3) · Gate B: fail (B6)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| A3 / D3 | "As an internal staff member" | Not a role in the customer's vocabulary. The brief names four internal roles (Event Coordinator, Venue Staff, Technical Support Staff, plus the discussion-only Event Operations Manager) — see [[user-roles]]. "Internal staff member" collapses all of them into a bucket the customer never used. | If the behaviour genuinely is identical for all internal roles, say so explicitly and name the roles it covers; otherwise write one story per role per §4.2. |
| B6 | "Is logged out automatically after a period of inactivity defined by ConnectSphere" | No source mentions a session-inactivity rule anywhere — not in [[access-control]], not in [[non-functional-requirements]]. This is the exact anti-pattern the standard calls out in §5.6: an invented threshold dressed as a customer-owned decision ("defined by ConnectSphere") when no such definition exists. | Raise as an open question for the customer, alongside the already-open account-recovery question (`#64`), rather than asserting it as an AC. |

The WHY clause ("so that my account is not left accessible on a shared or mobile device used during venue setup") is genuinely good — it's falsifiable and ties directly to the brief's stated mobile/on-site usage pattern ([[non-functional-requirements]] §c, brief s7). It does, however, narrow the beneficiary to on-site internal staff specifically — which is exactly why "internal staff member" got invented as a role in the first place. The cleanest fix is to broaden the WHY to the benefit every role actually shares (a session left open is a risk regardless of who's holding the device), which turns the role genuinely universal and resolves A3 without picking an arbitrary role list.

> [!success] Proposed rewrite
> **Title:** Log out of the system · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a user, I need to log out of the system so that my session is not left open and accessible if my device is shared, mobile, or left unattended.
>
> **AC:**
> - Can log out from any page in the system
> - Is returned to the login page after logging out
> - Cannot return to a protected page using the browser back button after logging out
>
> **Open question:** whether a session-inactivity timeout is required at all, and if so what duration, is not stated anywhere in the brief or the discussions (checked against [[access-control]] and [[non-functional-requirements]] §b — neither mentions one). Raise with the customer; don't assert "a period of inactivity defined by ConnectSphere" when no such definition exists yet.
>
> **Removed from the original:** the automatic-inactivity-logout AC (no customer source; see the open question above) and the "internal staff member" / "venue setup" framing (narrowed the role for no stated reason — see note above).

---

### U3 — Role-appropriate navigation

**Verdict: NEEDS WORK** — Gate A: fail (A3) · Gate B: fail (B8, B6)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| A3 | "As a user, I need to see only the functions available to my role" | Same generic-role problem as U2/U4. | Rewrite per role, or explicitly invoke the "genuinely universal capability" exception and say why. |
| B8 (gap) | *(no AC addresses this)* | The customer confirmed a person may hold multiple roles at once (`#56`) — a Venue Staff member may also be an Attendee elsewhere. Nothing in this card's ACs addresses what navigation shows for a multi-role account. This is a real, customer-confirmed rule the card is missing, not an edge case to invent. | Add: "Given an account holds more than one role, the navigation shows the union of functions permitted across all of that account's roles." |
| B6 | "GIVEN a user's role is changed WHEN they next log in THEN the navigation reflects..." | No source states that role changes only take effect on next login (vs. mid-session), or that role reassignment is even a modelled operation — [[user-account]] states explicitly that no account-level lifecycle beyond the roles held is specified. | Flag as an open question rather than asserting the next-login mechanic as fact. |

> [!success] Proposed rewrite
> **Title:** Role-appropriate navigation · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a user — §4.2 exception: every role experiences the same mechanism (see only permitted functions), even though the specific functions shown differ — I need to see only the functions available to my role(s) so that I am not presented with actions I am not permitted to perform.
>
> **AC:**
> - Can see navigation options for the functions permitted to their role
> - Cannot see navigation options for functions not permitted to their role
> - Given an account holds more than one role, then navigation shows the union of functions permitted across all roles held (`#56`, see [[user-account]] Binding rules)
> - Navigation is usable on both desktop and mobile screen sizes ([[non-functional-requirements]] §c; brief §7 — internal staff need mobile access while preparing venues or equipment)
>
> **Open question:** whether a role change takes effect immediately or only at the account's next login is not stated by the customer — [[user-account]] confirms no account-level lifecycle rule exists at all. Don't assume "next login" as the original card did; raise it.
>
> **Added vs. the original:** the multi-role union AC (a real, `#56`-confirmed rule the original card omitted entirely).

---

### U4 — Clear access-denied response

**Verdict: NEEDS WORK** — Gate A: fail (A3) only

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| A3 | "As a user, I need to be told clearly when I do not have access..." | Generic role again. | As above. |

Otherwise this is the strongest card in the Users section: "does not reveal whether the requested record exists" is exactly the kind of non-obvious, security-relevant rule the standard rewards (T3), and it lines up with [[access-control]]'s protected-information model (`#91`) — access is role **and** relationship, and a denial message that leaked existence would itself be a protected-information leak. Worth citing that page directly on the card.

> [!success] Proposed rewrite
> **Title:** Clear access-denied response · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a user — §4.2 exception: the denial mechanism is identical regardless of role — I need to be told clearly when I do not have access to something so that I know to contact the right person rather than assume a system fault.
>
> **AC:** *(unchanged from the original — they already pass Gates B and C)*
> - Is shown a message stating that they do not have access when they attempt to reach a resource not permitted to them
> - The message does not reveal whether the requested record exists or any of its details (see [[access-control]] "Protected Information", `#91` — a leak here would itself be a protected-information violation)
> - The message indicates who to contact to request access
> - Can return to a permitted page from the access-denied message

---

## 3. Equipment

### E1 — Update status of returned equipment

**Verdict: NEEDS WORK** — Gate B: fail (B2, B8)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| B2 | "Can change the status to one of the defined statuses (available, under maintenance, etc.)" | "etc." is not binary/enumerable (T1) — a reviewer can't check this against a fixed list. The customer *did* fix the list: exactly four values — available, unavailable, damaged, under maintenance (`#23`; see [[equipment-item]] States). | Enumerate all four explicitly; delete "etc." |
| B8 (gap) | *(no AC covers this)* | The title says "returned equipment," which in the real workflow is specifically where defect/damage logging happens: on return, Technical Support Staff record defects, damage or missing items, and status updates accordingly (`#86`, see [[equipment-reservation]]). None of the ACs mention this — the card is generic status-editing, not the return-inspection scenario its own title promises. | Add an AC for recording a defect/damage note at return, or retitle the card to the generic scope it actually has. |

Minor: "Only Technical Support Staff can change equipment status" is well-grounded (`#9`), but [[equipment-item]] flags this as `⚠️ Partial` — the source answer confirms TSS's responsibility without explicitly ruling out an Event Coordinator flag. Fine as an AC, but don't treat the exclusivity as more settled than the source does.

> [!success] Proposed rewrite
> **Title:** Update status of returned equipment · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As Technical Support Staff, I need to update the operational status of a piece of equipment so that other staff can see whether it's available, damaged, unavailable, or under maintenance before relying on it for an event.
>
> **AC:**
> - Can view a piece of equipment's current operational status
> - Can set the status to exactly one of: available, unavailable, damaged, or under maintenance — no other values (`#23`, see [[equipment-item]] States)
> - Only Technical Support Staff can change equipment status (`#9` — ⚠️ the source confirms TSS's responsibility without explicitly ruling out a Coordinator-initiated flag; treat as the working default, not an airtight exclusion)
> - Given equipment is returned from an event, when Technical Support Staff record a defect, damage, or missing item, then the operational status is updated to match (`#86`, see [[equipment-reservation]] "Defect / return-inspection log")
> - Updated status is immediately reflected on the equipment catalogue and on any in-progress reservation review
>
> **Changed vs. the original:** enumerated the exact four status values instead of "etc." (B2); added the return-inspection/defect-logging AC the title promised but the original never delivered (B8); tightened "wherever the equipment is displayed" to the two screens that actually matter, so the AC stays checkable (T1).

---

### E2 — Reserve suitable equipment for an event request

**Verdict: NEEDS WORK** — Gate A: fail (A5) · Gate B: fail (B8)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| A5 | "so that I can reserve the available equipment" | The WHY restates the WHAT (reserving) rather than stating a benefit from it — it's the same action, not an outcome. | e.g. "so that the event's equipment needs are met without manually cross-checking spreadsheets" (ties to the brief's stated pain point). |
| B8 (gap) | "List excludes equipment that is not available or already reserved for an overlapping period" | Correctly captures the D-1 → Return Day+1 commitment window (`#5`), but omits the other half of the customer's stated availability rule: transfer/preparation time from the item's *current location* to the venue also affects pre-event availability (`#2`) — see [[equipment-item]], Availability lifecycle. An item can be "free" by the D-1/+1 rule and still be unreachable in time. | Add an AC covering transfer/preparation time, or explicitly flag it as deferred (representation of transfer time is itself a `🔶` team decision per `#2`). |

Otherwise this is one of the better-grounded cards in the assignment: type/quantity/location/status on the listing, reserve-up-to-available-quantity, and date/time-based exclusion all map directly to customer-specified attributes and rules.

> [!success] Proposed rewrite
> **Title:** Reserve suitable equipment for an event request · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As Technical Support Staff, I need to see a list of suitable equipment for an event request so that the event's equipment needs are met without manually cross-checking spreadsheets.
>
> **AC:**
> - Can view the equipment requirements recorded for an event request
> - Can view possible equipment matches of the requested type for the event's date and time, accounting for the transfer/preparation time needed to move each candidate item from its current location to the venue (`#2`, see [[equipment-item]] "Availability lifecycle")
> - List excludes equipment that is operationally unavailable, damaged, or under maintenance (`#23`), or already committed to another event's D-1 → Return Day+1 window for an overlapping period (`#5`)
> - Each listed item shows its type, available quantity, location, and status
> - Can reserve listed equipment against the event request, up to the available quantity (`#13`)
>
> **Open question:** how transfer/preparation time and an item's current location are represented is explicitly left to the team (`#2`) — needed before the "matches... accounting for transfer time" AC above can actually be built and tested.
>
> **Changed vs. the original:** fixed the WHY (was restating the action); added the transfer-time factor the original silently dropped; split the single "not available or already reserved" bullet into its two distinct customer rules (operational status vs. commitment window) so each is independently checkable (T5).

---

### E3 — Clarify requirements with Event Coordinator

**Verdict: READY, with a naming note** — no Gate A/B/C failures found beyond the cross-cutting A7/D1 issues.

This card matches the real partial-fulfilment flow closely: flag-with-reason, optional suggested alternative, visibility to the assigned coordinator, and a notification trigger all correspond to `#90` and [[equipment-reservation]]'s Unavailable state. One drift worth naming: the title says "Clarify requirements," which in the real process is a separate scenario (`#87` — TSS raising a clarification question when *information is unclear*, routed through the coordinator as sole liaison) from what the ACs actually describe (`#90` — flagging equipment that is *unavailable*, not unclear). Consider retitling to "Flag an unmet equipment requirement" to match the ACs, and treat the `#87` clarification-question flow as a separate card if it's needed.

---

## 4. Venue

### V1 — Automatic Venue Availability Update upon Booking Confirmation

**Verdict: NEEDS WORK** — Gate B: fail (B5/B6, B9) · Gate D: fail (D3)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| D3 | "set the venue's status for those dates and times to 'Unavailable'" | Two problems in one line. First, [[venue]]'s State table names the per-slot state entered on booking confirmation **Confirmed**, not "Unavailable" — "Blocked"/"otherwise unavailable" are a *different*, Venue-Staff-driven state (maintenance etc.), not the state a confirmed booking produces. Second, the customer's model books venues in three fixed **slots** (AM/PM/Night, `#50`), not arbitrary "dates and times" — this same card correctly uses "Venue Availability Calendar" (the customer's actual term) two lines later, so the terminology is inconsistent within the same card. | "...set the venue's status for the booked slot(s) to 'Confirmed' in the Venue Availability Calendar." |
| B9 | The whole AC block | Written as an unlabelled run of bullet points that reads like a Given/When/Then scenario missing its "When" (there's a Given, then several unconditional system-shall statements). | Either write it as a proper Given-When-Then ("**Given** a pending request, **when** Venue Staff approve it, **then**...") or as a flat checklist — not a hybrid. |

The underlying rule (hard block on double-booking, one active hold/confirmed booking per venue+period) is correctly sourced to `#35`/`#41` — this card just needs its terminology aligned to [[venue]] and [[booking]].

> [!success] Proposed rewrite
> **Title:** Automatic Venue Availability Update upon Booking Confirmation · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a Venue Staff member, I want the system to automatically mark a venue's slot as confirmed once I approve a booking, so that we prevent double bookings.
>
> **AC:**
> - Given a pending venue booking request for a specific venue and slot(s), when Venue Staff approve and confirm the request, then the system sets the venue's status for those slot(s) to "Confirmed" in the Venue Availability Calendar (see [[venue]] States)
> - Given a venue slot is Confirmed, when another booking request is submitted for the same venue and an overlapping slot, then the request is blocked outright, not merely warned (`#35`, `#41`)
> - The occupancy used for conflict detection includes the venue's setup time before, and turnaround/reset time after, the published event times — not the published times alone (brief §4, see [[booking]] "Effective occupancy ≠ published event times")
>
> **Changed vs. the original:** "Unavailable" → "Confirmed" (matches [[venue]]'s actual state name — "Unavailable"/"Blocked" is a different, maintenance-driven state); "those dates and times" → "those slot(s)" (matches the fixed AM/PM/Night model, `#50`); reshaped into a proper Given/When/Then instead of a Given followed by unconditional statements (B9); added the setup/turnaround padding rule, which the original omitted.

---

### V2 — Cancel Venue Booking and Free Up Availability

**Verdict: REJECT** — Gate D: fail (D1, D4) — contradicts a customer-committed hard rule

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| D4 (contradiction) | "Venue staff is notified that the event coordinator wants to cancel the booking. Venue staff goes to that booking and changes status from 'Confirmed' to 'Cancelled'." | This gives cancellation authority to Venue Staff, acting on the Event Coordinator's request. The customer's answer to `#68` is one of only eight hard, non-negotiable rules in this whole project (see [[index]]'s "Hard rules" callout): **only the assigned Event Coordinator may cancel, at any stage.** No source describes a notify-Venue-Staff-then-Venue-Staff-executes cancellation workflow for a booking as distinct from the event. | Rewrite with the Event Coordinator as the actor who changes the booking status; Venue Staff's role, if any, is to be notified that the slot has been freed, not to perform the cancellation. |

The status label "Cancelled" also isn't the terminal state name used elsewhere in the wiki ("Released" — see [[booking]] States), though that label is itself a `🔶` team decision, not customer-specified, so this is a minor consistency note rather than a defect on its own.

> [!success] Proposed rewrite
> **Title:** Cancel Venue Booking and Free Up Availability · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As the assigned Event Coordinator, I want to cancel a venue booking, so that the venue's slot is immediately freed up for other coordinators to book.
>
> **AC:**
> - Only the assigned Event Coordinator for the event may cancel its venue booking, and may do so at any stage (`#68` — one of the project's eight non-negotiable rules, see [[index]]'s "Hard rules" callout)
> - Given the assigned Event Coordinator cancels a Confirmed booking, then its status changes to Released and the Venue Availability Calendar immediately shows the venue's slot(s) as Available again (see [[booking]] States)
> - Venue Staff are notified that the booking has been cancelled, since they own physical venue preparation ([[notifications]] — "a venue booking is requested, approved, rejected, or changed")
>
> **Open question:** whether Venue Staff ever need their own cancellation trigger (e.g. a venue becoming permanently unusable) is a *different* scenario from Event-Coordinator-initiated cancellation, and `#68` doesn't speak to it — if needed, that's a separate card, not a reason to reopen this one to a second actor.
>
> **Changed vs. the original:** role corrected from "venue staff member" to "the assigned Event Coordinator," per the hard rule the original card contradicted; Venue Staff's role is now recipient of a notification, not the actor performing the cancellation.

---

### V3 — Update Venue Specifications in Catalogue

**Verdict: READY**, beyond the cross-cutting A7/D1 issues.

"Maximum capacity, supported layouts, accessibility features, and available facilities" maps one-to-one onto [[venue]]'s Attributes table. Operating hours and turnaround/setup time are deliberately left to a sibling card (V6) — a legitimate Operations-pattern split — though see V6 below for what actually happened to that sibling.

---

### V4 — Indicate venue is unavailable due to maintenance

**Verdict: NEEDS WORK** — Gate D: fail (D3)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| D3 | "specifying a venue, start date/time, end date/time, and reason" | [[venue-booking]] states blocking is done at **slot** granularity (`#50`) — "enabling partial-day unavailability" is achieved by blocking individual AM/PM/Night slots, not by an arbitrary start/end timestamp range. Arbitrary date/time blocking is a different (unrequested) model. | "...specifying a venue, one or more slots (AM/PM/Night) per affected day, and a reason." |

The "Blocked" state name is correct here (matches [[venue]] States exactly) — the defect is scoped to granularity, not terminology.

> [!success] Proposed rewrite
> **Title:** Indicate venue is unavailable due to maintenance · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a venue staff member, I want to block a venue's availability for operational reasons such as maintenance or renovation, so that coordinators cannot request bookings during those periods.
>
> **AC:**
> - Venue Staff is on the Venue Unavailability Management screen
> - They create an unavailability block specifying a venue, one or more affected slots (AM/PM/Night) per affected day, and a reason (e.g. "Maintenance") (`#50`, see [[venue-booking]] "Venue Unavailability and Blocking" — blocking is slot-granularity, enabling partial-day unavailability)
> - The Venue Availability Calendar displays the blocked slot(s) as "Blocked" (see [[venue]] States)
> - The system prevents Event Coordinators from completing a booking request for this venue if it overlaps a blocked slot
>
> **Changed vs. the original:** "start date/time, end date/time" → "one or more slots (AM/PM/Night) per affected day," matching the customer's fixed slot model rather than an arbitrary timestamp range. Everything else in the original card was already correctly grounded.

---

### V5 — Review and Decide on Venue Booking Requests

**Verdict: NEEDS WORK** — Gate B: fail (B2)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| B2 | "System will change the status of the booking request accordingly- either 'Approved' or 'Reject'" | "Reject" is not a valid past-tense status value alongside "Approved" — a small thing, but it means the AC as literally written can never be satisfied (T1: a reviewer checking the literal enum would find no story ever reaches "Reject"). | "...either 'Approved' or 'Rejected'." |

Otherwise well-grounded: approve/reject-with-reason-or-alternative and a coordinator notification both match `#45` closely.

> [!success] Proposed rewrite
> **Title:** Review and Decide on Venue Booking Requests · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As a venue staff member, I want to review pending booking requests and provide approval or rejection details, so that coordinators are clear on the outcome of their requests.
>
> **AC:**
> - The Venue Staff gets notified of a new venue booking request submitted by an Event Coordinator ([[notifications]])
> - The Venue Staff reviews the request and indicates whether to approve or reject it
> - If rejected, they can input a reason for rejection and/or suggest an alternative venue or arrangement — an informal suggestion, not a formal counter-offer (`#45`, see [[booking]] States)
> - The system changes the status of the booking request accordingly, to either "Approved" or "Rejected"
> - The system notifies the Event Coordinator of the outcome
>
> **Changed vs. the original:** "Reject" → "Rejected" (the enum value has to actually be reachable); noted the "informal suggestion, not counter-offer" nuance from `#45` so the alternative-arrangement AC isn't over-read as creating a new object type.

---

### V6 — Configure Venue Turnaround and Setup Buffer

**Verdict: REJECT** — the card's story and ACs are a verbatim copy of V5 ("Review and Decide on Venue Booking Requests"). The title promises a real, well-documented customer feature — Venue Staff record setup and turnaround/reset time **per venue** (`#43`) — that the card body never actually describes. This needs a full rewrite, not a patch. Proposed replacement:

> **Title:** Configure Venue Turnaround and Setup Buffer · **Priority:** *(set relative to rest of backlog)* · **Estimate:** *(size in planning)*
>
> **Story:** As a venue staff member, I want to record the setup time and turnaround/reset time for a venue, so that bookings for that venue are automatically padded with the correct preparation and reset windows.
>
> **AC:**
> - Can set a setup time (before the published start) and a turnaround/reset time (after the published end) independently for each venue (`#43`)
> - Given a venue booking's effective occupancy is calculated, when the venue has a setup and/or turnaround time recorded, then that time is added to the published event start/end to determine the padded occupancy window (brief §4, *Setup and Turnaround Time*; see [[booking]] "Effective occupancy ≠ published event times")
> - Given a new booking request would overlap another booking's padded occupancy window, then the request is blocked, consistent with the double-booking rule (`#35`)
> - Setup and turnaround values are per-venue, not a single system-wide default (`#43`)
>
> **Open question:** whether turnaround time sits *on top of* the fixed AM/PM/Night slot boundaries or is expected to be absorbed within the existing unbooked gaps (12–1pm, 6–7pm) is not stated by the customer — [[venue-booking]] flags this tension explicitly and it should go back to the customer, not be assumed.

---

## 5. Registration

*(An earlier pass of this review found the `## Registration` section byte-for-byte duplicating `## Venue`. That has been fixed at the source with three real registration cards, reviewed below. Thank you for the correction.)*

### Reg1 — Register for an event

**Verdict: NEEDS WORK** — Gate B: fail (B6/D5) · Gate B: pass on structure otherwise

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| B6 / D5 | "Registration is refused if mandatory fields are missing or invalid" | Presupposes a fixed, known mandatory-field set for registration. The customer was asked directly — "Name only? Dietary needs? Accessibility requirements?" — and returned it in full: "propose an appropriate set of information based on the event needs" (`#22`, see [[registration]] Open modelling questions). No mandatory set currently exists to validate against. Same pattern as EV2 below. | Keep the AC's shape, but add an explicit open question: registration field set is undecided by the customer (`#22`) and may vary per event ("based on the event needs") rather than being one global set — confirm before this AC is testable. |

Solid otherwise: the not-confirmed / confirmed-and-in-period preconditions correctly split across this card and Reg2 (confirmed-but-disabled is Reg2's job, not duplicated here — a legitimate Operations-pattern split, not a gap). "Receive event confirmation details... upon successful registration" is a reasonable inference but isn't itself one of the customer's listed notification triggers in [[notifications]] (which lists registration *opening/closing* and *capacity/waiting-list changes* as triggers, not a per-registration confirmation) — minor, flag as inferred rather than sourced if precision matters.

**B8 gap, not this card's fault but worth naming here:** none of the three Registration cards cover the **waiting list** — capacity is set and enforced (Reg3), but what happens when an Attendee registers for an event already at capacity is never specified anywhere in the backlog. The 3-day waiting-list acceptance window (`#44`) is one of only eight rules the customer has committed to outright (see [[index]]'s "Hard rules" callout) and currently has no card at all. See the recommended additions at the end of this section.

> [!success] Proposed rewrite
> **Title:** Register for an event · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As an Attendee, I want to register for an event so that I can secure my place.
>
> **AC:**
> - Given an event is not Confirmed, when I search for it, then it is unavailable for registration (brief §4, [[event-request-workflow]] step 11)
> - Given an event is Confirmed, registration is enabled, and I am within the registration period, when I submit a valid registration, then a registration is created and shown to me (`#30`, brief §4)
> - Given the event is at capacity when I submit, then I am placed on the waiting list instead of being registered (`#26`, brief §4 — see Reg4 below)
> - I receive confirmation of my registration (event name, date, time, venue)
>
> **Open question:** what information is mandatory to supply at registration is explicitly undecided by the customer — "propose an appropriate set of information based on the event needs" (`#22`, see [[registration]]) — and may vary per event rather than being one fixed global set. The "refused if mandatory fields are missing" AC can't be made testable until this is resolved; don't validate against an invented field list in the meantime.
>
> **Changed vs. the original:** dropped the unqualified "mandatory fields" validation AC in favour of an explicit open question (`#22`); added the at-capacity → waiting-list branch, which the original silently skipped even though the precondition ("within... event capacity") already implied it exists.

---

### Reg2 — Enable registration for an event

**Verdict: NEEDS WORK** — Gate B: fail (B7)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| B7 | "Given I am the Event Coordinator, when the change is saved, then the actor, the old value, the new value and the timestamp are written to the activity history." | This is the auditability NFR (`#14`, [[non-functional-requirements]] §f: record what/who/when) restated as a full per-field AC on one specific card. §5.5 is explicit about this pattern: "Repeating 'must use TLS' on forty cards is forty places to maintain one decision." If every state-changing action on every card gets its own actor/old/new/timestamp AC, the same global rule is now duplicated across the backlog instead of stated once. | State the what/who/when requirement once, in the Definition of Done, and drop this bullet from the card — unless enabling/disabling registration specifically needs *stronger* audit treatment than the system-wide default, in which case say so explicitly and why. |

Otherwise well-formed: role, precondition (assigned Coordinator) and the enabled/disabled visibility rule for Attendees all check out, and it correctly complements Reg1 rather than duplicating it.

> [!success] Proposed rewrite
> **Title:** Enable registration for an event · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As an Event Coordinator, I want to enable or disable registration for an event, so that only events that require attendee sign-up collect registration.
>
> **AC:**
> - Given I am the Event Coordinator assigned to an event, when I open its registration settings, then I can set registration to enabled or disabled
> - Given registration is disabled, when an Attendee views the event, then they cannot register for it
>
> **Removed from the original:** the "actor, old value, new value, timestamp written to activity history" AC — this restates the system-wide auditability requirement (`#14`, [[non-functional-requirements]] §f) rather than something specific to this story. State it once, in the Definition of Done, not per state-changing card (§5.5's own TLS example is the template for this exact move).

---

### Reg3 — Set registration capacity for an event

**Verdict: READY, with a citation note** — the best-grounded card in the document; no Gate A/B/C failures beyond the recurring B7 audit-AC pattern and the cross-cutting A7/D1 issues.

Two of this card's ACs quietly **resolve open customer questions** rather than restating settled fact — which is a legitimate exercise of delegated authority (the customer explicitly invited a team proposal in both cases), but the standard's D5 check ("open questions listed on the card... rather than silently resolved") means the resolution should be visible, not silent:

| AC | What it resolves | Where the question was open |
|---|---|---|
| "when I enter a capacity above the venue's maximum capacity, then the system warns me before saving" | Chooses **overridable warning**, not a hard block, for over-capacity. The brief's own rule is hedged — "a venue should **not normally** be treated as suitable when expected attendance exceeds its capacity" — and neither discussion snapshot tightened it. [[venue-suitability]] lists "over-capacity: block or warn" as an explicit open team decision. | [[venue-suitability]] "Capacity vs Expected Attendance" |
| "when I enter a capacity, then the value is stored and shown on the event record" (i.e. set independently of the venue) | Chooses **coordinator-set independently, cross-checked against venue capacity**, over deriving the registration cap from the booked venue automatically. [[attendee-registration]] lists this relationship as explicitly unresolved by the brief. | [[attendee-registration]] "Team decision required — event vs venue vs registration capacity" |

Neither is wrong — both are sensible resolutions squarely within what the customer delegated — but per D5 the card should say "this resolves the venue-vs-registration-capacity question raised in [[venue-suitability]]/[[attendee-registration]]" so a reviewer doesn't mistake a team decision for a customer-stated rule.

The recurring B7 pattern applies again to the last AC ("the change is recorded in the change history") — same fix as Reg2: state once, globally, rather than per card.

> [!success] Proposed rewrite
> **Title:** Set registration capacity for an event · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As an Event Coordinator, I want to set the maximum number of registrations for an event, so that sign-ups stay within what the venue and programme can accommodate.
>
> **AC:**
> 1. Given registration is enabled, when I enter a capacity, then the value is stored and shown on the event record — set independently of the venue's own capacity, resolving the open question of whether registration capacity is coordinator-set or venue-derived ([[attendee-registration]] "Team decision required — event vs venue vs registration capacity")
> 2. Given the event has a confirmed venue, when I enter a capacity above the venue's maximum capacity, then the system warns me before saving — resolving the brief's hedged "should not normally be treated as suitable" rule as an overridable warning rather than a hard block ([[venue-suitability]] "Capacity vs Expected Attendance")
> 3. Given a capacity is set, when I attempt to lower it below the current number of confirmed registrations, then the system warns me and requires confirmation
> 4. Given the capacity is reached, when the Coordinator views the event, then the registration count and remaining places are visible
>
> **Removed from the original:** the "recorded in the change history" AC — same Definition-of-Done move as Reg2.
>
> **Changed vs. the original:** ACs 1 and 2 now name, in-line, which previously-open customer question each one resolves, per D5 — so a future reader can tell this was a deliberate team decision within the customer's delegated scope, not an overlooked ambiguity.

---

### Recommended additions — still missing from Registration

Even with Reg1–Reg3 in good shape, three customer-confirmed registration capabilities have no card anywhere in the backlog:

> **Reg4 — Be Promoted From the Waiting List**
> As an Attendee, I want to be offered a released spot from the waiting list in the order I joined, so that I have a fair chance to attend if space frees up.
> - Waiting-list offers are made in FIFO order, subject to a manual override (`#26`)
> - Given an Attendee is offered a spot, when they respond within 3 days, then they are registered (`#44`)
> - Given an Attendee does not respond within 3 days of being offered a spot, when the window lapses, then the offer expires (`#44`)
> - **Open question:** what happens to the freed spot once an offer expires — auto-pass to the next queued entry, return this entry to the back of the queue, or removal — is not specified and blocks estimation of the full flow (`#44`)
> - **Open question:** who may exercise the manual override is not named by the customer; the Event Coordinator is the natural candidate given they own the registration period (`#30`), but this needs confirmation, not assumption (`#26`)

> **Reg5 — Withdraw an Event Registration**
> As an Attendee, I want to withdraw my registration for an event, so that my place can be released to someone else if I can no longer attend.
> - Can withdraw a Registered registration (brief §4)
> - Given an Attendee withdraws and a waiting list exists for that event, when the withdrawal is processed, then the next queued entry is offered the freed spot (`#44`, brief §4)
> - **Open question:** whether withdrawal has a cut-off before the event date is not stated (brief §4 says only "where permitted"); note there is no general change cut-off for events either (`#8`) — raise with the customer rather than assume a deadline.

> **Reg6 — Check In a Registered Attendee**
> As *[role not named by the customer — see open question]*, I want to check an Attendee in on arrival by scanning a QR code or entering their details manually, so that ConnectSphere has an accurate attendance record distinct from registration status.
> - Can check in an Attendee via QR-code scan or manual entry (`#38`)
> - Only Attendees with a Registered status can be checked in (brief §4 requires registration and attendance to stay distinguishable)
> - Attendance is not self-reported and is not auto-marked without staff action (`#38`)
> - **Open question:** which internal role performs check-in is not named — the source says only "manually by staff" (`#38`). Do not default to Event Coordinator or Venue Staff without confirming; both are plausible and the choice affects who this story is written for.

**Recommend a spike, not a story, for the VIP / non-self-registering-attendee case:** per `#33`, an Event Organiser cannot add attendees directly; the list instead goes to the Event Coordinator "for processing" — but the mechanism (manual entry, bulk import, or off-system handover with only a headcount recorded) is entirely unspecified, as is whether these attendees consume ordinary capacity/waiting-list slots. Per §6.1 (Estimable) and §1 (Spike), this has too many unknowns to write a sized, testable story yet.

---

## 6. Event

### EV1 — Assignment of event requests

**Verdict: NEEDS WORK** — Gate B: fail (B6 ×2) · Gate D: fail (D5)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| B6 | "Be able to see all the Event Coordinators that are eligible for assignment" | Contradicts the source directly: coordinator selection is "**entirely up to the Event Operations Manager**, who follows an SOP maintained **outside the system** — **no [selection] criteria need to be encoded**" (`#93`, see [[user-account]] Binding rules). "Eligible" implies the system computes a filtered subset, which is scope the customer explicitly declined to ask for. | "Be able to see all Event Coordinators" (full stop) — selection judgement stays with the manager, off-system. |
| B6 | "Be informed if assignment of Event Coordinator has failed" | No failure mode exists in the source. `#95`: there is no acceptance/decline step; assignment "takes effect immediately"; [[event-request-workflow]] explicitly characterises assignment as "a simple write, not a workflow... no approval chain, no acceptance state." An AC for assignment "failure" invents a rejection path the customer ruled out. | Delete, unless the team means only a technical/system error (in which case say so, since that's a different and much less interesting claim). |
| D5 (gap) | *(not surfaced on the card)* | Whether the Event Operations Manager is even a formally modelled role is an open gap the customer never closed ([[user-roles]] — the role appears only in the Q&A, absent from the brief's role table). The card should carry this as a listed open question per D5, not silently proceed as if the role were settled. | Add an explicit open-question line. |

Also a **B8 gap**: reassignment of a coordinator to a different one is a separate, customer-confirmed capability (`#94`) not covered anywhere in this backlog. Recommend a second card ("Reassign an Event Coordinator") rather than folding it silently into this one.

> [!success] Proposed rewrite
> **Title:** Assignment of event requests · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As an Event Operations Manager, I want to see all pending event requests, so I can assign an Event Coordinator to each event.
>
> **AC:**
> - Receive a notification that a new event has been requested ([[notifications]])
> - Be able to see all the pending event requests
> - Be able to see all Event Coordinators — selection is entirely the manager's own judgement, following an SOP maintained outside the system; the system does not compute eligibility (`#93`, see [[user-account]] Binding rules)
> - Be able to assign an Event Coordinator to an event; the assignment takes effect immediately with no acceptance step (`#95`)
>
> **Open question:** whether the Event Operations Manager is formally modelled as a system role at all — with a defined visibility scope — is an unresolved gap between the brief's role table and the discussions (`#73`, `#93`, `#94`; see [[user-roles]] "Gap: The Event Operations Manager"). This card assumes the role exists because the discussions require *someone* to perform assignment; flag the modelling question rather than treat it as settled.
>
> **Recommend a companion card — Reassign an Event Coordinator:** As an Event Operations Manager, I want to reassign an event to a different Event Coordinator, so that coverage can change after initial assignment. AC: can reassign an already-assigned event to a different coordinator; the system may assume any reassignment entered has already been approved outside the system, with no in-system approval step (`#94`).
>
> **Changed vs. the original:** "eligible for assignment" → "all Event Coordinators" (the eligibility filter contradicted `#93`'s "no criteria need to be encoded"); removed the assignment-"failure" AC entirely (no failure/decline path exists per `#95`); split reassignment out as its own card instead of leaving it uncovered.

---

### EV2 — Submission of event requests

**Verdict: NEEDS WORK** — Gate A: fail (A5) · Gate B: fail (B8) · Gate D: fail (D5)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| A5 | "so I can successfully submit an event request" | Restates the WHAT (submitting) rather than giving a benefit from it. | e.g. "so that ConnectSphere can begin reviewing my event without me re-entering information later." |
| B8 (gap) | *(no AC covers this)* | The customer's actual Step 1 explicitly allows saving a request **before** submission so incomplete information can be finished later (brief s2/s4; [[event-request-workflow]] Step 1). Nothing in this card's ACs mentions draft-save, even though the story text ("indicate the initial event requirements") reads as if it should. | Add a draft-save AC, or split per §7.2 Workflow-steps into "Save a draft event request" and "Submit an event request." |
| D5 (gap) | "they should be informed of exactly what is missing or invalid" | This presupposes a fixed, known mandatory-field set. The customer has explicitly **not** firmed this up — "which fields are mandatory... has not been firmed up... please propose an appropriate set" (`#72`, see [[event-request]]). Validating against an unspecified rule set is not yet testable (T1/B10 depend on it). | Keep the AC's *shape*, but add an explicit open question: "mandatory field set for submission is undecided by the customer (`#72`) — team must propose and confirm before this AC is testable." |

> [!success] Proposed rewrite
> **Title:** Submission of event requests · **Priority:** *(set by team)* · **Estimate:** *(size in planning)*
>
> **Story:** As an Event Organiser, I want to indicate my event requirements and submit a request, so that ConnectSphere can begin planning without me having to re-enter information later.
>
> **AC:**
> - Can save a draft event request before submission, and return to complete or edit it later (brief step 1, see [[event-request-workflow]] Step 1)
> - Can submit the request once ready
> - Given complete and valid information, when the Event Organiser submits, then they are told the event has been successfully requested
> - Given invalid or incomplete information, when the Event Organiser submits, then they are told exactly what is missing or invalid
>
> **Open question:** the mandatory field set for submission is undecided by the customer — "has not been firmed up... please propose an appropriate set" (`#72`, see [[event-request]]). The validation AC above can't be made testable until the team proposes and confirms this set; don't invent one in the meantime.
>
> **Changed vs. the original:** fixed the WHY (was restating the action); added the draft-save AC the brief specifically calls out (brief step 1) and the original omitted entirely; surfaced the `#72` open question explicitly instead of silently assuming a field set exists.

---

### EV3 — Review and clarification of event requirements

**Verdict: REJECT** — Gate B: fail (B1, B8) · Gate A: fail (A6-adjacent)

| Check | Quote | Why it fails | Fix |
|---|---|---|---|
| B1 | Only 2 ACs total | "3–7 criteria present... 0–2 usually means the conversation has not happened" (§13, B1). This card has exactly 2. | Needs at least one more substantive AC, ideally covering the gap below. |
| B8 / title-content mismatch | Title: "Review **and clarification** of event requirements"; ACs: "Receive a notification... Be able to see the details and requirements" | The title promises two capabilities (review, and requesting clarification) — a live "and" per §7.2's splitting signal — but the ACs deliver only the first. Nothing addresses requesting clarification/amendment from the Event Organiser, or the approve/reject/return decision, both of which are real, well-documented customer capabilities for this exact step ([[event-request-workflow]] Steps 4–5: "requests clarification or amendment where incomplete or unclear"; decision retained per brief §5 step 5). | Split via §7.2 Workflow-steps: "Review an assigned event request" (as currently written, tightened) + "Request clarification on an event request" + "Approve, reject, or return an event request" (each independently testable, each with its own ACs pulled from [[event-request-workflow]] and [[event-request]]). |

> [!success] Proposed rewrite — split into three cards
> **EV3a — Review an assigned event request**
> As the assigned Event Coordinator, I want to see the details of events assigned to me, so I can judge whether enough information has been provided to proceed.
> - Receive a notification when an event request is assigned to me (`#73`, [[notifications]])
> - Can see the full details and requirements submitted for the event
>
> **EV3b — Request clarification on an event request**
> As the assigned Event Coordinator, I want to request clarification or amendment from the Event Organiser, so that incomplete or ambiguous requirements are resolved before I decide on the request.
> - Can send a clarification/amendment request to the Event Organiser against a specific request ([[event-request-workflow]] Steps 4–5)
> - The Event Organiser is notified of the request for clarification ([[notifications]])
> - The clarification request and the Organiser's response are retained with the event record (extends the same retain-with-record pattern `#87` establishes for technical clarifications)
> - Given the Organiser resubmits after amendment, then the coordinator's review resumes
>
> **EV3c — Approve, reject, or return an event request**
> As the assigned Event Coordinator, I want to approve, reject, or return an event request, so that only requests with sufficient information proceed to planning.
> - Can approve a request, which allows planning to begin but commits ConnectSphere to nothing yet (`#80`, see [[event]] States)
> - Can reject a request, retaining a decision record (see [[event-request]] States)
> - Can return a request to the Organiser for amendment
> - The Event Organiser is notified of the decision ([[notifications]])
> - **Open question:** whether a rejected request can be resubmitted, or rejection is terminal, is left to the team — "propose an appropriate approach if needed" (`#67`) — decide and record before treating this AC set as complete.
>
> Three independently testable, independently valuable cards instead of one card whose title over-promises and whose ACs under-deliver.

---

## 7. Recommended remediation order

1. **Fix the one remaining structural bug** — rewrite V6 ("Configure Venue Turnaround and Setup Buffer") per the draft in §4; it still duplicates V5's body.
2. **Fix the one hard-rule contradiction** — V2 (Cancel Venue Booking) currently authorises the wrong actor to cancel, in direct conflict with `#68`. This is the kind of defect the standard treats as an automatic Gate-D block, not a style note.
3. **Add the three missing Registration cards** (Reg4–Reg6 in §5) — waiting-list promotion, withdrawal, and check-in are all customer-confirmed capabilities with no card anywhere in the backlog; the waiting-list window is one of the project's eight non-negotiable hard rules.
4. **Strip invented, uncited rules** (X4: session timeout, password masking, "eligible" coordinators, assignment "failure") back to either a citation, a Definition-of-Done entry, or an explicit open question — per §5.5/5.6's own worked method.
5. **Pull the repeated audit/change-history ACs (Reg2, Reg3) out to the Definition of Done** — same §5.5 pattern, just for the auditability NFR instead of TLS/performance.
6. **Add Priority, Estimate, and Epic to every card** (X1) — none currently satisfy the Definition of Ready (§9) on this basis alone, independent of any other defect.
7. **Re-run Gate D** once citations are added, since several of the "NEEDS WORK" verdicts above may tighten to "READY" or loosen to "REJECT" depending on what the team decides for the open questions raised throughout.

See [[open-questions]] for the consolidated register of everything the customer has left to the team — most of the open questions raised in this review are already tracked there and should not be re-litigated from scratch.
