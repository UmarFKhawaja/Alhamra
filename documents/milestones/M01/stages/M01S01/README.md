# M01S01 — Terminology and Module Boundaries

## Status

Accepted

## Milestone

[M01 — Foundation](../../README.md)

## Purpose

Lock down the language, entity names, and first implementation boundary before schema and UI work starts.

## Scope

- Define the approved glossary for this area of the application
- Confirm that the application uses `customer` everywhere for the person receiving treatments
- Define which concepts belong in this module
- Define which concepts are explicitly not being built yet
- Identify dormant placeholder operational schema that should be removed before new domain work starts
- Create or update the repository-wide glossary and any local stage documentation needed by the implementation agent

## Deliverables

- Repository-wide glossary at [`documents/glossary/README.md`](../../../../glossary/README.md)
- Approved `M01` entity list
- Light definitions for later-stage entities to prevent naming conflicts
- Approved module boundary list
- Approved out-of-scope list
- Notes on naming conventions for database tables, TypeScript types, routes, UI labels, and persisted enum values
- Compact terminology drift register for follow-on stages
- Cleanup decision for dormant placeholder operational entities, including removal where safe and explicit retention where not yet safe

## Suggested data model

```txt
No new domain schema is required in this stage.

Dormant placeholder schema may be removed in this stage where it is not part of the validated current product surface.

Candidate entity names:
user
customer
practitioner
practice
clinic
room
treatment
appointment
appointmentEvent
consultation
consentTemplate
consentRecord
vendor
product
productPurchase
productPurchaseItem
productReceipt
productUsage
productAdjustment
treatmentRecord
treatmentRecordAmendment
media
mediaLink
mediaConsent
mediaAccessLog
aftercareTemplate
aftercareRecord
followUp
customerConcern
auditEvent
```

## Suggested application surfaces

- Documentation only
- No required user-facing UI change
- Canonical glossary document at [`documents/glossary/README.md`](../../../../glossary/README.md)

## Acceptance criteria

- The canonical glossary exists at [`documents/glossary/README.md`](../../../../glossary/README.md)
- The forbidden terminology has been removed from the planned domain model and planned UI copy
- Approved terms and legacy mappings exist for current naming conflicts
- Dormant operational placeholder entities have been removed, or explicitly marked for temporary retention with a reason
- The implementation agent has a clear list of approved entity names
- The implementation agent has a clear list of excluded functionality
- Future stages can reference this glossary instead of redefining terminology
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- UI screens
- Booking logic
- Product tracking logic
- Asset storage
- New feature schema additions unrelated to cleanup

## Implementation notes

- Keep this document small and strict
- Any later terminology change should be recorded as a deviation here and in the affected stage
- Do not introduce parallel names for the same concept
- The repository-wide domain glossary lives at [`documents/glossary/README.md`](../../../../glossary/README.md)
- `M01` terms are fully approved in the glossary; later-milestone terms are defined only enough to prevent naming conflicts
- `patient`, `appointment`, `product`, and `treatmentSession` placeholder schema have been removed from the active foundation
- Address storage and customer-role validation now live directly on `user`, removing the last active dependency on `patient`
- `location` is intentionally omitted as a first-class entity for now; use `practice` for the business and `clinic` for the physical treatment site
- Future stages should only add operational schema when the milestone scope has been validated, rather than reviving dormant placeholders
- This stage should not add new domain features or redesign routes while performing cleanup
- Milestone and stage planning documents have been reviewed, and the existing validated product surface still behaves correctly after the refactor and cleanup work

## Review notes

- Review the glossary with the milestone owner or project decision-maker before changing this stage from `Review` to `Accepted`
- Record any approved naming deviations in this document and in the affected later stage

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|
| 2026-06-20 | Replaced `location` with `practice` plus `clinic` in the approved glossary and `M01` planning terms. | `location` was too generic for the domain; `practice` and `clinic` are clearer and allow `location` to be omitted for now. | Project direction |

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-20 | Linked the stage to the repository-wide glossary and moved the document to `Review`. | Make the terminology pass concrete before marking the stage accepted. |
| 2026-06-20 | Replaced `location` with `practice` plus `clinic` in the candidate model and implementation notes. | Align the stage with clearer domain language. |
| 2026-06-20 | Added dormant placeholder operational schema cleanup to the stage scope and acceptance criteria. | The next domain phase should not inherit unused placeholder entities by default. |
| 2026-06-20 | Removed `patient` and other dormant operational placeholder schema from the active foundation and migrated address storage onto `user`. | Simplify the base schema before future milestone work expands it. |
| 2026-06-20 | Renamed the reserved later-stage catalog term from `vendorProduct` to `product`. | The vendor relationship is sufficient without baking vendor specificity into the primary product entity name. |
| 2026-06-21 | Renamed the reserved customer-care `asset*` terms to `media*`. | Keep generic `asset` available for site/content usage and use `media` for private customer-care photo/video records. |
| 2026-06-21 | Moved `M01S01` from `Review` to `Accepted`. | All milestone and stage documents were reviewed, and the existing validated product behavior was verified after the refactor. |
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
