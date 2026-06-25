# M04 — Treatment Evidence and Follow-Up

## Status

Proposed

## Purpose

Add treatment documentation media and post-treatment workflows.

This milestone ensures that the application can document work performed with photo and video media, issue aftercare, track follow-ups, and record customer concerns.

## Scope

This milestone covers:

- Photo and video media metadata
- Private object storage integration points
- Media links to customer records, appointments, consultations, treatment records, and concerns
- Before, during, after, follow-up, and concern capture stages
- Aftercare templates and aftercare records
- Follow-up records
- Customer concern records

## Stages

| Stage                               | Title | Purpose |
|-------------------------------------|---|---|
| [M04S01](./stages/M04S01/README.md) | Photo and Video Media | Attach private documentation media to records |
| [M04S02](./stages/M04S02/README.md) | Aftercare, Follow-Up, and Concerns | Track post-treatment instructions, reviews, and concerns |

## Completion criteria

This milestone is complete when:

- Photo and video files are stored outside MySQL.
- MySQL stores media metadata and links.
- Media can be linked to treatment records and other relevant records.
- Before and after media can be grouped through record links.
- Aftercare can be issued and acknowledged.
- Follow-ups can be scheduled and tracked.
- Customer concerns can be recorded, linked to treatment records, and resolved.
- Implemented work complies with accepted ADRs, or records any approved deviation.

## Out of scope

- Public media galleries
- Automated image editing
- Marketing automation
- Video transcoding beyond basic storage metadata
- Advanced customer messaging

## Deviations

Record any changes from this milestone plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the milestone completion criteria. | Make existing architecture decisions an explicit completion gate for media and follow-up work. |
| 2026-06-21 | Renamed the customer-care `asset*` planning terms to `media*` across M04. | Keep generic `asset` available for site/content usage and use `media` for private customer-care photo/video records. |
