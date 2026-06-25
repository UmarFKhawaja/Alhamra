# M03S01 — Vendor and Product Catalog

## Status

Proposed

## Milestone

[M03 — Customer-Specific Products and Treatment Records](../../README.md)

## Purpose

Add vendors and product references so product purchases can be recorded consistently.

## Scope

- Add vendor records
- Add product references
- Support active/inactive products
- Record whether a product requires prescription handling
- Provide admin search and selection support

## Deliverables

- Drizzle schema and migration for vendors and products
- Admin vendor management screens
- Admin product management screens
- Product selection component for later purchase flow
- Seed data for representative vendors and products
- Tests for basic vendor/product CRUD

## Suggested data model

```txt
vendor
  id
  name
  websiteURL?
  accountReference?
  notes?
  active
  createdAt
  updatedAt

product
  id
  vendorID
  name
  vendorSKU?
  manufacturer?
  category?
  requiresPrescription
  unit
  active
  createdAt
  updatedAt
```

## Suggested application surfaces

- Admin vendor list/detail/create/edit
- Admin product list/detail/create/edit
- Product search/select UI component

## Acceptance criteria

- A vendor can be created and edited
- A product can be created and linked to a vendor
- Inactive products are not offered for new product purchases
- Products can be marked as requiring prescription handling
- The model does not imply standalone product sales
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Customer-specific product purchase records
- Product receipt tracking
- Product usage tracking
- General shared stock management
- Vendor ordering API integration

## Implementation notes

- This is a reference catalog, not a shop catalog
- Vendors such as Wigmore should be represented as vendors
- `product` is the primary catalog term; vendor specificity belongs to the product relationship rather than the entity name
- Do not build checkout or sales basket behavior here

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
| 2026-06-20 | Renamed the planned catalog entity from `vendorProduct` to `product`. | Keep the primary product name concise while preserving vendor linkage on the model itself. |
