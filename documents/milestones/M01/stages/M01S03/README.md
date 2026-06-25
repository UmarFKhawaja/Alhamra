# M01S03 — Practice and Clinic Structure

## Status

Accepted

## Milestone

[M01 — Foundation](../../README.md)

## Purpose

Add the operational context needed to know which practice and clinic appointments and treatments belong to.

## Scope

- Add practice records
- Add clinic records
- Add optional room/resource records
- Allow practitioners to be associated with practices
- Allow practitioners to be associated with clinics

## Deliverables

- Drizzle schema and migration for practice/clinic/room structures
- Basic admin screens
- Practitioner-practice assignment capability
- Practitioner-clinic assignment capability
- Seed data for one practice and one clinic
- Tests for create/update/read and practitioner assignment

## Suggested data model

```txt
practice
  id
  tradingName
  legalName?
  companyNumber?
  vatNumber?
  active
  createdAt
  updatedAt

clinic
  id
  practiceID
  name
  addressLine1?
  addressLine2?
  townOrCity?
  postcode?
  country?
  active
  createdAt
  updatedAt

room
  id
  clinicID
  name
  active
  createdAt
  updatedAt

practitionerClinic
  id
  practitionerID
  clinicID
  active
  createdAt
  updatedAt

practitionerPractice
  id
  practitionerID
  practiceID
  active
  createdAt
  updatedAt
```

## Suggested application surfaces

- Admin practice detail with company/legal fields
- Admin clinic list/detail
- Admin room/resource list/detail
- Practitioner-practice and practitioner-clinic assignment control

## Acceptance criteria

- A practice can have one or more clinics
- A clinic can have zero or more rooms/resources
- A practitioner can be assigned to a practice
- A practitioner can be assigned to a clinic
- Inactive clinics and rooms/resources are not offered for new booking choices
- The model supports a single-clinic business without excessive UI overhead
- Company/legal details can be managed without forcing clinics or practitioner assignment into the same screen
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Availability rules
- Appointment booking
- Room conflict prevention
- Multi-tenant franchise permissions

## Implementation notes

- Keep this lightweight
- Do not overbuild multi-branch logic until the booking workflow proves it is needed
- `location` is intentionally omitted as a first-class entity for now
- Practice management lives under `/manage/practice`, with company details at `/manage/practice/company`; site management remains reserved for public-facing content artifacts
- `practitionerPractice` captures practice-wide membership/access; `practitionerClinic` captures active clinic assignment within that practice
- The `Company` section on `/manage/practice/company` represents the legal and trading structure of the broader `practice` entity; additional practice-wide information may be added later without renaming the entity
- Clinic pages focus on clinic data and room navigation; practitioner access is managed from user-profile workflows instead of the clinic screen itself
- The rooms table now uses the same framed table treatment as other management tables such as Assets and Users
- Use this structure later for appointment and treatment clinic links

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|
| 2026-06-20 | Replaced the `clinic`/`location` split with `practice`/`clinic` terminology. | `location` was too generic for the approved domain language and is intentionally omitted for now. | Project direction |
| 2026-06-21 | Kept room management inside clinic detail instead of adding separate room list and detail routes. | Rooms are intentionally lightweight in this stage, and colocating them with clinic management avoids unnecessary admin overhead. | Project direction |
| 2026-06-21 | Treated `practice` as a singleton management surface instead of supporting a multi-practice admin directory. | The product is not intended to be multi-tenant, so a single practice detail page is a better fit than a list/create flow. | Project direction |
| 2026-06-21 | Focused the singleton `/manage/practice/company` page on company/legal details, while keeping clinic and practitioner relationships in adjacent practice-management surfaces. | Company information is only one aspect of the broader `practice` entity, and the main page should stay lightweight. | Project direction |
| 2026-06-22 | Moved room create/edit into dedicated room pages and removed clinic-page practitioner assignment controls. | Clinic screens should stay focused on clinic data, while practitioner access is better managed from user-profile workflows. | Project direction |

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-20 | Renamed the stage from `Clinic and Location Structure` to `Practice and Clinic Structure`. | Remove `location` as a first-class entity and clarify the business/site split. |
| 2026-06-20 | Reworked the planned data model from `clinic/location/room` to `practice/clinic/room`. | Match the approved glossary and omit `location` for now. |
| 2026-06-21 | Moved `M01S03` to `In Progress` and added `practitionerPractice` to the planned model. | Practice-wide practitioner membership/access is a separate concern from clinic assignment and is worth capturing in the foundation. |
| 2026-06-21 | Split practice management from site management and treated `practice` as a singleton admin surface. | Public-facing content belongs under `/manage/site`, while operational practice data belongs under `/manage/practice/company` and related practice routes. |
| 2026-06-21 | Reframed the main `practice` form as a `Company` section with trading/legal identifiers. | The legal structure belongs to `practice`, but it should not define the whole entity or crowd the page with clinic access controls. |
| 2026-06-21 | Moved the editable company screen from the `/manage/practice` index route to `/manage/practice/company`. | Keep mutable form screens on explicit routes and avoid React Router's `?index` submission URL behavior. |
| 2026-06-22 | Extracted the shared address section and reused it on clinic management, while moving room create/edit to dedicated room routes. | Reuse the existing address workflow and keep the clinic page focused on clinic-level data plus room navigation. |
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
| 2026-06-22 | Moved `M01S03` from `In Progress` to `Accepted`. | Code and documentation were reviewed together and confirmed ready to merge before starting `M01S04`. |
