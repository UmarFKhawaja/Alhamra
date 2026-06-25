# M03S04 — Treatment Records

## Status

Proposed

## Milestone

[M03 — Customer-Specific Products and Treatment Records](../../README.md)

## Purpose

Record treatments administered to customers and link them to appointments, consultations, practitioners, and product usage.

## Scope

- Add treatment records
- Link treatment records to customer, practitioner, treatment, appointment, and consultation
- Link product usage to treatment records
- Add amendment support for important corrections
- Show treatment records on the customer timeline

## Deliverables

- Drizzle schema and migration for treatment records and amendments
- Treatment record create/edit or amend flow
- Link product usage to treatment record
- Treatment record detail page
- Customer treatment timeline
- Tests for treatment creation and product usage linking

## Suggested data model

```txt
treatmentRecord
  id
  customerID
  appointmentID?
  consultationID?
  treatmentID
  practitionerID
  locationID?
  roomID?
  performedAt
  treatmentArea?
  notes?
  outcome?
  createdAt
  updatedAt
  completedAt?

treatmentRecordAmendment
  id
  treatmentRecordID
  amendedByUserID
  amendedAt
  reason
  beforeSnapshot?
  afterSnapshot?
  notes?
```

## Suggested application surfaces

- Create treatment record from appointment
- Create treatment record from consultation
- Treatment record detail page
- Product usage link/add control
- Customer timeline treatment entries
- Amendment history panel

## Acceptance criteria

- A treatment record can be created for a customer
- A treatment record can link to an appointment and consultation when available
- A treatment record identifies the practitioner who performed the treatment
- Product usage can be attached to the treatment record
- Treatment records appear on the customer timeline
- Important corrections are represented as amendments rather than silent overwrites
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Photo and video asset upload
- Aftercare
- Follow-up
- Complex treatment diagramming
- Advanced outcome scoring

## Implementation notes

- Keep appointment and treatment record responsibilities separate
- Use structured fields for reporting and notes for narrative detail
- Avoid making treatment records depend on a public booking flow

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
