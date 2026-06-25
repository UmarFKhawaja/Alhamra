# M02 — Booking, Consultation, and Consent

## Status

Proposed

## Purpose

Build the workflow that allows appointments to be created, booked, tracked, converted into consultations, and supported by versioned consent records.

This milestone moves the application from static records into real operational workflow.

## Scope

This milestone covers:

- Appointment records
- Appointment event history
- Practitioner availability
- Booking requests
- Public booking flow
- Consultation records
- Versioned consent templates and signed consent records

## Stages

| Stage                               | Title | Purpose |
|-------------------------------------|---|---|
| [M02S01](./stages/M02S01/README.md) | Appointment Core | Create appointments and event-backed status history |
| [M02S02](./stages/M02S02/README.md) | Availability and Public Booking | Allow safe booking and rescheduling |
| [M02S03](./stages/M02S03/README.md) | Consultation Records | Capture pre-treatment assessment and recommendations |
| [M02S04](./stages/M02S04/README.md) | Consent Records | Add versioned consent and signed snapshots |

## Completion criteria

This milestone is complete when:

- Appointments can be created and status changes are recorded as events.
- Practitioner availability can be configured.
- Booking prevents double-booking of practitioner time.
- Customers can request or book appointments.
- Consultations can be created from appointments.
- Consent templates are versioned.
- Signed consent records preserve the exact wording and data snapshot used at signing time.
- Implemented work complies with accepted ADRs, or records any approved deviation.

## Out of scope

- Customer-specific product purchase implementation
- Product receipt and usage ledger
- Treatment records
- Photo and video assets
- Aftercare and concern workflows
- Advanced reporting

## Deviations

Record any changes from this milestone plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the milestone completion criteria. | Make existing architecture decisions an explicit completion gate for booking, consultation, and consent work. |
