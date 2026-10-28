# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` is superseded by PRD N-17 and is history only; nothing in this intent relies on it.

## Problem

- P-1: The fictional clinic receives avoidable calls from patients checking whether their results are ready.
- P-2: "Ready" is ambiguous. A result is not ready for the patient merely because the laboratory upload finished; it is ready only once a doctor has reviewed and released it.

## Proposed outcome

- I-1: A patient is notified when a result is ready for their review in the existing authenticated portal.
- I-2: A notification is eligible only after a doctor has reviewed and released the result. Completion of the laboratory upload alone never makes a result eligible.
- I-3: The notification says only that an update is available and directs the patient to the existing authenticated portal.
- I-4: The same released result version never produces duplicate notifications, and delivery failures are handled without sending duplicate messages to the patient.

How success will be observed:

- S-1: Fewer status-only calls (patients asking whether results are ready), counted separately from clinical questions and compared against a measured baseline.
- S-2: Delivery failures are tracked as a measure.
- S-3: No baseline or numerical target is approved. Both are to be agreed during discovery, so this intent sets none.
- S-4: Observable conditions: no notification exists for a result without a recorded doctor review and release; no message or notification log contains clinical detail; no released result version has more than one notification.

## Affected users and systems

- Patients: receive the notification and read their result in the portal.
- Reviewing doctors: their review and release is the gate for every notification.
- Support staff and the support lead: they take the status calls and count them.
- Product, privacy, clinical and engineering owners: they hold the decisions in the open questions and constraints below.
- Systems: the existing authenticated patient portal and its result route; the notification delivery path (channel undecided, see OPEN-1); the laboratory upload, which supplies results but is not a trigger; notification and operational logs.

## Out of scope

- X-1: Portal redesign, and a new patient dashboard (a separate project per the engineering lead).
- X-2: Appointment scheduling.
- X-3: Clinical interpretation of results.
- X-4: Notifying third parties. Referring-doctor notification is not approved and is tracked only as OPEN-2.
- X-5: Sending notifications on laboratory upload, or sending every result automatically before review.

## Constraints

- C-1: A doctor must review and release the result before any notification is eligible.
- C-2: No diagnosis, test result, clinical measurement or treatment detail in the message or in notification and operational logs. The message holds only the update-available statement and the pointer to the portal.
- C-3: Access is authenticated in the existing portal. No results are attached to the message and no public result link is introduced.
- C-4: Use the existing portal result route; it is adequate per the engineering lead.
- C-5: No duplicate notifications for the same released result version; delivery failures must not cause duplicate patient messages.
- C-6: The patient is the only approved recipient. No other recipient is approved or assumed.
- C-7: Consent and the actual channel are still under privacy review, so nothing here assumes a channel.

## Open questions and owners

- OPEN-1: Channel (SMS, email or portal push). Owners: product owner and privacy owner, after consent and delivery review. Undecided.
- OPEN-2: Notification to the referring doctor. Owners: clinical owner and privacy owner, who must decide recipient authorization. Not approved by PRD N-17, and approval is not inferred from the superseded request. It is decided separately and is not part of this scope.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
