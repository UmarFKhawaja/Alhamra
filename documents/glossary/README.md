# Domain Glossary

This document is the canonical repository-wide glossary for domain terminology used by the milestone plans and implementation work.

## Use

- Fully approve `M01` terms here before schema, route, and UI work expands.
- Define later-milestone terms only enough to prevent naming conflicts.
- Record terminology drift here before follow-on implementation stages rename or replace legacy code.

## Approved `M01` Terms

| Term | Definition | Primary stage | Notes |
|---|---|---|---|
| `user` | The base application account that can authenticate, hold contact details, and carry one or more roles. | `M01S02` | `user` is the account record, not a customer-only concept. |
| `customer` | The customer-specific record linked to a `user` for the person receiving treatments. | `M01S02` | `customer` is the approved business term. |
| `practitioner` | The practitioner-specific record linked to a `user` for someone delivering consultations, treatments, or clinical recommendations. | `M01S02` | Keep practitioner identity separate from customer identity. |
| `practice` | The top-level operating business under which one or more clinics may exist. | `M01S03` | Use `practice` for the organization rather than overloading `clinic`. |
| `clinic` | A physical treatment site where appointments, consultations, and treatments may happen. | `M01S03` | `location` is intentionally omitted as a first-class entity for now. |
| `room` | An optional room or resource within a clinic used to represent where work happens. | `M01S03` | This remains optional in the foundation milestone. |
| `treatment` | A catalog record describing a service the practice can offer, present, book, consult on, or later record as performed. | `M01S04` | This is catalog data, not treatment history. |

## Reserved Later-Stage Terms

| Term | Short definition | Primary stage | Notes |
|---|---|---|---|
| `appointment` | A booked or requested customer interaction tied to time, treatment choice, and operational context. | `M02` | Separate from consultation and treatment history. |
| `appointmentEvent` | A durable event or status-history record associated with an appointment. | Future stage | Reserve the name now; detail is deferred until explicitly planned. |
| `consultation` | A pre-treatment assessment and recommendation record for a customer. | `M02S03` | Can exist without a treatment record. |
| `consentTemplate` | A reusable consent definition that can later be issued for specific customer decisions. | `M02S04` | Not the same as a signed or issued consent record. |
| `consentRecord` | A customer-specific record of consent captured for an appointment, consultation, or treatment context. | `M02S04` | Keep separate from templates. |
| `vendor` | A supplier from whom customer-specific products may be purchased. | `M03` | Product purchasing remains customer-specific in this roadmap. |
| `product` | A product catalog record that can be purchased for a customer from a vendor. | `M03` | Keep the core catalog term simple; vendor remains a relationship on the product. |
| `productPurchase` | A purchase record representing products bought for a specific customer. | `M03S02` | Not a general retail order. |
| `productPurchaseItem` | A line item within a customer-specific product purchase. | `M03S02` | Ownership stays with the same customer. |
| `productReceipt` | A record of receipt or intake against a purchased customer-specific product item. | `M03` | Separate from purchase intent and product usage. |
| `productUsage` | A record of how a purchased customer-specific product item was used, wasted, returned, expired, or adjusted. | `M03S03` | Separate from treatment records even when linked. |
| `productAdjustment` | A corrective or operational adjustment to product quantity or status. | `M03` | Keep explicit rather than hiding corrections inside usage notes. |
| `treatmentRecord` | A durable record of treatment actually performed for a customer. | `M03S04` | Approved planning term; current code still uses a legacy name. |
| `treatmentRecordAmendment` | A later amendment or correction linked to a treatment record. | `M03S04` | Preserve treatment history rather than overwriting it silently. |
| `media` | A private photo or video record associated with customer care or operational evidence. | `M04S01` | Reserve generic `asset` for site/content usage; use `media` for customer-care photos and video. |
| `mediaLink` | A link record connecting media to a customer, appointment, consultation, treatment record, or concern. | `M04S01` | Keep linking separate from the media record itself. |
| `mediaConsent` | A record describing customer permission or restriction for media capture or use. | `M04S01` | Separate from general treatment consent. |
| `mediaAccessLog` | An audit-style record of access to private media. | `M04S01` | Focused on media handling, not broad system auditing. |
| `aftercareTemplate` | A reusable set of post-treatment instructions or guidance. | `M04S02` | Template only; not customer-specific delivery. |
| `aftercareRecord` | A customer-specific record showing what aftercare guidance was issued. | `M04S02` | Separate from follow-up outcomes. |
| `followUp` | A follow-up interaction, review, or outcome record after treatment or aftercare. | `M04S02` | Keep distinct from the original treatment record. |
| `customerConcern` | A customer-reported concern, issue, or outcome requiring tracking. | `M04S02` | May relate to a treatment record when appropriate. |
| `auditEvent` | A broad operational audit record used for compliance and hardening. | `M05S01` | Reserve detailed audit design for the operational hardening milestone. |

## Legacy And Conflicting Terms

| Approved term | Legacy or conflicting term | Current repo presence | Decision |
|---|---|---|---|
| `customer` | `patient`, `patientAddress`, `patientID`, `patientMessage` | Legacy migration history and superseded placeholder schema | `customer` is the approved business term. `patient` was removed from the active foundation during `M01S01`. |
| `treatmentRecord` | `treatmentSession`, `treatmentSessionProduct` | Legacy migration history and milestone planning references | `treatmentRecord` is the approved planning term. The old placeholder schema was removed during `M01S01`. |
| `product` | `vendorProduct` | Superseded milestone planning terminology | `product` is the approved catalog term. Keep vendor specificity on the relationship and purchasing records rather than in the core entity name. |
| `media` | `asset`, `assetLink`, `assetConsent`, `assetAccessLog` in the customer-care domain | Superseded milestone planning terminology | Reserve generic `asset` for site/content usage. Use `media*` names for private customer-care photo and video records. |
| `practitioner` | `practitionerUserID` as the only practitioner identity | Legacy migration history and milestone planning references | `practitioner` is the approved domain concept. Later stages should model practitioners through explicit records, not direct placeholder links. |
| Customer contact identity on `user` | `fullName` duplicated on `patient` | Legacy migration history and superseded placeholder schema | Customer contact identity belongs on `user`. Address storage also lives on `user` in the cleaned foundation until richer customer-specific data is introduced deliberately. |

## Conventions

| Surface | Convention | Example |
|---|---|---|
| Database tables | `snake_case`, singular nouns | `customer` |
| Database columns | `snake_case`; foreign keys end with `_id` | `customer_id` |
| Database enums and persisted string literals | lowercase values; use underscores when needed | `no_show`, `contact_only` |
| TypeScript types | `PascalCase` using the approved term | `Customer` |
| TypeScript exported values and schema constants | `camelCase` using the approved term | `customer` |
| Route segments | existing route hierarchy plus kebab-case URL segments | `/manage/user/:userID/profile` |
| UI labels | human-readable labels using the approved business term | `Customer` |
| Documentation terms | use the approved glossary term consistently | `treatmentRecord` |

## Concept Separation Notes

- `user` is the base account; `customer` and `practitioner` are role-specific records.
- `practice` is the operating business; `clinic` is the physical site customers attend.
- `appointment` is separate from `consultation`, `consentRecord`, and `treatmentRecord`.
- `consentTemplate` is separate from `consentRecord`.
- `productPurchase`, `productReceipt`, `productUsage`, and `productAdjustment` are separate concepts.
- `treatment` is the catalog entry; `treatmentRecord` is the record of what was actually performed.
- `media` is separate from `mediaLink`, `mediaConsent`, and `mediaAccessLog`.
- `aftercareRecord`, `followUp`, and `customerConcern` remain separate records even when linked to the same treatment history.

## `M01` Boundary

The `M01` foundation milestone fully owns:

- `user`
- `customer`
- `practitioner`
- `practice`
- `clinic`
- `room`
- `treatment`

The glossary also reserves later terms so future stages do not invent parallel names, but `M01S01` does not lock down implementation details for those later milestones.

## Compact Drift Register

| Drift | Surfaces affected | Impact | Follow-up |
|---|---|---|---|
| `customer` vs `patient` | Legacy migration history and older planning language | Medium | Keep `customer` as the only active foundation term going forward. |
| `treatmentRecord` vs `treatmentSession` | Milestone terminology and legacy migration history | Medium | Use `treatmentRecord` in future planning and implementation work. |
| `media` vs `asset` | M04 planning language and generic site/content terminology | Medium | Reserve `asset` for site/content usage and use `media*` names for customer-care photo/video records. |
| `practitioner` vs `practitionerUserID` | Later-stage planning language | Low | Introduce practitioner records explicitly when the people model is built. |
| Dormant operational placeholder schema vs validated product surface | Legacy operational tables and schema exports removed during cleanup | Resolved in active code | Keep future schema additions tied to validated milestones only. |
| Route hierarchy is user-centric, not customer-entity-centric | Route naming guidance and future admin UI planning | Low | Keep current route structure unless a later stage explicitly redesigns it. |

## Non-Goals For `M01S01`

- Rename code
- Add new feature migrations unrelated to cleanup
- Redesign routes
- Merge later-stage implementation details into `M01`
- Treat this glossary pass as final for every future workflow detail
