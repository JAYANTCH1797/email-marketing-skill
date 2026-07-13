# Emailer types — the taxonomy spine

> The **first axis** of the 3-axis design system (email-templates Step 2). An *emailer type* =
> **genre × occasion**. Every type carries **defaults for the other two axes** — a template
> structure (`references/template-structures.md`) and a hero image structure
> (`references/layout-patterns.md`) — plus a default image-source recommendation.
>
> **How to use:** (1) detect the type from the brief, (2) look up its row for the recommended
> structure + hero + image source, (3) run the guided intake (`references/intake-questions.md`) to
> **confirm or override each axis with the user** before building. The defaults are starting points,
> never silent decisions — see the ask rule in `intake-questions.md`.

## The three axes at a glance

| Axis | Question it answers | Reference | Exhaustive menu |
|---|---|---|---|
| **1. Emailer Type** | *What kind of email is this?* | this file | genre × occasion (below) |
| **2. Template Structure** | *What is the body skeleton?* | `template-structures.md` | named section-block sequences |
| **3. Hero Image Structure** | *What is the top moment + how do image & text combine?* | `layout-patterns.md` | 9 core + 6 extended archetypes; text strategies A/B/C |
| **+ Image Source** | *AI-generated, user-provided, or none?* | this file + SKILL Step 6 | generate · user-asset · mix · CSS-only |

## Hero-text strategy legend (used in every table below)

- **A** — bake the headline INTO the image (`overlay-text`). Art-directed; image-gen only; needs scrim + safe-zone; no `<h1>` repeating it.
- **B** — live HTML text over a plain image (deliberate empty safe-zone + dark scrim). Accessible, localizable, editable.
- **C** — no overlay (`standard-photo` or `image-only`); headline is HTML below/above the image. Safest.
- **CSS** — no hero photo at all; the hero is built from type + color (`color-block` / `status-card` / `text-only`).

## Image-source default legend

- **generate** — no brand asset; create with the image-gen skill (lifestyle, illustration, baked-art).
- **user-asset** — real first-party photo/render strongly preferred (credibility/accuracy); ask the user for it.
- **mix** — hero generated, product/asset cells user-provided (or vice-versa).
- **none** — CSS-only; generate nothing.

---

## 1 · Ecommerce / fashion / apparel

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| New collection drop / lookbook / brand-moment | `ecom-collection-drop` | `image-only` | C | user-asset → generate |
| Seasonal sale / markdowns / BFCM (promotion-led) | `ecom-sale-standard` | `text-over-image` | A | generate (sale hero) · mix |
| Editorial 'The Edit' / trend round-up / gift guide | `ecom-editorial-edit` | `split` | C | user-asset |
| Single-product / new-arrival spotlight / restock | `ecom-product-spotlight` | `image-only` | C | user-asset |
| Flash promo / last chance / asset-light offer | `ecom-flash-promo` | `color-block` | CSS | none |
| Multi-department 'New In' mosaic | `ecom-category-mosaic` | `product-cutout` | C | user-asset |

## 2 · D2C / CPG / supplement / beauty / food

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| Lifestyle newsletter / 'day with the product' | `d2c-lifestyle` | `image-only` | C | generate · user-asset |
| Product launch / new flavor or shade / hero-SKU promo | `d2c-launch` | `product-cutout` | C | user-asset (cutout) · generate |
| Ingredient / science / 'how it works' explainer | `d2c-science` | `split` | C | mix (cutout + generated context) |
| Replenishment / subscription reorder reminder | `d2c-replenish` | `product-cutout` | C | user-asset |
| Minimalist announcement / back-in-stock / waitlist | `d2c-restock` | `color-block` | CSS | none · user-asset |
| Playful sale / variety-pack / community-UGC | `d2c-playful-sale` | `baked-art` | A | generate |

## 3 · SaaS / B2B tech

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| Major product / platform launch (flagship) | `saas-launch` | `illustrated-banner` | A* | generate (illustration) · user UI |
| Changelog / 'what's new' roundup | `saas-changelog` | `color-block` | CSS | user UI screenshots |
| Single-feature / integration spotlight | `saas-feature` | `text-over-image` | B | user UI · generate |
| Onboarding day-0 / nurture / founder note | `saas-founder-note` | `text-only` | CSS | none |
| Company announcement (funding / pricing / event) | `saas-announcement` | `color-block` | CSS | none · generate |
| Activation / getting-started checklist | `saas-onboarding-checklist` | `split` | C | user UI |
| Transactional / system notice (receipt, invite) | `transactional-receipt` | `status-card` | CSS | none |

\* SaaS `illustrated-banner` bakes the tagline as vector type but **must** restate it as a live HTML H1 below (a11y + dark mode + blocked images). Never photography/people.

## 4 · Consumer hardware / wearables / electronics

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| Product reveal / 'it's here' launch | `hardware-reveal` | `image-only` | C | user-asset (render) |
| Spec showcase / 'meet the technology' (dark) | `hardware-spec` | `text-over-image` | A | user-asset · generate |
| Pre-order / reservation drive (scarcity + ship date) | `hardware-preorder` | `split` | C | user-asset |
| Lifestyle-led / aspirational launch | `hardware-lifestyle` | `image-only` | C | generate · user-asset |
| Comparison / gen-over-gen upgrade nudge | `hardware-comparison` | `split` | C | user-asset (two matched renders) |
| Pre-launch teaser / countdown | `hardware-teaser` | `color-block` | CSS | none |

## 5 · Transactional (commerce, account, booking)

> **Trust genre.** Real catalog/first-party assets or NO image. Never marketing imagery (erodes trust, trips spam). Key facts/codes/amounts are always live selectable text.

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| E-commerce order confirmation | `transactional-receipt` | `status-card` | CSS | none · real thumbnails |
| Shipping / delivery / tracking | `transactional-shipping` | `status-card` (+ `progress-stepper`) | CSS | none |
| Receipt / invoice (SaaS, payment, subscription) | `transactional-invoice` | `status-card` | CSS | none |
| Security / account action (password, OTP, sign-in) | `transactional-security` | `status-card` (+ `otp-action-block`) | CSS | none |
| Reservation / booking confirmation (travel, ticket) | `travel-booking` | `split` | C | user-asset (property/seat) |
| Brand-forward order confirmation (D2C / fashion) | `transactional-branded` | `baked-art` | A | generate (header band only) |

## 6 · Newsletter / media / editorial / content

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| Sectioned daily digest (multi-story, sponsor slots) | `newsletter-digest` | `color-block` | CSS | mix |
| Smart-brevity brief ('why it matters') | `newsletter-brief` | `text-over-image` | B | generate · user-asset |
| Single-essay creator letter (long-form) | `newsletter-essay` | `image-only` | C | user-asset |
| Curated link roundup (by category) | `newsletter-roundup` | `split` | C | user-asset (thumbnails) |
| Designed editorial magazine issue (art-directed) | `newsletter-magazine` | `illustrated-banner` | A | generate · user-asset |
| Welcome / subscriber onboarding | `newsletter-welcome` | `text-over-image` | B | generate |

## 7 · Travel / hospitality / events

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| Booking confirmation receipt (stay / flight) | `travel-booking` | `image-only` | C | user-asset (real property) |
| Trip itinerary / pre-arrival timeline | `travel-itinerary` | `text-over-image` | B | user-asset · generate |
| Event invite (ticketed / conference / experience) | `travel-event-invite` | `text-over-image` | A | generate · user-asset |
| Pre-event / check-in reminder (time-sensitive) | `travel-checkin` | `color-block` | CSS | none |
| Destination inspiration / promotional discovery | `travel-destination` | `image-only` | C | user-asset (real destination) |
| Post-stay / post-event follow-up (review + rebook) | `travel-poststay` | `color-block` | CSS | user-asset |

## 8 · Fintech / banking / insurance

> **Trust + regulatory genre.** First-party data only; no stock/lifestyle. Live text for all figures/rates/dates. Often add license/registration numbers + anti-phishing note in footer.

| Occasion | Default structure | Default hero | Text | Image source |
|---|---|---|---|---|
| Transactional receipt / money-moved confirmation | `transactional-invoice` | `color-block` | CSS | none |
| Security / account alert (login, OTP, frozen card) | `transactional-security` | `status-card` | CSS | none |
| Periodic statement / account summary / 'wrapped' | `fintech-statement` | `color-block` | CSS | none (CSS data-viz) |
| Product / feature announcement | `fintech-announcement` | `illustrated-banner` | A | generate (illustration) |
| Onboarding / welcome setup checklist (KYC, fund) | `fintech-onboarding` | `split` | C | generate (illustration) |
| Promotional offer / incentive (referral, APY boost) | `fintech-offer` | `baked-art` | A | generate (typographic) |
| Regulatory / policy / document notice | `fintech-regulatory` | `text-only` | CSS | none |

---

## Type detection — how to pick the row

Read the brief for these signals, in priority order (mirror `layout-patterns.md` ordering — most specific first):

1. **Transactional/system intent** — order/receipt/invoice/shipping/OTP/security/booking-confirm wording → Transactional or the genre's transactional row. *Check first.*
2. **Genre** — from `brand.industry` + product nouns: wearable/device → Hardware (before SaaS); saas/software/platform/app/api/b2b → SaaS; fashion/apparel/clothing → Ecommerce; supplement/protein/beauty/food/wellness → D2C; bank/card/APY/insurance → Fintech; newsletter/digest/issue → Newsletter; hotel/flight/trip/event → Travel.
3. **Occasion** — from `campaign.goal` + headline: sale/discount, launch/announcement, welcome/onboarding, nurture, win-back, statement, etc.

If the brief doesn't pin a row, default to the genre's most general marketing occasion, or **Newsletter → `newsletter-welcome`** / **Default Editorial** as the catch-all. When two rows are plausible, **ask** (it's axis 1 of the guided intake).

## Extending the taxonomy

A genre/occasion the table doesn't cover → after the guided intake's research fallback derives a new hero (`email-structure-researcher` agent), add: (1) the new occasion row here, (2) a named structure in `template-structures.md`, (3) the archetype in `layout-patterns.md`. Keep the three references in sync — a type always resolves to a structure and a hero.
