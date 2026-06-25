# M03S02 — Customer-Specific Product Purchases

## Status

Proposed

## Milestone

[M03 — Customer-Specific Products and Treatment Records](../../README.md)

## Purpose

Record products purchased from vendors for specific customers.

## Scope

- Add product purchase records
- Add product purchase item records
- Link each purchase to a customer
- Link purchase items to products
- Record vendor order and invoice references
- Attach invoice/order asset metadata later or through a placeholder link

## Deliverables

- Drizzle schema and migration for product purchases and product purchase items
- Customer-specific product purchase create/edit flow
- Product purchase detail page
- Customer product purchase list
- Tests for customer linkage and purchase item creation

## Suggested data model

```txt
productPurchase
  id
  customerID
  vendorID
  purchasedByUserID
  orderReference?
  invoiceNumber?
  invoiceAssetID?
  purchasedAt?
  status
  notes?
  createdAt
  updatedAt

productPurchaseItem
  id
  productPurchaseID
  customerID
  productID
  quantityPurchased
  unit
  costAmount?
  costCurrency?
  prescriptionRequired
  prescriptionReference?
  notes?
  createdAt
  updatedAt

Suggested purchase statuses:
  draft
  ordered
  partiallyReceived
  received
  canceled
  returned
```

## Suggested application surfaces

- Create product purchase from customer record
- Create product purchase from consultation recommendation
- Product purchase detail page
- Customer product purchase tab/list
- Product picker

## Acceptance criteria

- A product purchase is always linked to a specific customer
- A purchase can contain one or more purchase items
- Each purchase item is linked to a product
- The purchase appears on the customer record
- Purchase and usage are not collapsed into one concept
- The UI makes clear that products are bought for the customer
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Product receipt
- Product usage
- Treatment records
- Shared clinic stock
- Automated vendor ordering
- Accounting integration

## Implementation notes

- Duplicate `customerID` on purchase items intentionally if it simplifies queries and reinforces ownership
- Keep vendor specificity on the linked `product`, rather than in a `vendorProduct` entity name
- Reassignment of a purchase item to another customer should not exist in the normal workflow
- If reassignment is ever added, it must be exceptional and audited

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
| 2026-06-20 | Renamed the referenced catalog entity from `vendorProduct` to `product`, including `vendorProductID` to `productID`. | Align the purchase model with the simplified product terminology approved in the glossary. |
| 2026-06-21 | Replaced `customer profile` wording with `customer record` in the suggested surfaces. | Keep later-stage planning aligned with the approved `customer` entity name. |
