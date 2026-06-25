# M05 — Operational Hardening

## Status

Proposed

## Purpose

Harden the system so it is reviewable, accountable, reportable, and safer to operate.

This milestone should not be treated as optional polish. Once customer records, treatment records, product usage, consent records, and assets exist, the system needs auditability and operational reporting.

## Scope

This milestone covers:

- Audit events
- Record access logging
- Record amendments
- Reporting views
- Export support
- Retention policy foundations
- Data quality checks
- Operational review screens

## Stages

| Stage                               | Title | Purpose |
|-------------------------------------|---|---|
| [M05S01](./stages/M05S01/README.md) | Audit, Reporting, and Hardening | Add accountability, reporting, and operational safeguards |

## Completion criteria

This milestone is complete when:

- Important create, update, status change, access, export, and correction actions are logged.
- Sensitive record and asset access can be reviewed.
- Important record corrections are represented as amendments rather than silent overwrites.
- Operational reports exist for appointments, missed appointments, product purchases, product usage, unused products, expired products, treatment records, follow-ups, and assets.
- A basic export path exists for a customer's record.
- Known data quality checks are visible to admins or maintainers.
- Implemented work complies with accepted ADRs, or records any approved deviation.

## Out of scope

- Enterprise data warehouse
- Advanced BI dashboards
- External compliance filing
- Automated regulatory reporting
- Full document management system

## Deviations

Record any changes from this milestone plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the milestone completion criteria. | Make existing architecture decisions an explicit completion gate for operational hardening work. |
