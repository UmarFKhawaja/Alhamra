# M01 — Foundation

## Status

Accepted

## Purpose

Establish the terminology, people model, operational structure, and treatment catalog needed before building booking, records, product purchase tracking, media, and aftercare workflows.

This milestone prevents the rest of the system from being built on vague or inconsistent concepts.

## Scope

This milestone covers:

- Domain terminology and module boundaries
- Cleanup of dormant placeholder operational schema before new domain work expands
- Core `user`, `customer`, and `practitioner` concepts
- Practice, clinic, and room/resource structure
- Treatment catalog records

## Stages

| Stage                               | Title | Purpose |
|-------------------------------------|---|---|
| [M01S01](./stages/M01S01/README.md) | Terminology and Module Boundaries | Lock down naming, scope, and exclusions |
| [M01S02](./stages/M01S02/README.md) | Core Customer and Practitioner Model | Create the people and role foundations |
| [M01S03](./stages/M01S03/README.md) | Practice and Clinic Structure | Add operational context |
| [M01S04](./stages/M01S04/README.md) | Treatment Catalog | Define treatments available in the system |

## Completion criteria

This milestone is complete when:

- The glossary is approved.
- Dormant placeholder operational schema has been removed or explicitly retained with a recorded reason.
- The application uses `customer` consistently.
- Users can be represented as customers and/or practitioners.
- The system has a minimal practice/clinic structure.
- Treatments can be created, edited, activated, and deactivated.
- Implemented work complies with accepted ADRs, or records any approved deviation.
- All completed stages have been reviewed and marked as accepted.

## Out of scope

- Public appointment booking
- Consultation records
- Consent records
- Treatment records
- Product purchase tracking
- Product usage tracking
- Photo and video assets
- Aftercare and follow-up workflows
- Audit and reporting hardening

## Deviations

Record any changes from this milestone plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-20 | Updated `M01` terminology from `clinic/location` to `practice/clinic`. | `location` was too generic for the approved domain language. |
| 2026-06-20 | Added dormant placeholder schema cleanup to the foundation milestone scope. | The next domain phase should start from the validated product surface instead of carrying unused operational tables forward. |
| 2026-06-20 | Removed `patient` and other dormant operational placeholder schema from the active foundation. | Keep the foundation aligned with the validated product surface before future milestones add new domain models. |
| 2026-06-20 | Renamed the planned M03 catalog entity from `vendorProduct` to `product` in milestone terminology. | Keep the core catalog term simple while preserving vendor specificity through relationships. |
| 2026-06-21 | Moved `M01` to `In Progress` after accepting `M01S01`. | The foundation milestone is now underway with the terminology and cleanup stage reviewed and verified. |
| 2026-06-21 | Accepted `M01S02` after reviewing the implemented people model, unified user management surface, and updated documentation. | The foundation milestone can now proceed to `M01S03` with the customer and practitioner model locked in. |
| 2026-06-22 | Added ADR compliance to the milestone completion criteria. | Make existing architecture decisions an explicit completion gate for foundation work. |
| 2026-06-22 | Accepted `M01S03` after reviewing the implemented practice, clinic, and room management surface and the updated stage notes. | The foundation milestone can now proceed to `M01S04` once this branch is merged. |
| 2026-06-25 | Accepted `M01S04` after reviewing the implemented treatment catalog and updated stage notes. | The foundation milestone now has all planned stages reviewed and accepted. |
| 2026-06-25 | Accepted `M01` after reviewing the completed treatment catalog stage and confirming all foundation stages are now accepted. | The foundation milestone is complete and ready to be treated as the locked baseline for later milestones. |
