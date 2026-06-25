# M02S01 — Appointment Core

## Status

Proposed

## Milestone

[M02 — Booking, Consultation, and Consent](../../README.md)

## Purpose

Create appointment records and an event-backed appointment status history before public booking is implemented.

## Scope

- Add appointment records
- Add appointment event records
- Allow manual appointment creation
- Allow status changes to be recorded as events
- Provide admin calendar/list/detail views

## Deliverables

- Drizzle schema and migration for appointments and appointment events
- Appointment validation schemas
- Manual appointment create/edit/status actions
- Admin appointment list/calendar view
- Appointment detail view with event history
- Tests for status event recording

## Suggested data model

```txt
appointment
  id
  customerID
  practitionerID
  treatmentID?
  locationID?
  roomID?
  startsAt
  endsAt
  status
  notes?
  createdAt
  updatedAt

appointmentEvent
  id
  appointmentID
  eventType
  createdByUserID
  createdAt
  notes?

Suggested statuses/events:
  requested
  confirmed
  rescheduled
  canceledByCustomer
  canceledByClinic
  checkedIn
  attended
  missed
  completed
```

## Suggested application surfaces

- Admin appointment list
- Admin calendar view
- Appointment detail page
- Manual create appointment form
- Status change controls
- Event history panel

## Acceptance criteria

- An appointment can be created manually
- Appointment status changes create appointment event records
- Appointments can be marked attended
- Appointments can be marked missed
- Appointment history is visible in the UI
- Appointment state is not tracked only by overwriting a single status field
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Public booking
- Availability calculation
- Consultation records
- Treatment records
- Reminder delivery

## Implementation notes

- This stage should focus on correctness of the appointment model
- Do not introduce public booking complexity yet
- Keep the event table append-friendly

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
