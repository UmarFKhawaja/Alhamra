# Aphrodite Roadmap

Last audited: 2026-06-22

This roadmap reflects the current codebase in `app/routes`, `app/lib/server`, and the baseline homepage content provided through the database migrations.

## Current Product Status

The project has a solid foundation in seven areas:

1. Public homepage rendering from database-backed content
2. Authentication and account lifecycle flows
3. Admin tools for site-content, articles, assets, user management, and practice management
4. A managed article system powering `explore/*` and `browse/*` public pages
5. S3-backed asset storage with managed upload, replacement, and scheduled deletion
6. Database schema for auth, content, users, customers, practitioners, and practice structure
7. Practice, clinic, and room management foundations under `/manage/practice`

The main gap is that clinic operations domains beyond the newly accepted practice/clinic foundation (appointments, treatments, products, orders) remain in planning or placeholder stages. The homepage, auth flows, articles, asset management, and foundational admin editing experience are still further along than the operational side.

## Implemented Now

### Public experience

- `/` renders from stored homepage content via `getStoredHomePageDocument()` in `app/lib/server/content/home-page.ts`
- Homepage content is managed as structured data, not hardcoded page copy
- Navigation promos, grouped promo links, CTA text, categories, features, footer links, and brand assets are all editable
- `/explore/*` and `/browse/*` render published articles via catch-all routes backed by the managed article system
- Articles are stored as markdown in S3 and rendered to HTML with `marked`
- Article pages share the same homepage chrome (navigation, footer, branding)

### Authentication and account lifecycle

- `/sign-in` (password sign-in, email OTP sign-in, Apple and Google provider support)
- `/sign-up`
- `/sign-out`
- `/reset-password`
- `/change-email/:token`
- `/complete-account-setup` (admin-creates-user flow: verify email, then set password)
- Auth handler is proxied through `/api/auth/*`
- Mail delivery via Mailjet with React Email templates (not Svelte)
- Session management via `app/lib/server/session.ts`

### User and account management

- `/manage/account/profile` (editable customer data; requires telephone plus address)
- `/manage/account/security` (password change, external account linking)
- `/manage/user/:userID/profile` (admin-managed user profile with role-aware access)
- `/manage/user/:userID/security` (admin-managed user security)
- `/manage/practice/company` (singleton practice management surface)
- `/manage/practice/clinics` (clinic listing and clinic management flow)
- `/manage/practice/clinic/:clinicID/room/:roomID` (room create/edit flow inside a clinic)
- `/manage/practice/users` (user listing with pagination across customer, practitioner, owner, and roleless users)
- `/create/user` (admin creates user without password; verification email sent; user completes setup afterward)
- Role hierarchy: owner > administrator > practitioner = customer

### Site content management

- `/manage/site/identity` (brand name, logo, icon — light and dark mode)
- `/manage/site/search` (search link label and href)
- `/manage/site/hero` (background image, title, description, CTA)
- `/manage/site/promos` (link and group promo items with images and derived CTA text)
- `/manage/site/categories` (category cards with images and browse-all link)
- `/manage/site/features` (callout and collection feature items)
- `/manage/site/footer` (link groups, newsletter, copyright)
- `/manage/site/newsletter` (heading, description, labels)
- `/manage/site/copyright` (years, name, link)
- All content editing uses a JSON-based editor form with Zod validation
- Homepage normalization supports legacy content shapes and auto-derives promo CTA text

### Practice management

- `/manage/practice/company` (singleton practice management surface, currently focused on company/legal details)
- `/manage/practice/clinics` (clinic listing)
- `/manage/practice/clinic/:clinicID` (clinic details, address management, and room navigation)
- `/manage/practice/clinic/:clinicID/room/:roomID` (room create/edit flow)
- Rooms are managed through dedicated room pages while remaining listed from the clinic screen
- Practitioner access is managed from user-profile workflows rather than the clinic page
- Practice management is intentionally separate from public-facing site management

### Article and asset management

- `/manage/site/articles` (paginated article listing)
- `/create/article` (create new article with title, slug, status, markdown body)
- `/manage/article/:slug` (edit existing article — save and delete actions)
- Articles support `browse/*` and `explore/*` slug prefixes; slug validation enforces these
- Article markdown is stored in S3; metadata stored in managed article DB records
- `/manage/site/assets` (paginated asset listing with upload, schedule-delete, undo-delete)
- `/manage/site/asset/:assetID` (single asset: create, replace, or remove)
- Assets served via `/content/assets/*` proxy route from S3
- Scheduled asset deletion with 30-second delay and background reconciliation
- Asset usage tracking across articles and homepage (shows "used by" references)
- Support for image (avif, gif, jpeg, jpg, png, svg, webp) and video (mov, mp4, webm) assets
- Upload size limit: 10 MB

### Data and backend foundation

- MySQL database with Drizzle ORM (`drizzle-orm/mysql2`)
- `app/lib/server/db/client.ts` — database client with entity and relation schemas
- Entity tables: `user`, `account`, `session`, `verification`, `customer`, `practitioner`, `userRole`, `practice`, `clinic`, `room`, `practitionerPractice`, `practitionerClinic`, `site` (homepage), `site_content` (homepage content sections)
- Managed article records and managed asset records (in `site` / `site_content` pattern)
- `npm run db:migrate` applies schema changes and seeds baseline managed content
- S3 storage layer for articles (markdown body), managed assets (images/videos), and other content blobs
- `app/lib/server/content/assets.ts` — full S3 client with multipart upload, ACL, endpoint/bucket detection
- Authorization: `app/lib/authz/roles.ts` with role ranks, access checks, and content management guards
- Shared types in `app/lib/types/` (home, content, article, auth, managed-assets)

## Partially Implemented (Placeholder Routes)

These routes exist but render only a `UserPlaceholderCard` with descriptive text. No real data loading, form handling, or backend wiring:

- `/manage/account/appointments`
- `/manage/account/orders`
- `/manage/account/purchases`
- `/manage/account/treatments`
- `/manage/user/:userID/appointments`
- `/manage/user/:userID/orders`
- `/manage/user/:userID/purchases`
- `/manage/user/:userID/treatments`

The navigation model and layout shell anticipate these areas, but no database tables exist yet for appointments, orders, purchases, or treatment records. These are blocked on schema work defined in the milestones.

## Milestone Tracking

- `M01S01` — Accepted
- `M01S02` — Accepted
- `M01S03` — Accepted
- `M01S04` — Next foundation stage to start after this branch merges

The foundation milestone is still in progress overall because the treatment-catalog stage (`M01S04`) remains outstanding.

## Missing Public Routes Referenced By Seeded Content

These destinations are linked from the seeded homepage content but do not currently exist as routes:

### Booking and utility

- `/book/appointment`
- `/search`
- `/shop`
- `/buy`

### Practice and policy

- `/view/practitioner`
- `/view/safety-policy`
- `/view/aftercare-policy`
- `/view/terms-and-conditions`
- `/view/privacy-policy`

### Shop content

- `/shop/products/clinic-skincare`
- `/shop/products/aftercare`
- `/shop/products/daily-essentials`
- `/shop/products/gift-vouchers`

Note: the `/explore/*` and `/browse/*` paths that were listed as missing in the prior roadmap are now implemented via the article system. Specific article slugs (e.g. `/explore/treatments/consultations`) will render if a matching published article exists; otherwise they return 404.

## Recommended Next Milestones

### Milestone 1: Complete M01 foundation work (see documents/milestones/M01)

Goal: finish the foundation milestone now that `M01S03` practice and clinic structure is accepted.

- Deliver `M01S04` treatment catalog work
- Define the first real treatment records dependency surface for later milestones
- Keep new implementation aligned with the accepted glossary and ADR set

### Milestone 2: Complete the managed content architecture

Goal: close the content-management loop so all homepage-linked destinations resolve to manageable content.

- Build `/view/*` routes backed by the article system (or similar managed content)
- Build `/search` page
- Ensure all seed data links point to working routes
- Add `/manage/site/preview` route to preview homepage changes before publishing

### Milestone 3: Deliver the first real booking flow (see documents/milestones/M02)

Goal: turn the homepage CTA into a working clinic conversion path.

- Define `appointment`, `availability_slot`, and `treatment` database tables
- Build `/book/appointment` with treatment, practitioner, and slot selection
- Connect appointment creation to schema entities
- Add practitioner availability management
- Build customer-facing appointment review under `/manage/account/appointments`

### Milestone 4: Build out placeholder account areas

Goal: make the account area useful after sign-in and for practitioners/admins.

- Appointments list and detail views (customer and practitioner perspectives)
- Treatment record history (see M03)
- Purchases/orders views (see M05)
- Role-aware practitioner/admin operational views

### Milestone 5: Operational domains (see documents/milestones/M03, M04, M05)

Goal: build treatment records, evidence, follow-up, and operational hardening.

- Customer-specific products and treatment records (M03)
- Treatment evidence: before/after media, consent records, aftercare plans (M04)
- Operational hardening: audit logs, data retention, backup (M05)

### Milestone 6: Commerce decision

Goal: clarify whether Aphrodite is informational only or includes real commerce.

- If commerce is planned:
  - define basket and checkout scope for `/buy`
  - build catalog pages for `/shop` and `/shop/products/*`
  - add `product`, `batch`, and `order` tables
- If commerce is not planned:
  - replace shop/buy links with consultation or contact flows

## Notes

- The app uses React Router v7 with file-based routing in `app/routes/`. Route configuration lives in `app/routes.ts`.
- The database uses MySQL via `drizzle-orm/mysql2`. The `site` table holds homepage documents; `site_content` holds content sections, managed articles, and managed assets (distinguished by `type` enum).
- Address fields (`addressLine1`, `addressLine2`, `townCity`, `county`, `postcode`, `country`) and `telephoneNumber` live directly on the `user` table — there is no separate `address` table.
- The old roadmap referenced `src/` paths; the app was migrated to `app/` with the move to React Router v7. The `src/lib/data/home-page.ts` no longer exists — homepage content comes from `app/lib/server/content/home-page.ts`.
- `/manage/site/preview` is not currently a real route.
- The asset deletion system uses a global scheduler (`startManagedAssetDeletionScheduler`) with in-process timers. Assets are soft-deleted (marked pending, 30-second delay) then hard-deleted if unreferenced.
- Article assets referenced in markdown are auto-collected via URL scanning. Managed asset URLs in markdown are normalized to the `/content/assets/*` proxy path on render.
- The most mature parts of the app today are auth, admin content editing, the article system, asset management, and the user-creation lifecycle.
- The biggest product risk remains the mismatch between polished content infrastructure and the absence of operational clinic features (appointments, treatments, products).
