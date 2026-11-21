# Bangladesh University of Textiles (BUTEX) — University Website

A multi-page university website with a public site, a student portal and an admin
dashboard. Built with Next.js (App Router), TypeScript, Tailwind CSS v4, Lucide icons
and PostgreSQL via Drizzle ORM.

## What is included

| Area | Route | Notes |
| --- | --- | --- |
| Homepage | `/` | Hero slider, quick links, statistics, about, departments, research, notices, events, news, campus life |
| About | `/about` | History timeline, mission and vision, leadership, Vice Chancellor's message, achievements, accreditation |
| Academics | `/academics` | Program levels, calendar, downloadable documents |
| Departments | `/departments`, `/departments/[slug]` | Nine departments, each with eight sections |
| Admission | `/admission` | UG/PG, circulars, eligibility, guidelines, results, dates, FAQ |
| Research | `/research` | Eight research themes |
| Campus life | `/campus-life` | Organizations, clubs, sports, facilities, residence, welfare, interactive map |
| Notice board | `/notices`, `/notices/[id]` | Search, category filters, PDF attachments |
| News / Events | `/news`, `/events` | News stories and a month calendar |
| Faculty directory | `/faculty` | Search by name, department, designation |
| Contact | `/contact` | Details, contact form (stored in the database), map |
| Search | `/search` + header dialog (`Ctrl/⌘ K`) | Pages, departments, notices, news, events, faculty, documents |
| Student portal | `/portal` | Dashboard, calendar, routines, results, materials, labs, notices |
| Admin dashboard | `/admin` | Notices, news, events, departments, faculty, admission, documents, site content, statistics, messages |

Site-wide: light and dark themes, English and Bengali interface, larger-text toggle,
breadcrumbs, skip link, reduced-motion support, sitemap, robots.txt and structured data.

## Demonstration vs. production

This build is honest about what is real:

- **Verified facts** (address, phone, email, history milestones, legal basis) come from
  public sources and are flagged on the site as pending official review.
- **Nothing is invented.** Statistics, office-holders, faculty names, mission and vision
  text and accreditation details are left empty and render an "awaiting official
  information" placeholder until an administrator fills them in.
- **Sample records** (notices, news, events, documents, faculty slots) are seeded so
  every feature can be exercised. They carry a "Sample" or "Placeholder" badge.
- **Student portal data** (courses, timetable, grades) is demonstration data and is
  labeled as such. Notices, calendar entries and documents in the portal are live.
- **Authentication** is real (salted scrypt hashes, hashed session tokens, role checks,
  rate limiting) but ships with demonstration accounts:

  | Role | ID | Password |
  | --- | --- | --- |
  | Student | `DEMO-0001` | `butex-student-demo` |
  | Admin | `admin@butex.demo` | `butex-admin-demo` |

  Set `ADMIN_EMAIL` and `ADMIN_PASSWORD` before the first start to create a private
  administrator instead; the demo credentials are then no longer displayed.
  Connect the university's identity provider before launch.

## Photography

`src/lib/images.ts` is the single registry of images with photographer, licence and
source. Photographs of the BUTEX campus and laboratories come from Wikimedia Commons
(CC BY-SA). Subject illustrations are Pexels stock photos, labeled "Illustrative image"
on the site, and should be replaced with official photography. See `/credits`.

The official emblem of Bangladesh University of Textiles (BUTEX) is implemented in `src/components/logo.tsx` and `src/app/icon.svg`, featuring the 24-tooth gear with weaving loom and shuttle, the university motto "Knowledge is Power", and ESTD - 2010.

## Running locally

```bash
npm install
npx drizzle-kit push     # create tables
npm run dev
```

The database seeds itself on the first request (`src/lib/seed.ts`). If the database is
unreachable, public pages still render using the defaults in `src/lib/seed-data.ts`.

Environment variables:

| Variable | Purpose |
| --- | --- |
| `DATABASE_URL` | PostgreSQL connection string (required) |
| `NEXT_PUBLIC_SITE_URL` | Canonical origin for metadata, sitemap and robots |
| `ADMIN_EMAIL`, `ADMIN_PASSWORD` | Optional private administrator created at first start |

## Project structure

```
src/
  app/(site)/        public pages sharing the header and footer
  app/portal/        student portal
  app/admin/         admin dashboard
  app/api/           route handlers (auth, search, files, contact, portal, admin CRUD)
  components/        reusable UI (ui.tsx primitives, cards, header, footer, widgets)
  db/schema.ts       Drizzle schema
  lib/               data access, auth, seeding, validation, formatting, site constants
```

Admin CRUD is driven by two small configuration files: `src/lib/admin-resources.ts`
(server-side whitelist and validation) and `src/components/admin/config.ts` (forms and
tables). Adding a field means editing those two files and the schema.

## Known limitations

- Bengali covers navigation, headings, buttons and key homepage text. Long-form content
  and database records are shown in the language they were written in.
- The campus map is an illustrative schematic, not a surveyed plan.
- Uploaded PDFs are stored in PostgreSQL (8 MB limit). Move to object storage if the
  archive grows large.
