# Template structures — named body skeletons

> The **second axis** of the design system. A *template structure* is a named, reusable **ordered
> sequence of section blocks** (the blocks are defined in `references/layout-patterns.md` → "Section
> blocks"). Picking a named structure is what makes builds **consistent** instead of improvised.
>
> Each structure declares the **hero archetype it pairs with** (axis 3) and the block order for the
> body. The hero is always block #1 after the masthead; the structure governs everything below it.
>
> **How to use:** the emailer type (`email-types.md`) gives a *default* structure key → look it up
> here → present it + its **alternates** to the user in the guided intake (`intake-questions.md`,
> Step B). Compose the final HTML by walking the block sequence, pulling each block's build rules
> from `layout-patterns.md` and tokens from the brand kit.

## Conventions

- **Every structure** implicitly starts with **block #0 `preheader`** (hidden preview text) and ends with **`footer`**. They're omitted from the sequences below for brevity — always include them.
- `hero: <archetype> (A|B|C|CSS)` names the paired hero archetype + default hero-text strategy.
- Blocks in `(parens)` are **optional** — include when the brief warrants.
- `[a | b]` means "pick one"; `×N` means the block repeats.
- Keep to **one primary CTA** per email (exception: `*-comparison` / split-screen, which run two co-equal CTAs by design).

---

## 1 · Ecommerce / fashion / apparel

**`ecom-collection-drop`** · hero: `image-only` (C)
`logo-bar → hero → headline-cta → category-mosaic → (lifestyle-band) → (testimonial) → footer`
Brand-moment lookbook. Big edge-to-edge photography; let the imagery carry it. *Alternates:* `split`, `text-over-image`.

**`ecom-sale-standard`** · hero: `text-over-image` (A)
`announcement-bar → logo-bar → hero → headline-cta → product-grid → urgency-strip → (reassurance-bar) → footer`
The workhorse promo. Sale headline baked on hero; product grid with struck/sale prices; deadline near CTA. *Alternates:* `color-block`, `product-cutout`.

**`ecom-editorial-edit`** · hero: `split` (C)
`logo-bar → hero → (section-divider-label) → feature-split-block ×2–4 → (cross-sell-strip) → footer`
'The Edit' / gift guide. Alternating image-left/right story rows. *Alternates:* `image-only`.

**`ecom-product-spotlight`** · hero: `image-only` (C)
`logo-bar → hero → headline-cta → detail-stack → (testimonial) → (urgency-strip) → footer`
Single SKU / restock. Macro detail crops justify the buy. *Alternates:* `product-cutout`, `split`.

**`ecom-flash-promo`** · hero: `color-block` (CSS)
`(announcement-bar) → logo-bar → hero → (product-cutout grid) → urgency-strip → footer`
Asset-light, fastest to build, bulletproof. Type-forward offer on brand color. *Alternates:* `product-cutout`.

**`ecom-category-mosaic`** · hero: `product-cutout` (C)
`logo-bar → hero → category-mosaic → (urgency-strip) → footer`
Multi-department 'New In'. Uniform cutout tiles, identical crops. *Alternates:* `color-block`, `image-only`.

## 2 · D2C / CPG / supplement / beauty / food

**`d2c-lifestyle`** · hero: `image-only` (C)
`logo-bar → hero → headline-cta → benefit-stack → (testimonial) → footer`
Brand-moment newsletter; product-in-context. *Alternates:* `split`, `text-only`.

**`d2c-launch`** · hero: `product-cutout` (C)
`(announcement-bar) → logo-bar → hero → headline-cta → benefit-stack → testimonial → (subscribe-upsell-panel) → footer`
New SKU/flavor. Cutout-on-tile hero; benefits + social proof drive trial. *Alternates:* `color-block`, `baked-art`.

**`d2c-science`** · hero: `split` (C)
`logo-bar → hero → benefit-stack → (stats-row) → (comparison-table) → headline-cta → footer`
Ingredient / 'how it works'. Scannable education stack. *Alternates:* `image-only`, `text-only`.

**`d2c-replenish`** · hero: `product-cutout` (C)
`logo-bar → hero → headline-cta → (reassurance-bar) → subscribe-upsell-panel → footer`
Reorder reminder. Single clear reorder CTA; subscribe upsell secondary. *Alternates:* `text-only`, `color-block`.

**`d2c-restock`** · hero: `color-block` (CSS)
`logo-bar → hero → (product-cutout) → headline-cta → footer`
Back-in-stock / waitlist. Minimal, type-forward. *Alternates:* `text-only`, `image-only`.

**`d2c-playful-sale`** · hero: `baked-art` (A)
`(announcement-bar) → logo-bar → hero → headline-cta → product-grid → ugc-collage → urgency-strip → footer`
Challenger-brand energy; baked typographic poster hero + UGC. *Alternates:* `product-cutout`, `color-block`.

## 3 · SaaS / B2B tech

**`saas-launch`** · hero: `illustrated-banner` (A*)
`logo-bar → hero → headline-cta → feature-split-block ×2–3 → (stats-row) → testimonial → headline-cta(repeat) → footer`
Flagship launch. Illustrated banner (tagline restated as live H1); alternating feature rows. **Never photos of people.** *Alternates:* `color-block`, `split`.

**`saas-changelog`** · hero: `color-block` (CSS)
`logo-bar → hero → feature-trio → (feature-split-block) → quick-hits → footer`
'What's new' roundup. Masthead color block + featured items + long-tail quick-hits. *Alternates:* `split`.

**`saas-feature`** · hero: `text-over-image` (B)
`logo-bar → hero → headline-cta → feature-trio → (testimonial) → footer`
Single-feature spotlight. UI screenshot hero with live overlay. *Alternates:* `image-only`, `split`.

**`saas-founder-note`** · hero: `text-only` (CSS)
`hero(personal greeting + 2–4 paragraphs) → (inline link/quiet button) → sign-off → footer`
Onboarding / nurture / founder note. Looks like a 1:1 email — no banner, no cards. *Alternates:* `split`.

**`saas-announcement`** · hero: `color-block` (CSS)
`logo-bar → hero → headline-cta → (stats-row) → footer`
Funding / pricing / event / waitlist. *Alternates:* `baked-art`, `illustrated-banner`.

**`saas-onboarding-checklist`** · hero: `split` (C)
`logo-bar → hero → setup-checklist → (whitelist-tip) → headline-cta → footer`
Activation. Numbered finishable steps + one 'Finish setup' CTA. *Alternates:* `text-only`.

## 4 · Consumer hardware / wearables / electronics

**`hardware-reveal`** · hero: `image-only` (C)
`logo-bar(light-on-dark) → hero → headline-cta → stats-row → detail-stack → headline-cta(repeat) → footer`
'It's here'. Floating edge-to-edge render; specs + macro details. Dark-native common. *Alternates:* `text-over-image`, `color-block`.

**`hardware-spec`** · hero: `text-over-image` (A)
`logo-bar → hero → stats-row → feature-split-block ×2 → (comparison-table) → headline-cta → footer`
'Meet the technology'. Rim-lit device-on-black; spec storytelling. *Alternates:* `split`, `image-only`.

**`hardware-preorder`** · hero: `split` (C)
`logo-bar → hero → headline-cta → stats-row → (countdown-meta) → urgency-strip → footer`
Reservation drive. Value-prop column + render; scarcity + ship date. *Alternates:* `color-block`, `image-only`.

**`hardware-lifestyle`** · hero: `image-only` (C)
`logo-bar → hero → headline-cta → benefit-stack → (lifestyle-band) → footer`
Aspirational launch; product-in-use. *Alternates:* `text-over-image`, `split`.

**`hardware-comparison`** · hero: `split` (C)
`logo-bar → hero → comparison-table → stats-row → headline-cta → footer`
Gen-over-gen upgrade. Two matched renders on identical backgrounds. *Alternates:* `baked-art`, `image-only`.

**`hardware-teaser`** · hero: `color-block` (CSS)
`logo-bar → hero → countdown-meta → footer`
Pre-launch teaser/countdown. Minimal, mysterious. *Alternates:* `baked-art`, `image-only`.

## 5 · Transactional (commerce, account, booking)

> CSS-first. No marketing imagery. Facts/codes/totals = live selectable text. Unsubscribe limited/omitted for purely transactional mail.

**`transactional-receipt`** · hero: `status-card` (CSS)
`hero(status + order#/date) → line-item-table → order-summary → info-grid → (cross-sell-strip) → support-strip → footer`
Order confirmation. Cross-sell strictly below details, behind a divider. *Alternates:* `color-block`, `baked-art`.

**`transactional-shipping`** · hero: `status-card` (CSS)
`hero(status) → progress-stepper → line-item-table(condensed) → info-grid(carrier+tracking) → support-strip → footer`
Shipping/tracking. Stepper is CSS so it survives images-off. *Alternates:* `illustrated-banner`.

**`transactional-invoice`** · hero: `status-card` (CSS)
`hero(amount + status) → line-item-table → order-summary → info-grid → policy-strip → footer`
Receipt/invoice/money-moved. Total is the visual anchor. *Alternates:* `color-block`, `text-only`.

**`transactional-security`** · hero: `status-card` (CSS)
`hero(what happened) → otp-action-block → (info-grid: device/location/time) → policy-strip → support-strip → footer`
OTP / sign-in alert / password. Code is large, monospaced, selectable; severity by word+icon, not color alone. *Alternates:* `text-only`.

**`transactional-branded`** · hero: `baked-art` (A)
`logo-bar → hero(brand header band) → line-item-table → order-summary → info-grid → cross-sell-strip → footer`
D2C/fashion order confirm with a brand moment — but the receipt facts stay live text. *Alternates:* `status-card`, `color-block`.

## 6 · Newsletter / media / editorial / content

**`newsletter-digest`** · hero: `color-block` (CSS)
`logo-bar → hero(masthead) → editor-note → [section-divider-label → link-roundup-item ×N] ×sections → quick-hits → (referral-share) → footer`
Multi-story digest with sponsor slots. Section labels are the wayfinding. *Alternates:* `text-over-image`, `split`.

**`newsletter-brief`** · hero: `text-over-image` (B)
`logo-bar → hero → editor-note → section-divider-label → link-roundup-item ×N → footer`
Smart-brevity 'why it matters'. *Alternates:* `image-only`, `color-block`.

**`newsletter-essay`** · hero: `image-only` (C)
`logo-bar → hero → editor-note(long-form body) → (cross-sell-strip) → footer`
Single-essay creator letter. *Alternates:* `text-only`.

**`newsletter-roundup`** · hero: `split` (C)
`logo-bar → hero → [section-divider-label → link-roundup-item ×N] → quick-hits → footer`
Curated links by category. *Alternates:* `text-only`, `color-block`.

**`newsletter-magazine`** · hero: `illustrated-banner` (A)
`logo-bar → hero → countdown-meta(issue/date) → feature-split-block ×N → section-divider-label → footer`
Art-directed magazine issue. *Alternates:* `split`, `color-block`.

**`newsletter-welcome`** · hero: `text-over-image` (B)
`logo-bar → hero → editor-note(welcome) → setup-checklist(or benefit-stack) → whitelist-tip → headline-cta → footer`
Subscriber onboarding. Include the deliverability/whitelist tip. *Alternates:* `color-block`, `text-only`.

## 7 · Travel / hospitality / events

**`travel-booking`** · hero: `image-only` (C)
`logo-bar → hero(real property) → itinerary-card → info-grid → policy-strip → map-directions → support-strip → footer`
Booking confirmation. Real first-party property photo; itinerary as structured live text. *Alternates:* `status-card`, `split`.

**`travel-itinerary`** · hero: `text-over-image` (B)
`logo-bar → hero → itinerary-card → progress-stepper(trip timeline) → policy-strip → support-strip → footer`
Pre-arrival timeline. *Alternates:* `illustrated-banner`, `split`.

**`travel-event-invite`** · hero: `text-over-image` (A)
`logo-bar → hero → headline-cta → info-grid(date/venue) → (countdown-meta) → map-directions → footer`
Ticketed event invite. *Alternates:* `baked-art`, `split`.

**`travel-checkin`** · hero: `color-block` (CSS)
`hero(reminder) → info-grid → map-directions → policy-strip → support-strip → footer`
Time-sensitive check-in reminder. *Alternates:* `status-card`.

**`travel-destination`** · hero: `image-only` (C)
`logo-bar → hero → headline-cta → category-mosaic(destinations) → footer`
Destination inspiration. Real destination photography. *Alternates:* `text-over-image`, `product-cutout`.

**`travel-poststay`** · hero: `color-block` (CSS)
`logo-bar → hero → headline-cta(review) → cross-sell-strip(rebook) → support-strip → footer`
Post-stay review + rebook. *Alternates:* `text-over-image`, `image-only`.

## 8 · Fintech / banking / insurance

> First-party data only. Live text for all figures/rates/dates. Footer adds license/registration + anti-phishing note.

**`fintech-statement`** · hero: `color-block` (CSS)
`logo-bar → hero(period + balance) → stats-row → data-viz → info-grid → policy-strip → footer`
Statement / 'wrapped'. CSS bars / labeled charts; figures right-aligned. *Alternates:* `baked-art`, `stats-row`.

**`fintech-announcement`** · hero: `illustrated-banner` (A)
`logo-bar → hero → headline-cta → feature-trio → (comparison-table) → policy-strip → footer`
Feature announcement. Illustration, not photography. *Alternates:* `color-block`, `split`.

**`fintech-onboarding`** · hero: `split` (C)
`logo-bar → hero → setup-checklist(KYC/fund/activate) → reassurance-bar(deposit insurance) → headline-cta → footer`
Onboarding checklist. *Alternates:* `text-only`, `illustrated-banner`.

**`fintech-offer`** · hero: `baked-art` (A)
`logo-bar → hero(offer figure) → headline-cta → policy-strip(terms) → footer`
Referral / APY boost. Offer value + terms + CTA always live text below the baked numeral. *Alternates:* `color-block`.

**`fintech-regulatory`** · hero: `text-only` (CSS)
`hero(notice) → policy-strip(what's changing / what to do) → support-strip → footer`
Regulatory / policy notice. Plain, document-grade. *Alternates:* `color-block`.

---

## Catch-all

**`default-editorial`** · hero: `image-only` (C)
`logo-bar → hero → headline-cta → editor-note(2–3 paragraphs) → headline-cta → footer`
Use when no type row fits. Clean editorial photo, no text overlay, live HTML headline below.

## Composing & extending

- **Walk the sequence top-to-bottom**, instantiating each block per `layout-patterns.md` build rules, applying the universal baseline (SKILL Step 3) and brand kit.
- **Journeys:** vary the structure *and* hero across emails — never reuse the same skeleton + hero headline back-to-back (SKILL Step 2.5 cross-email check).
- **New structure needed?** Derive it during the guided intake (via `email-structure-researcher` for the hero), then add the named structure here, its row in `email-types.md`, and the archetype/blocks in `layout-patterns.md`. Keep all three in sync.
