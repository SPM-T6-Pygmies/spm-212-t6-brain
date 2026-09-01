---
title: Event Request
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, event-request]
---

# Event Request

The pre-approval artefact an Event Organiser creates and submits to ConnectSphere. Approval converts a request into a plannable [[event]]; the two are modelled as **separate records** because approval commits ConnectSphere to nothing — no venue, equipment, technical support, registration setup, or delivery (#80). The request exists to be reviewed and, if accepted, to seed an Event; it is not itself a plannable object.

> [!warning] The brief blurs Event Request and Event
> Brief §4 lists *Event Request Creation* and *Event Status Management* as separate functionality areas, implying two distinct records. Yet the *Event*'s example status list in the same section includes `draft` and `submitted` — statuses that, on the pre/post-approval reading above, belong to the request, not the event. The brief never draws the request/event boundary explicitly; the split modelled on this page and on [[event]] is this wiki's resolution, not a customer-confirmed data model. This is the single largest open modelling decision in the event cluster.

## Attributes

Every field below is `⚠️ Partial`: the mandatory field set for submission has **not been firmed up**, and is explicitly left to the team to propose; there are also no field-length requirements (#72).

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Event name | Working title of the proposed event | ⚠️ Partial | brief §4; #72 |
| Description | Free-text description | ⚠️ Partial | brief §4; #72 |
| Purpose | Why the event is being held | ⚠️ Partial | brief §2; #72 |
| Preferred dates / time | Organiser's requested date and time | ⚠️ Partial | brief §2; #72 |
| Expected attendance | Organiser's attendance estimate | ⚠️ Partial | brief §2; #72 |
| Venue requirements | Capacity, layout, accessibility, facilities sought | ⚠️ Partial | brief §2; #72 |
| Accessibility needs | Organiser-stated accessibility requirements | ⚠️ Partial | brief §2; #72 |
| Equipment requirements | Organiser-stated equipment needs | ⚠️ Partial | brief §2; #72 |
| Registration requirements | Whether/how the event should support Attendee registration | ⚠️ Partial | brief §2; #72 |
| Room-layout preferences | Preferred layout(s) | ⚠️ Partial | brief §4; #72 |
| General programme | Draft agenda/programme information | ⚠️ Partial | brief §2; #72 |
| Other special arrangements | Anything else the Organiser flags | ⚠️ Partial | brief §2; #72 |
| Requesting Event Organiser | The [[user-account]] who authored the request | ⚠️ Partial | brief §2; #72 |
| Client organisation | The [[client-organisation]] the requester belongs to | ⚠️ Partial | brief §4; #72 |
| Status | Draft / submitted / under review / approved / rejected / returned — see States | ⚠️ Partial | brief §4; #72 |
| Decision record | Retained record of the coordinator's approve/reject/return decision | ⚠️ Partial | brief §5 (step 5); #72 |

## States

The request's own status set is narrower than the full Event lifecycle in [[event-lifecycle]] — it covers only the pre-approval stages. This narrowing is this wiki's proposed resolution of the ambiguity flagged above, not a customer-specified split.

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| Draft | Saved before submission; Organiser may keep editing | Entry: Organiser creates. Exit: submit | brief §4 |
| Submitted | Sent to ConnectSphere, awaiting coordinator assignment | Entry: Organiser submits. Exit: coordinator reviews | brief §5 |
| Under review | Assigned coordinator reviewing, may request clarification | Entry: review begins. Exit: approve / reject / return | brief §5 |
| Approved | Sufficient information for planning to proceed; the request becomes an [[event]] | Entry: coordinator approves. Terminal for this record — see [[event]] | #80 |
| Rejected | Declined at review; decision record retained | Entry: coordinator rejects. Terminal, unless resubmission is permitted (🔶 #67) | brief §5 |
| Returned | Sent back to the Organiser for amendment or clarification | Entry: coordinator requests amendment. Exit: Organiser resubmits | brief §4 |

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | 0..1 : 1 | Approval converts the request into a plannable Event; the request does not carry planning-stage data (venue, equipment, confirmation) itself | #80 |
| [[user-account]] | many : 1 | Created by the requesting Event Organiser | brief §2 |
| [[client-organisation]] | many : 1 | Requester's client organisation | brief §4 |
| [[audit-record]] | 1 : many | Decision record and activity history | brief §5, §8f |

## Binding rules

- The request may be created by an Event Organiser or an Event Coordinator (#78).
- No minimum lead time is required between request creation and the event date (#78).
- No contract or terms-acceptance step exists in the system — contracts and payments are handled physically, outside the system (#24).
- No field-length requirements are specified for any request field (#72).

## Open modelling questions

> [!question]- Team decision required — resubmission after rejection
> Whether a rejected request can be resubmitted, or whether rejection is final, was left to the team: "Please propose an appropriate approach if needed." (#67).

> [!question]- Team decision required — mandatory field set
> Which fields are mandatory for a request to be submittable has not been firmed up by the customer; the team must propose an appropriate set (#72).

See [[domain-model]] for the full object catalogue.
