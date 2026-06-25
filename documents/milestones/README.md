# Milestones

This folder contains the staged implementation plan for the customer, practitioner, appointment, treatment record, customer-specific product purchase, media, aftercare, and operational hardening areas of the application.

The milestone files describe larger delivery groupings. The files in `stages/MxxSyy` folder under each `milestones/Mxx` describe smaller reviewable stages.

## Rules for using these documents

1. Treat each stage as a review boundary.
2. Do not merge unrelated stages unless the deviation is recorded in the relevant stage document.
3. Keep terminology consistent with the domain glossary.
4. Use `customer` everywhere for the person receiving treatments.
5. Treat appointment records, consultation records, treatment records, product purchase records, product usage records, consent records, and media records as separate concepts.
6. Record meaningful deviations in the `Deviations` section of the affected document.
7. When a stage is complete, update its status and add implementation notes.
8. Update the `Change record` section at the bottom of the affected milestone or stage file when the document evolves.
9. Treat accepted ADRs as binding requirements unless a newer ADR supersedes them or an approved deviation is recorded in the relevant milestone or stage document.

## Milestones

| Milestone              | Title | Stages |
|------------------------|---|---|
| [M01](./M01/README.md) | Foundation | M01S01-M01S04 |
| [M02](./M02/README.md) | Booking, Consultation, and Consent | M02S01-M02S04 |
| [M03](./M03/README.md) | Customer-Specific Products and Treatment Records | M03S01-M03S04 |
| [M04](./M04/README.md) | Treatment Evidence and Follow-Up | M04S01-M04S02 |
| [M05](./M05/README.md) | Operational Hardening | M05S01 |

## Stage status values

Use these values consistently:

| Status | Meaning |
|---|---|
| Proposed | Planned but not started |
| In Progress | Implementation has started |
| Blocked | Cannot progress until a blocker is resolved |
| Review | Implementation is ready for review |
| Accepted | Reviewed and accepted |
| Superseded | Replaced by another plan or implementation |

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance as a standing requirement across milestone and stage planning documents. | Make architectural decisions reviewable as explicit delivery criteria instead of implied guidance. |
| 2026-06-20 | Added a required `Change record` footer convention for milestone and stage files. | Keep document evolution visible inside the planning docs themselves. |
