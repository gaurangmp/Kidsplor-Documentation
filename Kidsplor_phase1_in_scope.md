# Kidsplor — Parent Portal
## Product Requirements Document (PRD)
### Version 1.0 · Phase 1 (MVP) · SEPT 2026

---

## Document Control

| Field | Detail                                                                         |
|---|--------------------------------------------------------------------------------|
| Product | Kidsplor Parent Portal                                                        |
| Phase | Phase 1 — Parent MVP                                                           |
| Version | 1.0                                                                            |
| Author | Kidsplor Product Team                                                         |
| Status | Draft — For Developer Review                                                   |
| Related Doc | [Kidsplor Provider Portal BRD v1.0](https://gaurangmp.github.io/kidsplor-BRD/) |
| Target Release | TBC                                                                            |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [User Personas](#2-user-personas)
3. [System Overview](#3-system-overview)
4. [Phase 1 — Functional Requirements (MVP)](#4-phase-1-functional-requirements-mvp)
5. [Non-Functional Requirements](#5-non-functional-requirements)
6. [Integration Requirements](#6-integration-requirements)
7. [User Stories — Priority Matrix](#7-user-stories-priority-matrix)
8. [Acceptance Criteria — Critical Flows](#8-acceptance-criteria-critical-flows)
9. [Data Model — Key Entities (Parent-Side, High Level)](#9-data-model-key-entities-parent-side-high-level)
10. [Open Questions for Developer Team](#10-open-questions-for-developer-team)
11. [Glossary](#11-glossary)

---

## 1. Executive Summary

Kidsplor is a two-sided marketplace connecting parents with local kids' activity providers across Australia. The **Parent Portal** is the consumer-facing half of the platform — the surface through which parents discover activities, book classes, pay providers, manage their children's schedules, communicate with providers, and share great experiences with their network.

**Phase 1** delivers a responsive web application covering end-to-end discovery, 3rd Party booking, and communication. 

**Phase 2** delivers a full end to end booking engine with a rich provider interface and booking, notifications, payments functionality

**Phase 3** extends this to native mobile apps, AI-powered search, group payments, and richer parent community features.
 

### Design Principles

- **Trust first.** Every screen reinforces that Kidsplor is a safe, verified, and parent-endorsed platform. For a product involving children, trust signals are existential — not decorative.
- **Frictionless booking.** A parent should be able to find, evaluate, and confirm a booking for their child in under 3 minutes.
- **WOM-native.** The product is designed to make sharing and referral a natural outcome of a great booking experience, not an afterthought.
- **Inclusive by default.** Accessibility and inclusion filters (sensory-friendly, WWCC verified, etc.) are prominent, not buried. Every Australian family should feel Kidsplor was built for them.
- **Mobile-first responsive.** The Phase 1 web app must be fully usable on a smartphone. Native apps follow in Phase 2.

### Relationship to Provider Portal

The Parent Portal consumes the class schedule feed published by providers through the Provider Portal. When a parent books a class, payment is processed via Stripe and flows directly to the provider. Kidsplor deducts a platform fee automatically. The two portals share a unified data model but are separate application surfaces. The native booking experience will be part of Phase 2.

---

## 2. User Personas

### 2.1 The Busy Parent (Primary)
A parent of 1–4 school-age children, searching for activities nearby we are Time-poor. Makes decisions on their phone. Relies on peer recommendations and reviews. Won't tolerate a clunky booking experience. Wants one place to manage all kids' activities.

### 2.2 The Inclusion-Aware Parent (High Priority Segment)
A parent of a child with additional needs (autism, sensory processing, ADHD, physical disability). Currently faces significant friction finding inclusive, verified activities. Will become a highly loyal user and vocal advocate if Kidsplor solves this for them. Specific filter requirements and trust signals apply.

### 2.3 The Research-First Parent
Compares multiple providers before deciding. Reads all reviews. Wants detailed instructor profiles, qualification badges, and transparent pricing. High-value customer once they convert.

### 2.4 The New-to-Area Parent
Recently moved suburb or city. Has no local network yet. Relies heavily on search, map view, and ratings to find trusted local options. Referral features less relevant initially; SEO and map discovery most important.

---

## 3. System Overview

```
Parent Portal (Web App — Phase 1)
├── Landing Page (shared with Provider acquisition)
├── Account & Registration
│   ├── Parent Profile
│   └── Child Profiles (multiple)
├── Search & Discovery
│   ├── Keyword / Category / Location Search
│   ├── Filters (age, accessibility, inclusion, schedule, price)
│   └── Map View or Listings View (TBA)
├── Activity & Provider Listings
│   ├── Provider Profile Page
│   ├── Activity Detail 
│   ├── Reviews
```

---

## 4. Phase 1 — Functional Requirements (MVP)

---

### FR-01: Landing Page


**Priority:** P0

#### FR-01.1 — Purpose
The landing page serves two audiences: parents (searching for activities) and providers (signing up to list). It is the primary conversion surface for both sides of the marketplace.

#### FR-01.2 — Parent-Facing Elements
- Hero section: headline, subheadline, suburb/activity search bar (primary CTA).
- Social proof bar: "Trusted by X,XXX families · X,XXX activities listed · Verified providers".
- Category quick-links (icons): Sports, Arts, Music, Dance, Academic, Drama, Outdoors, Coding.
- "How it works" — 3-step explainer: Search → Book → Go.
- Featured / trending activities (curated by Kidsplor editorial or algorithmic).
- Trust signals section: WWCC Verified, Stripe Payments, Secure Booking guarantee, "As seen in [press logos]".
- Parent testimonials (3–5, with photo and child age).
- Footer: About, How it works, For Providers, Privacy Policy, Terms of Service, Contact.

#### FR-01.3 — Provider-Facing Elements (separate section or tab)
- Use existing [kidsplor business](https://kidsplor.com.au/kidsplor-for-business.html) page for static content. Notable elements as outlined below : 
  - "List your classes in 5 minutes" CTA with link to Provider Portal sign-up.
  - Provider benefit highlights: free listing, direct payments, scheduling tools.
  - Link to provider sign-up / login.

#### FR-01.4 — Technical Notes
- Page must load in < 2s on mobile (LCP target).
- SEO-optimised: structured data (schema.org/LocalBusiness, schema.org/Event) for discovery via Google.
- Meta title/description per city/suburb landing page variant (Phase 1: Melbourne; Phase 2: national).

---

### FR-02: Account & Login

**Priority:** P0

#### FR-02.1 — Registration Options
- Email + password registration.
- Google OAuth (one-click).
- Fields on email registration: first name, last name, email, password (min 8 chars, 1 uppercase, 1 number), suburb/postcode (used to personalise initial search results).
- Email verification required before booking (not before browsing). (Phase 2)

#### FR-02.2 — Login
- Email + password login.
- Google OAuth login.
- "Forgot password" flow via email reset link.
- "Remember me" (persistent session, 30 days).

#### FR-02.3 — Parent Profile
Fields:
- First and last name
- Email address
- Mobile number (used for booking SMS reminders — Phase 2)
- Suburb / postcode
- Profile photo (optional)
- Communication preferences: email notifications (on/off per type), off-platform email (configured; used for message forwarding to parent's inbox)

#### FR-02.4 — Account Security
- Passwords stored as bcrypt hashes. Kidsplor never stores plaintext passwords.
- All sessions use JWT tokens with expiry.
- Option to enable two-factor authentication via email OTP (Phase 2 for SMS 2FA).

---

### FR-03: Child Profiles

**Priority:** P0

#### FR-03.1 — Multiple Children
- A parent account can hold unlimited child profiles.
- Child profiles are separate from the parent account and cannot be made public.

#### FR-03.2 — Child Profile Fields

| Field | Required | Notes |
|---|---|---|
| First name | Yes | Used to display in classes |
| Last name | Yes | Stored but not displayed publicly |
| Date of birth | Yes | Used to auto-filter age-appropriate activities; displayed as age on bookings |
| Gender | No | Optional; used for relevant activity filtering (e.g. girls-only classes) |
| Profile photo | No | Optional; shown on booking confirmations |
| Allergies | No | Free text + common allergy checkboxes (nuts, dairy, gluten, bee stings) |
| Medical notes | No | Free text (e.g. asthma, epilepsy, EpiPen required) |
| Additional needs | No | Multi-select: ADHD, Autism Spectrum, Sensory Processing, Physical Disability, Vision Impairment, Hearing Impairment, Other (free text) |
| Emergency contact | No | Name + phone; shared with provider on booking confirmation |

#### FR-03.3 — Privacy & Data Handling
- Medical notes and additional needs data are classified as **sensitive personal information** under the Australian Privacy Act 1988.
- This data is encrypted at rest (AES-256) and is visible only to:
  1. The parent who entered it.
  2. The provider, in context of an active enrollment (read-only, on the class roster).
- Kidsplor staff do not have routine access to medical/additional needs fields.
- Parent can delete or edit any child profile field at any time.
- Deleting a child profile anonymises (not hard-deletes) historical booking data for compliance.

### FR-04: Search & Discovery — Listings

**Priority:** P0

#### FR-04.1 — Search Bar
- Present on the Parent landing page and persistent across all parent-facing pages.
- Fields:
  - **What**: keyword (free text) or activity category (dropdown/typeahead).
  - **Where**: suburb/postcode/location (autocomplete via Google Places API), or "Use my location".
- Search returns a results page (list + map toggle).

#### FR-04.2 — Natural Language / AI Search (Phase 1 basic, enhanced Phase 2)
- Phase 1: keyword matching with category inference (e.g. "soccer for my 8 year old on weekends" parses to: category=Sports/Soccer, age=8, schedule=weekend).

#### FR-04.3 — Search Results Page
- Default view: list of matching classes/providers.
- Toggle: List view / Map view.
- Sort options: Relevance (default) · Distance · Rating · Price (low to high) · Newest.
- Pagination: infinite scroll (list view); continuous map (map view).

#### FR-04.4 — Search Result Card (List View)
Each card shows:
- Provider photo / class photo
- Provider name
- Class name
- Activity category tag
- Age group (e.g. "Ages 5–8")
- Day(s) + time
- Suburb + distance from search location
- Price (per session or from $X/term)
- Star rating + review count (e.g. ★ 4.9 · 47 reviews)
- Trust badge(s): WWCC Verified ✓, Top Provider ⭐, Accessibility icons (wheelchair, sensory) 
- CTA: "Book Now" or "View Details" 

#### FR-04.5 — Filters Panel
Filters appear as a collapsible panel on the left (desktop) or as a bottom sheet (mobile). All filters are multi-select unless noted.

- Day of week (Mon–Sun, multi-select)
- Time of day: Morning / Afternoon / Evening
- Start date (date picker)

**Activity Filters**
- Activity category (multi-select from taxonomy)
- Age group (slider: 0–18)
- Session type: Single session / Recurring / Trial / Camp / Holiday Program

**Price Filters**
- Price range (slider: $0 – $200+)
- Free or trial sessions only (toggle)

**Inclusion & Accessibility Filters** *(surfaced prominently; not buried)*
- WWCC Verified ✓ (toggle — on by default)
- Neuro-diversity friendly
- Sensory-friendly environment
- Autism-friendly
- Wheelchair accessible

**Provider Filters**
- Minimum rating (★ 4.0+, ★ 4.5+, ★ 4.8+)
- Verified providers only
- Has video intro
- Accepts online payments (vs. enquire only)

### FR-05: Provider & Class Listings

**Priority:** P0

#### FR-05.1 — Provider Profile Page
A dedicated page for each provider, containing:

**Header Section**
- Cover photo + provider logo/avatar
- Provider name, category tag(s)
- Star rating + review count (e.g. ★ 4.9 · 138 reviews)
- Suburb + distance from parent's location
- Trust badges (WWCC Verified, Top Provider, Accessibility badges)
- Response rate ("Typically responds within 2 hours")
- "Message Provider" button
- "Share" button (generates shareable URL with referral tracking)

**Trust & Verification Section** *(prominent, not buried)*
- WWCC Verified ✓ (with verification date)
- ABN Registered ✓
- Public Liability Insurance ✓
- First Aid Certified (if declared)
- Kidsplor tier badge (Verified / Top Provider / Kidsplor Select)

**About Section**
- Provider description
- Years in operation (if provided)
- Languages offered

- List or grid of all active classes offered by this provider.
- Each class card shows: class name, age group, day/time, price, spots available, CTA.
- Filterable by age group, day, class type.

- Coach profile cards: photo, name, role, brief bio, qualifications.
- Clicking a coach card opens the Instructor Profile (FR-06.3).

**Photos & Videos Section** 
- Photo gallery (grid, lightbox on click).
- Intro video embed (YouTube/Vimeo) if provided.

**Reviews Section** 
- Retreive review from Google if available for the Business ID

**Location & Venues Section**
- Embedded map showing venue location(s).
- Address, parking notes (if provided by provider).
- Link to Google Maps directions.

#### FR-05.2 — Class Detail Page (single class/lesson details card for phase 1 )
A dedicated page for each class, containing:
- Class name, provider name (link to provider profile)
- Activity category, age group
- Class description
- Schedule (recurring days/times, term dates)
- Venue (map embed)
- Price: per session and/or term/block pricing
- Instructor (link to instructor profile)
- What to bring (if provided by provider)
- Cancellation / refund policy
- Inclusion attributes applicable to this class
- Reviews specific to this class (filtered from provider reviews)
- "Book Now" CTA takes you to providers booking page
- "Save to Favourites" toggle
- Share button

### FR-06: Booking Flow

**Priority:** P0

#### FR-06.1 — Booking Entry Points 
A parent can initiate a booking from:
- The "Book Now" button on a Class card (search results).
- The "Book Now" CTA on the Class Detail Page.

### FR-07: Provider Listings
**Priority:** P0

#### FR-07.1 — Provider Listings Compare Page

On the listing view, the parent can compare upto 3 listings using a compare button.

- Parent can view a side by side compare on listings.
- The parent can:
  - View Price, Rating, Schedule, Frequency, Delivery, Trial, Distance, Address, Contact no.
  - CTA Favorites, Directions and Call
---

### FR-08: Scraper or a Service to Enter Providers in Bulk

**Priority:** P0

- A service to search for Providers from online Social communities such as Facebook group posts, instagram, TikTok and/or Google business listings
- A way for Kidsplor team to curate the Providers and advertise


### FR-09: Responsive Website (Mobile-First)

**Priority:** P0

- The Phase 1 Parent Portal is a **responsive web application**, fully usable on mobile browsers (Safari iOS, Chrome Android) without a native app.
- Key mobile UX requirements:
  - Search bar accessible within one tap from any page.
  - Booking flow completable with one thumb on a small screen.
  - Filters accessible as a bottom sheet (not a sidebar) on mobile.
  - Map view uses full-screen on mobile.
  - Bottom navigation bar on mobile: Home, Search, My Bookings, Messages, Profile.
- Performance targets (mobile): LCP < 2.5s, FID < 100ms, CLS < 0.1 (Core Web Vitals).

---

## 5. Non-Functional Requirements

| Ref | Requirement | Target |
|---|---|---|
| NFR-01 | Booking completion time | A logged-in parent with a saved payment method can complete a booking in < 3 minutes |
| NFR-02 | Page load time (desktop) | < 2s for landing, search results, and provider profile pages (p95) |
| NFR-03 | Page load time (mobile) | Core Web Vitals: LCP < 2.5s, FID < 100ms, CLS < 0.1 |
| NFR-04 | Search response time | Search results returned in < 1.5s after form submission |
| NFR-05 | Real-time spot count | Spot count on class cards updates within 5 seconds of a new booking |
| NFR-06 | Uptime | 99.5% monthly uptime |
| NFR-07 | Browser support | Chrome, Firefox, Safari, Edge — latest 2 versions; Safari iOS 15+; Chrome Android |
| NFR-08 | Data security | All PII encrypted at rest (AES-256) and in transit (TLS 1.2+) |
| NFR-09 | Sensitive data | Medical notes / additional needs data encrypted with separate key; access-logged |
| NFR-10 | Payment security | PCI-DSS compliant via Stripe; no raw card data touches Kidsplor servers (out of scope for phase 1)
| NFR-11 | Privacy Act 1988 | Compliant with Australian Privacy Act; Privacy Policy accessible from all pages |
| NFR-12 | SEO | Server-side rendering (SSR) or static generation for all public-facing pages (landing, search, provider/class pages) for Google indexability |
| NFR-13 | Email deliverability | Transactional email via SendGrid or Postmark with DKIM/SPF/DMARC configured |
| NFR-14 | GDPR-adjacent | Data deletion on request within 30 days; data portability on request |

---

## 6. Integration Requirements

| Integration | Purpose | Phase |
|---|---|---|
| **Stripe** | Payment processing, Apple Pay, Google Pay, refunds, saved cards | 2 |
| **Google OAuth 2.0** | Parent sign-in with Google | 1 |
| **Google Maps / Places API** | Location autocomplete, map view, venue maps | 1
| **Google Maps Geocoding API** | Convert suburb/postcode to lat/lng for proximity search | 1
| **Bureau of Meteorology / OpenWeatherMap API** | Weather forecast for indoor-only filter promotion | 3
| **SendGrid / Postmark** | Transactional email delivery | 2 |
| **ABN Lookup API** (abr.business.gov.au) | Validate provider ABN (referenced from Provider BRD) | 3
| **Twilio** | SMS booking reminders and notifications | 3
| **Google Calendar API** | Two-way calendar sync | 3
| **Apple Calendar (CalDAV)** | Calendar sync for iOS | 3

---

## 7. User Stories — Priority Matrix

| ID | User Story | Phase | Priority | Phase
|---|---|---|---|---|
| US-P-01 | As a parent, I can find kids' activities near me by suburb, type and age group | P1 | P0 | 1
| US-P-02 | As a parent, I can see activities on a map and search continuously as I pan | P1 | P0 | 1
| US-P-03 | As a parent, I can filter results by WWCC verified, sensory-friendly, wheelchair accessible etc. | P1 | P0 | 1
| US-P-04 | As a parent, I can create multiple child profiles with age, allergies and medical notes | P1 | P0 |1
| US-P-05 | As a parent, I can book a single casual class session for my child in under 3 minutes | P1 | P0 | 2
| US-P-06 | As a parent, I can enrol my child in a recurring weekly term | P1 | P0 |2
| US-P-07 | As a parent, I can book a free or paid trial class | P1 | P0 |2
| US-P-08 | As a parent, I can join a waitlist when a class is full and be notified when a spot opens | P1 | P0 |2
| US-P-09 | As a parent, I can pay with a saved card, Apple Pay, or Google Pay | P1 | P0 |2
| US-P-10 | As a parent, I can see a transparent fee breakdown before I pay | P1 | P0 |2
| US-P-11 | As a parent, I can save payment methods securely for future bookings | P1 | P0 |2
| US-P-12 | As a parent, I receive a booking confirmation email and PDF receipt | P1 | P0 |2
| US-P-13 | As a parent, I can view all my upcoming bookings in one place | P1 | P0 |2
| US-P-14 | As a parent, I can cancel a booking and receive a refund per the provider's policy | P1 | P0 |2
| US-P-15 | As a parent, I can see WWCC, insurance and ABN verification badges on provider listings | P1 | P0 |2
| US-P-16 | As a parent, I can read verified reviews from other parents with real bookings | P1 | P0 | 1
| US-P-17 | As a parent, I can leave a star rating and written review after my child attends a class | P1 | P0 |2
| US-P-18 | As a parent, I can message a provider before booking with an enquiry | P1 | P0 |2
| US-P-19 | As a parent, I can message a provider after booking (schedule change, catch-up, absence) | P1 | P0 |2
| US-P-20 | As a parent, I can receive provider messages in my email inbox as well as in-app | P1 | P0 |2
| US-P-21 | As a parent, I can register and log in with Google | P1 | P0 |1
| US-P-22 | As a parent, I receive email reminders 24 hours and 2 hours before a session | P1 | P0 |2
| US-P-23 | As a parent, I can view instructor/coach profiles and qualifications | P1 | P0 |2
| US-P-24 | As a parent, I can see real-time spot availability and urgency signals on class cards | P1 | P1 |2
| US-P-25 | As a parent, I can save providers / classes to a favourites list | P1 | P1 |1
| US-P-26 | As a parent, I can view my full booking history and download receipts | P1 | P1 |2
| US-P-27 | As a parent, I can comapre listings/classes | P1 | P0 |1
| US-P-28 | As a parent, I can use natural language search to find activities | P2 | — |2

---

## 8. Acceptance Criteria — Critical Flows

### AC-01: Search & Discovery
- [ ] Searching "soccer 8 year old Melbourne" returns relevant class results within 1.5s.
- [ ] Enabling the "Sensory-friendly" filter removes all providers who have not self-declared this attribute.

### AC-02: Child Profiles
- [ ] A parent can create, edit, and delete a child profile.
- [ ] Medical notes and additional needs fields are not visible in any public-facing UI.
- [ ] When a child is selected in the booking flow, their additional needs note is shown with a "Shared with provider" disclosure.

## 9. Data Model — Key Entities (Parent-Side, High Level)

```
Parent
 └── has many: ChildProfiles
 └── has many: Bookings
 └── has many: SavedPaymentMethods (via Stripe)
 └── has many: InboxThreads
 └── has many: Reviews
 └── has many: Favourites (Class | Provider)
 └── has one: ReferralCode
 └── has one: NotificationPreferences

ChildProfile
 └── belongs to: Parent
 └── sensitive fields: allergies, medicalNotes, additionalNeeds [encrypted]
 └── has many: Bookings

Booking
 └── belongs to: Parent, ChildProfile, ClassSession
 └── type: SingleSession | Recurring | Trial | Waitlist
 └── has one: Payment
 └── status: Confirmed | Pending | Waitlisted | Cancelled | Completed

Payment
 └── belongs to: Booking
 └── references: Stripe PaymentIntent
 └── fields: amount, platformFee, currency, status, method

Review
 └── belongs to: Parent, Provider, Class (optional)
 └── requires: completed Booking (verified)
 └── fields: rating, text, photos[], tags[], providerReply

InboxThread
 └── belongs to: Parent, Provider
 └── type: Enquiry | ScheduleChange | CatchupRequest | AbsenceNotice | General
 └── has many: Messages

```

---

## 10. Open Questions for Developer Team

| # | Question | Owner | Due | Phase |
|---|---|---|---|---|
| OQ-P-01 | Weather API: BOM doesn't have a clean public API. Recommend OpenWeatherMap (free tier sufficient for Phase 1). Confirm.  | Tech Lead | — | 3
| OQ-P-02 | Natural language search (FR-04.2 Phase 1 basic): is intent parsing done server-side via OpenAI function calling, or a custom classifier? | Tech Lead | — | 3
| OQ-P-03 | Medical / additional needs data: confirm encryption key management strategy (envelope encryption with KMS?). Data access logging required. | Tech Lead + Security | — | 1
| OQ-P-04 | Apple Pay domain verification: requires `.well-known/apple-developer-merchantid-domain-association` file on the domain. Confirm infra setup. | Tech Lead | — |2
| OQ-P-05 | WWCC verification: Phase 1 is manual (Kidsplor admin user confirms). What is the SLA for verification? Does this gate listing going live? | Product / Ops | — |1
| OQ-P-06 | Referral credit: implemented as Stripe coupon / promotion code, or as a Kidsplor internal credit ledger? | Tech Lead | — |2
| OQ-P-07 | Community verification for accessibility attributes (FR-04.7): what is the threshold for upgrading from "provider declared" to "community verified"? Recommend 3 confirmed reviews. | Product | — |3
| OQ-P-08 | Schools directory: should school pages be indexable by Google? If yes, SSR/SSG is required. Confirm rendering strategy for school pages. | Tech Lead | — |3
| OQ-P-09 | Kidsplor booking fee passed to parent vs. absorbed: confirm commercial decision — is the $0.95 fee shown to parent or absorbed into the class price shown? | Product / Commercial | — |3

---

## 11. Glossary

| Term | Definition |
|---|---|
| Parent | The adult user of the parent-facing portal who books activities on behalf of their child(ren) |
| Child Profile | A sub-profile under a parent account representing one child, including sensitive medical/needs data |
| Provider | A business listed on Kidsplor offering kids' activities (see Provider BRD) |
| Class | A recurring or one-off activity offered by a provider |
| Session | A single occurrence of a Class (e.g. one Saturday at 9am) |
| Booking | A confirmed reservation linking a Child to a Class Session, with payment |
| Waitlist | A queue of parents who want to enrol when a class is full |
| Enrolment | Synonymous with Booking; used interchangeably in provider context |
| Stripe | Payment infrastructure used for all transactions on Kidsplor |
| Platform Fee | Kidsplor's charge per transaction (currently shown as booking fee to parent) |
| WWCC | Working With Children Check — a government-issued safety screening for those working with children in Australia |
| NDIS | National Disability Insurance Scheme (Australia) |
| Trust Badge | A visual indicator on a provider listing confirming a verified attribute (WWCC, insurance, etc.) |
| WOM | Word of Mouth — a primary growth mechanic for Kidsplor |
| Community Verified | An accessibility attribute confirmed by ≥3 parent reviews, upgrading from provider self-declaration |
| Natural Language Search | Search using conversational input (e.g. "indoor soccer for a 7-year-old on weekends") rather than structured fields |

---

*End of Document — Kidsplor Parent Portal PRD v1.0*
*For questions, contact the Kidsplor Product Team.*
*Related: Kidsplor Provider Portal BRD v1.0*
