# M02S04 — Consent Records

## Status

Proposed

## Milestone

[M02 — Booking, Consultation, and Consent](../../README.md)

## Purpose

Add versioned consent templates and signed consent records that preserve the exact wording and data snapshot used at signing time.

## Scope

- Add consent templates
- Add consent records
- Support template versioning
- Support signatures or signed acknowledgement
- Link consent to customer, appointment, consultation, and later treatment records

## Deliverables

- Drizzle schema and migration for consent templates and records
- Admin consent template management
- Consent signing flow
- Signed consent detail view
- Tests for versioning and snapshot preservation

## Suggested data model

```txt
consentTemplate
  id
  treatmentID?
  version
  title
  body
  active
  activeFrom?
  retiredAt?
  createdAt
  updatedAt

consentRecord
  id
  customerID
  appointmentID?
  consultationID?
  treatmentRecordID?
  consentTemplateID
  templateVersion
  signedByUserID
  signedAt
  snapshot
  withdrawnAt?
  createdAt
  updatedAt
```

## Suggested application surfaces

- Admin consent template list/detail/create/edit
- Consent signing page
- Signed consent record page
- Customer timeline consent entries

## Acceptance criteria

- A consent template can be created and retired
- A signed consent record stores the template version
- A signed consent record stores a snapshot of the wording and submitted data
- Changing a consent template does not alter historic signed records
- Consent records can be linked to consultations and later treatment records
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Advanced digital signature provider integration
- Marketing campaign consent automation
- Treatment record creation
- Asset upload

## Implementation notes

- Use JSON snapshots for signed form data and wording where appropriate
- Do not rely on live template joins to reconstruct historic consent
- Consent records should be append-friendly and reviewable

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
