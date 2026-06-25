# M03 — Customer-Specific Products and Treatment Records

## Status

Proposed

## Purpose

Build the customer-specific product purchase and usage system, then connect product usage to treatment records.

This milestone is central to the application. Products are not primarily general retail items. The core requirement is to track products purchased from aesthetics vendors for specific customers and then track how those products are received, used, wasted, returned, expired, or left unused.

## Scope

This milestone covers:

- Vendor records
- Product catalog references
- Product purchases for specific customers
- Product purchase items
- Receipt tracking
- Usage ledger
- Adjustments for waste, expiry, return, or correction
- Treatment records
- Links between treatment records and product usage

## Stages

| Stage                               | Title | Purpose |
|-------------------------------------|---|---|
| [M03S01](./stages/M03S01/README.md) | Vendor and Product Catalog | Add vendors and reusable product references |
| [M03S02](./stages/M03S02/README.md) | Customer-Specific Product Purchases | Record products bought for specific customers |
| [M03S03](./stages/M03S03/README.md) | Product Receipt and Usage Ledger | Track received, used, wasted, returned, expired, and remaining quantities |
| [M03S04](./stages/M03S04/README.md) | Treatment Records | Record administered treatments and link product usage |

## Completion criteria

This milestone is complete when:

- Vendors can be maintained.
- Products can be selected during purchase entry.
- Product purchases are linked to a specific customer.
- Product purchase items can be received.
- Product usage is tracked against product purchase items.
- Remaining quantity can be calculated reliably.
- Usage cannot exceed available received quantity.
- Treatment records can link to appointments, consultations, practitioners, treatments, and product usage.
- The customer record shows product purchase and treatment history.
- Implemented work complies with accepted ADRs, or records any approved deviation.

## Out of scope

- General ecommerce checkout
- General standalone product sales
- Complex stock management for shared clinic stock
- Automated vendor ordering integrations
- Invoice OCR
- Accounting integrations

## Deviations

Record any changes from this milestone plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the milestone completion criteria. | Make existing architecture decisions an explicit completion gate for product and treatment-record work. |
| 2026-06-20 | Renamed the planned catalog entity from `vendorProduct` to `product` across M03. | Keep the primary catalog term simple and treat vendor specificity as a relationship. |
