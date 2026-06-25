# M02S02 — Availability and Public Booking

## Status

Proposed

## Milestone

[M02 — Booking, Consultation, and Consent](../../README.md)

## Purpose

Allow customers to book or request appointments while preventing double-booking of practitioner time.

## Scope

- Add practitioner availability
- Add blocked time
- Generate available slots
- Add public booking flow
- Support cancellation and rescheduling rules
- Protect booking writes with transaction-safe logic

## Deliverables

- Drizzle schema and migration for availability and blocked time
- Availability calculation service
- Public booking route/page
- Booking request/create action
- Admin confirmation flow if required
- Tests for conflict prevention and slot calculation

## Suggested data model

```txt
practitionerAvailability
  id
  practitionerID
  locationID?
  dayOfWeek
  startsAtTime
  endsAtTime
  active
  createdAt
  updatedAt

blockedTime
  id
  practitionerID
  locationID?
  startsAt
  endsAt
  reason?
  createdByUserID
  createdAt
  updatedAt

bookingRequest
  id
  customerID?
  treatmentID?
  practitionerID?
  requestedStartsAt
  requestedEndsAt
  status
  notes?
  createdAt
  updatedAt
```

## Suggested application surfaces

- Practitioner availability editor
- Blocked time editor
- Public booking treatment selection
- Public slot selection
- Booking confirmation page
- Admin pending booking review if confirmation is not automatic

## Acceptance criteria

- Available slots are derived from practitioner availability, existing appointments, and blocked time
- A customer can request or book an appointment
- The system prevents two bookings for the same practitioner at the same time
- Canceled slots can become available again when appropriate
- Rescheduling records an appointment event
- Booking works for a single-location setup without extra configuration burden
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Payment deposits
- Reminder delivery
- Consultation records
- Treatment records
- Complex room/resource scheduling unless already needed

## Implementation notes

- Use database constraints and transactions where possible
- Avoid check-then-insert booking logic outside a transaction
- Keep the public flow minimal until operational rules are proven

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
