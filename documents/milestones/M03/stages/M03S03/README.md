# M03S03 — Product Receipt and Usage Ledger

## Status

Proposed

## Milestone

[M03 — Customer-Specific Products and Treatment Records](../../README.md)

## Purpose

Track the receipt, use, waste, return, expiry, correction, and remaining quantity of customer-specific product purchase items.

## Scope

- Add receipt records
- Add usage records
- Add adjustment records
- Calculate remaining quantity
- Prevent usage beyond available received quantity
- Keep usage tied to the original customer-specific purchase item

## Deliverables

- Drizzle schema and migration for product receipt, usage, and adjustment records
- Receipt recording flow
- Usage recording flow
- Adjustment flow for waste, expiry, return, and correction
- Remaining quantity calculation
- Tests for ledger calculations and overuse prevention

## Suggested data model

```txt
productReceipt
  id
  productPurchaseItemID
  receivedByUserID
  receivedAt
  quantityReceived
  unit
  batchNumber?
  expiryDate?
  notes?
  createdAt
  updatedAt

productUsage
  id
  customerID
  productPurchaseItemID
  treatmentRecordID?
  usedByPractitionerID
  usedAt
  quantityUsed
  unit
  bodyArea?
  notes?
  createdAt
  updatedAt

productAdjustment
  id
  productPurchaseItemID
  adjustmentType
  quantity
  unit
  reason
  createdByUserID
  createdAt

Suggested adjustment types:
  wasted
  expired
  returned
  corrected
```

## Suggested application surfaces

- Product purchase item receipt action
- Product usage action
- Product adjustment action
- Remaining quantity display
- Customer product ledger view

## Acceptance criteria

- A purchase item can be marked as received in full or in part
- A usage record reduces available quantity
- Waste, expiry, return, and correction can be recorded
- Remaining quantity is calculated from ledger records
- The system prevents product usage beyond available quantity
- Usage remains tied to the customer-specific purchase item
- A purchase item can be reported as unused, partially used, or fully used
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Treatment record UI beyond optional placeholder link
- Shared clinic stock
- Automatic expiry notifications unless trivial
- Accounting integration

## Implementation notes

- Treat this as a ledger rather than a mutable stock counter
- Use transactions around usage writes
- Do not allow negative remaining quantity
- Status can be derived from ledger state where possible

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
