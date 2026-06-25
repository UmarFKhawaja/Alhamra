# M04S01 — Photo and Video Media

## Status

Proposed

## Milestone

[M04 — Treatment Evidence and Follow-Up](../../README.md)

## Purpose

Attach private photo and video documentation media to customer records, appointments, consultations, treatment records, and concerns.

## Scope

- Add media metadata records
- Add generic media links
- Integrate private object storage upload/download flow
- Support capture stage and body area metadata
- Add media access logging foundation

## Deliverables

- Drizzle schema and migration for media metadata, links, consent, and access logs
- Private upload flow
- Signed download/view flow
- Media gallery on customer/treatment screens
- Before/after grouping through metadata and links
- Tests for metadata creation and access restrictions

## Suggested data model

```txt
media
  id
  customerID
  uploadedByUserID
  mediaType
  storageProvider
  storageBucket
  storageKey
  mimeType
  byteSize
  checksum?
  createdAt
  updatedAt

mediaLink
  id
  mediaID
  linkedType
  linkedID
  captureStage
  bodyArea?
  createdAt

mediaConsent
  id
  mediaID
  consentRecordID?
  permittedUse
  expiresAt?
  withdrawnAt?
  createdAt
  updatedAt

mediaAccessLog
  id
  mediaID
  accessedByUserID
  accessType
  accessedAt
  reason?
```

## Suggested application surfaces

- Upload media from customer record
- Upload media from treatment record
- Treatment media gallery
- Customer media gallery
- Media detail page
- Media access log panel if simple to expose

## Acceptance criteria

- Photo and video files are not stored directly in MySQL
- MySQL stores media metadata and links
- Media is private by default
- Media can be linked to treatment records
- Media supports capture stages such as before, during, after, follow-up, and concern
- Media access can be logged
- Signed/private URLs are used for access
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Public galleries
- Marketing workflow automation
- Advanced image editing
- Video transcoding pipeline
- OCR or AI analysis

## Implementation notes

- Use object storage for file bytes
- Keep storage keys private
- Use metadata to group before and after media
- Do not mix documentation use and marketing use without explicit consent modelling

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
| 2026-06-21 | Renamed the customer-care `asset*` planning terms to `media*`. | Keep generic `asset` available for site/content usage and use `media` for private customer-care photo/video records. |
| 2026-06-21 | Replaced `customer profile` wording with `customer record` in the suggested surfaces. | Keep later-stage planning aligned with the approved `customer` entity name. |
