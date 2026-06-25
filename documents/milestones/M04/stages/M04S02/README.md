# M04S02 — Aftercare, Follow-Up, and Concerns

## Status

Proposed

## Milestone

[M04 — Treatment Evidence and Follow-Up](../../README.md)

## Purpose

Track post-treatment instructions, acknowledgements, review appointments, customer concerns, and outcomes.

## Scope

- Add aftercare templates
- Add aftercare records
- Add follow-up records
- Add customer concern records
- Allow concerns to link to treatment records and media

## Deliverables

- Drizzle schema and migration for aftercare, follow-up, and concern records
- Aftercare template management
- Issue aftercare flow
- Follow-up scheduling and status flow
- Concern logging flow
- Customer timeline integration
- Tests for aftercare/follow-up/concern creation and linking

## Suggested data model

```txt
aftercareTemplate
  id
  treatmentID?
  version
  title
  body
  active
  createdAt
  updatedAt

aftercareRecord
  id
  customerID
  treatmentRecordID
  aftercareTemplateID?
  templateVersion?
  issuedByUserID
  issuedAt
  acknowledgedAt?
  snapshot?
  createdAt
  updatedAt

followUp
  id
  customerID
  treatmentRecordID?
  appointmentID?
  dueAt
  status
  reason?
  outcome?
  createdAt
  updatedAt

customerConcern
  id
  customerID
  treatmentRecordID?
  reportedByUserID?
  severity
  description
  status
  actionTaken?
  resolvedAt?
  createdAt
  updatedAt
```

## Suggested application surfaces

- Aftercare template admin
- Issue aftercare from treatment record
- Follow-up list/detail
- Customer concern create/detail
- Concern media attachment through media links
- Customer timeline entries

## Acceptance criteria

- Aftercare can be issued for a treatment record
- Issued aftercare preserves a snapshot where needed
- A customer can acknowledge aftercare if customer-facing UI exists at this stage
- Follow-ups can be scheduled and tracked
- Concerns can be logged against a customer and optionally a treatment record
- Concern records can link to uploaded media
- Resolved concerns retain their history
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Customer messaging
- Automated reminder sending unless already supported
- Emergency triage automation
- Advanced outcome scoring

## Implementation notes

- A concern is a record, not just a note
- Follow-up may link to an appointment if a review visit is booked
- Aftercare content should be versioned like consent where wording matters

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
