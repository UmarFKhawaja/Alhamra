# M01S04 — Treatment Catalog

## Status

Accepted

## Milestone

[M01 — Foundation](../../README.md)

## Purpose

Define the treatments that can be presented, booked, consulted on, and recorded.

## Scope

- Add treatment categories
- Add treatment records
- Support active/inactive state
- Support duration and requirement flags
- Provide admin management screens

## Deliverables

- Drizzle schema and migration for treatment catalog
- Validation schemas
- Admin treatment list/detail/create/edit screens
- Seed data for representative treatments
- Tests for treatment management

## Suggested data model

```txt
treatmentCategory
  id
  name
  slug
  description?
  sortOrder
  active
  createdAt
  updatedAt

treatment
  id
  treatmentCategoryID?
  name
  slug
  type
  description?
  durationMinutes
  requiresConsultation
  requiresPrescription
  active
  createdAt
  updatedAt

Allowed treatment type example:
  procedure
```

## Suggested application surfaces

- Admin treatment category screens
- Admin treatment screens
- Public treatment listing/detail integration if public catalog pages already exist

## Acceptance criteria

- Treatments can be created, edited, activated, and deactivated
- Inactive treatments are not offered for new bookings
- Treatment duration is available for booking stage calculations
- Treatment records can later link to this catalog
- The enum uses `procedure` where this kind of treatment type is required
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Pricing engine
- Discounts and promotions
- Product purchasing
- Appointment booking
- Treatment records

## Implementation notes

- Avoid mixing treatment catalog data with treatment record data
- The catalog describes what can be offered
- Treatment records later describe what was actually performed

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|
| 2026-06-25 | No automated tests were added in this stage. | Delivery was completed without introducing a test harness, by explicit user request. | User request |

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
| 2026-06-24 | Implemented treatment category and treatment entities, queries, mutations, server handler, widget, and routes. | Codebase delivery based on approved stage plan. |
| 2026-06-25 | Fixed treatment management data preservation issues, added representative seed data, and adopted a zero-TypeScript-diagnostics ADR. | Close review findings and make type safety an explicit quality bar. |
| 2026-06-25 | Moved `M01S04` from `In Progress` to `Accepted`. | Treatment catalog implementation and stage documentation were reviewed together and confirmed complete. |
