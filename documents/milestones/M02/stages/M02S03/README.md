# M02S03 — Consultation Records

## Status

Proposed

## Milestone

[M02 — Booking, Consultation, and Consent](../../README.md)

## Purpose

Capture pre-treatment assessment, suitability, recommendations, and practitioner notes as durable customer records.

## Scope

- Add consultation records
- Link consultations to customers, practitioners, appointments, and treatments
- Capture structured assessment fields and notes
- Allow consultation to exist without a treatment record
- Allow consultation to recommend treatment or product purchase

## Deliverables

- Drizzle schema and migration for consultations
- Consultation validation schemas
- Practitioner consultation form
- Customer timeline integration
- Tests for consultation creation and linking

## Suggested data model

```txt
consultation
  id
  customerID
  practitionerID
  appointmentID?
  treatmentID?
  reasonForVisit?
  areasOfConcern?
  desiredOutcome?
  relevantHistory?
  contraindications?
  suitabilityDecision
  recommendedPlan?
  productPurchaseRecommended
  notes?
  createdAt
  updatedAt
  completedAt?
```

## Suggested application surfaces

- Create consultation from appointment
- Create consultation from customer record
- Consultation detail page
- Consultation edit/amend page depending on record policy
- Customer timeline entry

## Acceptance criteria

- A consultation can be created from an appointment
- A consultation can exist without a treatment being performed
- A consultation can record that product purchase is recommended
- Consultation records appear on the customer timeline
- The consultation record is separate from the appointment record
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Consent signing
- Treatment records
- Product purchase implementation
- Photo and video uploads
- Advanced form builder

## Implementation notes

- Keep structured fields for things that need filtering or reporting
- Use free-text notes only where structure would be premature
- Do not overload appointment records with consultation details

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
| 2026-06-21 | Replaced `customer profile` wording with `customer record` in the suggested surfaces. | Keep later-stage planning aligned with the approved `customer` entity name. |
