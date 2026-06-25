# M05S01 — Audit, Reporting, and Hardening

## Status

Proposed

## Milestone

[M05 — Operational Hardening](../../README.md)

## Purpose

Add accountability, reporting, export foundations, and data quality checks across the completed workflow.

## Scope

- Add audit event records
- Add access logging for sensitive records where not already covered
- Add amendment patterns for important records
- Add operational reports
- Add customer record export foundation
- Add basic retention policy documentation and hooks

## Deliverables

- Drizzle schema and migration for audit and export records if needed
- Audit logging service/helper
- Operational reporting pages
- Customer timeline review page
- Customer record export action or documented foundation
- Data quality checks
- Tests for audit logging and key reports

## Suggested data model

```txt
auditEvent
  id
  actorUserID?
  entityType
  entityID
  action
  occurredAt
  reason?
  metadata?

exportRequest
  id
  requestedByUserID
  customerID
  status
  requestedAt
  completedAt?
  storageKey?
  notes?

retentionPolicy
  id
  entityType
  retentionPeriod
  action
  active
  createdAt
  updatedAt
```

## Suggested application surfaces

- Audit event viewer
- Customer timeline review
- Operational reports
- Export request screen/action
- Data quality dashboard or admin list

## Acceptance criteria

- Important create/update/status/access/export/correction actions are logged
- Sensitive asset access can be reviewed
- Reports exist for attended appointments and missed appointments
- Reports exist for product purchases, usage, unused quantity, expired quantity, and returned quantity
- Reports exist for treatment records and follow-ups
- A customer record export foundation exists
- Data quality issues are visible to admins or maintainers
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Enterprise data warehouse
- External BI tool integration
- Automated regulatory filing
- Full document management system
- Complex retention automation

## Implementation notes

- This stage is not cosmetic polish
- The system contains sensitive operational records and needs accountability
- Prefer reusable audit helpers over one-off logging code
- Do not silently overwrite important records without an amendment trail

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
