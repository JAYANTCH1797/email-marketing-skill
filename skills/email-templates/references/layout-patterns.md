# Email layout patterns — archetype library

> Derived from a multi-genre research pass. This is the **pattern language** the blueprint (email-templates Step 2.5) selects from. Loaded on demand — read it when choosing a hero/structure.

**How to use:** in the blueprint, (1) look up the genre+use-case in *Genre defaults* for the starting archetype, (2) confirm/override with the user from the *Hero archetypes* menu, (3) compose the body from *Section blocks*, (4) apply *Build & image guardrails* to everything. Archetypes are ordered **most-reliable → highest-risk** — default to the safe one; reach for VML-dependent overlays only when the genre needs atmosphere.

## Hero archetypes (the menu)

### `image-only` — Image-Only Hero (caption-below)
- **Best for:** Fashion collection drops, D2C/CPG lifestyle newsletters, hardware product reveals, aspirational lifestyle launches, single-product spotlights, single-essay creator letters, travel destination heroes.
- **How built:** Full-container-width (600px) edge-to-edge brand or generated photograph with ZERO text baked in; headline, subhead and CTA live as live HTML text in a solid-background block directly BELOW (or above) the image, never on it. Build as a single <img> inside a 100%-width table cell, padding:0. Always set width/height attributes + display:block + a retina-ready 2x source. Supply a mobile-safe focal point (CSS object-position or art-directed crop) so faces/product aren't cropped out on narrow screens.
- **Image strategy:** Hero-quality art-directed lifestyle or studio photography used directly; for asset-less brands, generate ONE cinematic in-context image per section rather than many small ones. Hardware: photoreal studio render on a seamless backdrop whose color matches the email body so the device floats edge-to-edge.
- **Readability:** Bulletproof — because no copy sits on the photo, legibility is independent of image brightness, dark mode, or retina scaling. Body copy is always live HTML on solid/off-white background.
- **Risks:** If images are blocked the email looks empty: requires descriptive alt text and a bgcolor placeholder on the image cell. Never the right choice when a CTA or critical fact must survive image-blocking (that belongs in the text block, which it always is here).

### `text-over-image` — Text-Over-Image Hero (baked scrim + safe zone)
- **Best for:** Seasonal sale heros, SaaS single-feature spotlights, hardware dark-mode spec showcases, event invites, smart-brevity newsletter leads, newsletter welcome, travel itinerary/destination banners.
- **How built:** Headline/offer overlaid on a photo. CRITICAL email-client technique: the background image is set on a table cell via BOTH css background plus a VML <v:rect>/<v:fill> fallback so Outlook (which ignores CSS background-image) still renders it; live HTML text sits on top via VML <v:textbox> or an absolutely-overlaid table. Contrast is guaranteed by baking a darkened scrim/gradient INTO the image asset itself (not relying on a CSS overlay Outlook may drop) and confining text to a pre-cleared safe zone (sky/wall/negative space). Add text-shadow as belt-and-suspenders. Keep overlay copy to 2-6 words.
- **Image strategy:** One promotional lifestyle/flat-lay (sale) or rim-lit device-on-black (spec) or destination shot (travel). The scrim band and safe zone are designed into the asset at export time so contrast passes regardless of client.
- **Readability:** Passes only because the scrim is baked into the JPEG and text sits in the cleared safe zone; AA-checked against the darkest region the text can touch. Never place text over busy/reflective image areas.
- **Risks:** Highest-risk archetype for Outlook (no CSS bg-image) and image-blocking. MUST mirror the headline as live HTML text below as well for blocked-image clients; MUST NOT be the sole carrier of the CTA — the CTA is always a separate live HTML button beneath the image.

### `split` — Split Hero (image + text columns)
- **Best for:** Editorial 'The Edit', ingredient/science explainers, hardware pre-order & comparison, SaaS onboarding checklists & launches, fintech onboarding, curated link roundups, booking/itinerary cards.
- **How built:** Two-column table: image on one side, headline+subhead+CTA stack on the other (editorial fashion, onboarding step, pre-order value-prop, comparison, boarding-pass card). Build with role=presentation tables and align/width attributes; each column ~50%. Stacks to a single column on mobile via fluid/hybrid technique (max-width + align=left ghost tables, or media-query stacking). Text column is solid-background live HTML so legibility is column-independent of the photo.
- **Image strategy:** Product cutout or UI screenshot or two-up device comparison on one side; copy on the other. For comparison, two matched renders on identical backgrounds. Alternate image-left/image-right across stacked blocks to build editorial rhythm.
- **Readability:** Strong — headline legibility is fully decoupled from the photo because text owns its own solid column. Mobile stacking keeps image-then-text reading order.
- **Risks:** Mobile stack order must be controlled (image-first vs text-first) or the reading logic breaks; Outlook needs ghost-table column widths. Don't let a tall image push the text column's CTA far below the fold on mobile.

### `color-block` — Color-Block / Type-Forward Hero
- **Best for:** Asset-light flash promos, minimalist beauty/restock, SaaS changelog mastheads & gradient announcements, fintech receipts/statements/alerts, hardware teasers, daily-digest newsletter mastheads, travel check-in reminders, post-stay headers.
- **How built:** Flat brand-color (or subtle CSS gradient) field set as a table-cell bgcolor with an oversized live HTML headline + single CTA; optional transparent-PNG product cutouts floated on the block. For gradients, set a solid bgcolor fallback first (Outlook flattens gradients) and AA-check headline color against the darkest stop. The status-band variant (transactional/fintech) puts a status word + big number/amount on the block. Cheapest, most bulletproof hero — all contrast is controlled because text sits on solid color, not a photo.
- **Image strategy:** Imagery minimal-to-none — the flat color field IS the design. Any products appear as transparent-PNG cutouts (with soft contact shadow) floated on the block so no photo background competes.
- **Readability:** Best-in-class and dark-mode-safe: pick one accessible fg/bg pair and it always passes; keep to ~2 type sizes. Live web-safe/system type renders crisply everywhere.
- **Risks:** Can read as plain/cheap if overused for premium brands; gradient must have a solid Outlook fallback; sample the block color from the logo so it never clashes with the palette.

### `product-cutout` — Product-Cutout on Color/Tile Hero
- **Best for:** D2C/CPG product launches & hero-SKU promos, replenishment reminders, fashion sale grids & category mosaics, travel listing/destination grids (photo-on-top, facts-beneath variant).
- **How built:** Transparent-PNG product (bottle, pouch, jar, garment) composited onto a flat brand-color or lightly-gradient field, with a soft baked contact shadow so it grounds rather than looks pasted-on. Live HTML headline/price/CTA sit in a cleared safe zone on the block or immediately below. For grids/mosaics, reuse smaller cutouts on uniform tinted tiles, each with name+price as live text beneath. Any overlay text avoids the busy/detailed part of the product.
- **Image strategy:** Brand-asset PNG cutouts preferred; if generating, render product in clean studio light with a soft contact shadow. Keep every grid cell identical in crop, padding and background; never mix on-white and on-model thumbnails in one grid.
- **Readability:** High — cutout sits on solid color so any overlaid type has controlled contrast; grid/price text is always live HTML below the image, never baked.
- **Risks:** Low-res cutouts with rough edges or leftover white halos look broken; don't put name/price over a busy cutout without a solid tile behind it. Keep to ≤2 background colors per hero.

### `illustrated-banner` — Illustrated / Baked-Art Banner Hero
- **Best for:** SaaS major launches, fintech feature announcements, designed editorial-magazine mastheads, playful CPG collage heros, shipping/tracking progress steppers.
- **How built:** Designed full-width banner — vector illustration, geometric/gradient artwork, or composed sticker-collage — exported as one image (jpg/png). For launches the tagline is baked into the artwork as vector type at export for pixel-perfect brand-font kerning; the same value is ALWAYS restated as a live HTML H1 below the banner for accessibility, dark mode and blocked images. Feature/UI screenshots sit in their own tinted cards below, never under body text. Status-stepper variant (shipping) is built CSS-first (icon dots + connecting line) so it survives images-off.
- **Image strategy:** Brand illustration system or product-UI render first; NO stocky lifestyle photos of people at laptops (breaks digital-native genre code). Feature blocks use real cropped UI on subtle tinted cards.
- **Readability:** Good when the live-text restatement exists; the banner is decorative/brand-moment and the H1 below carries the accessible message.
- **Risks:** Baked tagline is invisible to screen readers and blocked-image clients and won't reflow — so the live-HTML restatement is mandatory. Keep illustration style consistent across items in roundups.

### `baked-art` — Baked-Art Typographic Hero (offer/teaser)
- **Best for:** Fintech promotional offers/incentives, hardware pre-launch teasers, playful CPG sale heros, brand-forward D2C order-confirmation header bands.
- **How built:** A single designed graphic where oversized typography or an offer figure IS the composition — '$50', '4.5% APY', a teaser silhouette, or a playful sale poster — on a bold brand-color field, exported as one image. Because key value can be trapped in the image, the offer value, terms and CTA are ALWAYS live HTML beneath it and the image carries descriptive alt text. Often paired with a solid bgcolor cell behind it as the blocked-image fallback.
- **Image strategy:** Typography/illustration-led, not photographic: big numerals, brand color field, optional coin/card/gift spot illustration. The most visually expressive option in restrained genres — used sparingly.
- **Readability:** Acceptable only with the mandatory live-text fallback; the baked numerals are for impact, the live text is the accessible source of truth.
- **Risks:** Never trap the offer value or CTA solely in the image (blocked images + a11y); never bake live-changing data (dates, prices, rates) since they go stale and break in dark mode. Keep brand-aligned so it doesn't read as a third-party scam.

### `text-only` — Plain-Text / Personal-Note Hero
- **Best for:** SaaS founder/onboarding notes & beta invites, minimalist beauty notes, fintech regulatory/policy notices, document-plain receipts, text-first creator letters.
- **How built:** Intentionally image-free (or one tiny inline wordmark): personal greeting, 2-4 short left-aligned paragraphs in system/sans font, ONE inline text link or quiet button, human sign-off. No header banner, no cards, no background colors — single-column full-width text measure that mimics a normal 1:1 email. Renders identically everywhere and tends to land in the primary tab.
- **Image strategy:** None by design — the whole point is that it looks like it came from a person, not a design tool.
- **Readability:** Perfect and unbreakable: dark text on white at comfortable size; nothing breaks in dark mode or with images off; no contrast risk at all.
- **Risks:** Adding any designed header instantly breaks the 'personal email' illusion; resist stacking multiple CTAs. Wrong choice when the brand needs a visual moment.

### `status-card` — Status / System-Notice Card Hero
- **Best for:** All transactional & system mail: order confirmations, shipping facts, SaaS/payment receipts, invoices, security/OTP/sign-in alerts, booking detail cards, fintech money-moved receipts and account alerts.
- **How built:** CSS-only, no photography: small centered logo, optionally ONE monochrome status glyph (check/lock/shield/box/key), a literal H1 stating what happened, and a real HTML <table> facts block (amount/date/order#/device/itinerary) with high label-value contrast. OTP codes and money figures are large, letter-spaced, selectable LIVE TEXT — never images. Severity shown by a flat color border/chip PLUS a word/icon, never color alone. Single primary CTA button. Renders perfectly in Outlook and dark mode because it's table-and-type, not background images.
- **Image strategy:** Essentially imageless — small logo + at most one status icon + product thumbnails (square ~64-80px, real catalog images) in line-item tables. No hero photo, no marketing imagery (it erodes trust and trips spam filters).
- **Readability:** Maximal: single-column, generous padding, the critical fact (amount/order#/status) is the largest element and legible even in notification preview. Real table cells keep figures crisp in Outlook.
- **Risks:** Almost none technically; the failure mode is dressing it up with marketing heros/cross-sell that breaks the 'this is a receipt' trust signal. Never render key numbers/codes as images; never convey status by color alone.

## Extended archetypes (specialized — motion / interactive / location)

Surfaced by the completeness review. Use when the genre demands it; each is **progressive enhancement over a working static fallback**.

### animated-hero (GIF / APNG looped hero)
- **Why:** GIF is permitted in the image guardrails but has no archetype, so the library gives zero build guidance for the single most common motion treatment in fashion, D2C, gaming, and SaaS product-demo email. It is NOT just 'image-only that moves' — it has distinct, load-bearing rules the static archetypes don't state.
- **Best for / build:** Fashion/D2C product reveals and 'see it move' moments, SaaS feature micro-demos (UI interaction loop), gaming/streaming launch hype, food (steam/pour loops), replenishment reminders. Build rule: design frame 1 as the complete static 'money frame' (Outlook/Outlook.com + Apple Mail Low-Power freeze on it), ≤6-8 frames / sub-1MB, no critical copy or CTA visible only mid-loop, live-HTML headline+CTA still below the hero (same caption-below spine), and the loop must read fine if it never animates.

### interactive-hero (AMP / CSS-only, progressive enhancement)
- **Why:** No coverage of AMP for Email or CSS-hack interactivity (carousel, hover image-swap, accordion, tap-to-reveal, in-email add-to-cart, live poll). These are real in production (Gmail/Yahoo AMP; Nike/Booking/Pinterest kinetic). Without an archetype, the library's 'default to safe static' bias has no upgrade path and a builder won't know interactivity must be layered over a working static fallback.
- **Best for / build:** Gmail/Yahoo/Mail.ru audiences: product carousels that swipe in-inbox, FAQ accordions, live inventory/price, one-click NPS/poll, RSVP, add-to-cart. Hard rule: ALWAYS progressive enhancement — the AMP MIME part degrades to the HTML part, and any :checked/:hover trick must fully degrade to a static, complete layout in Outlook and most mobile clients. Never the sole carrier of the message or CTA.

### video-thumbnail hero (linked-out play affordance)
- **Why:** Video is a distinct, common reveal treatment and the ONLY safe pattern is a poster image with a baked play button that links to a hosted video (true inline <video> works only in Apple Mail). The current set would misroute this to 'image-only' and lose the play affordance + linked-out + animated-GIF-preview-of-the-video nuance.
- **Best for / build:** Hardware/product reveals, course/webinar promos, creator/media trailers, testimonial video. Build: static poster with a high-contrast play triangle (baked or live overlay), the whole hero links to the hosted video page, optionally an animated-GIF micro-preview as the poster; never rely on inline HTML5 video; headline+CTA live-HTML below per the caption spine.

### full-bleed-color dark-native hero (dark-mode-first)
- **Why:** The library treats dark mode as a defensive constraint on light layouts, but a whole class of brands (gaming, premium hardware, crypto/web3, nightlife/events, dev-tools) ships DARK-NATIVE emails by design. This is distinct from color-block (which is brand-color-on-light) and needs its own rules so it isn't accidentally inverted by Gmail/Outlook.com forced dark mode into an unreadable mess.
- **Best for / build:** Gaming/streaming, premium consumer hardware, dev-tools/crypto, nightlife and ticketed events. Build: design dark end-to-end (never invert a light layout), declare color-scheme meta + supported-color-schemes, use near-black not #000, light (not gray) text on dark, transparent PNG logos that survive on dark AND on a forced-light flip, and avoid the forced-inversion trap by testing both schemes.

### map-led / location-first hero
- **Why:** For local/marketplace/real-estate/store-event/last-mile-delivery email, the dominant top moment is a place, not a product or a status — a static map tile (linked out) with the address/CTA as live text. The library has a 'map-directions' SECTION block but no hero treatment, so a 'your store is open' / 'new listing near you' / 'driver is 2 stops away' email has no genre-appropriate hero.
- **Best for / build:** Real estate/marketplace listings, local-store events and grand openings, last-mile delivery ('arriving soon' with live-ish map), restaurant/booking reminders, in-person event venue reveals. Build: static Google/Mapbox tile as a linked image (never heavy embedded interactive map), pin + address + 'Get directions'/'View listing' as live text, real first-party location data, image-off fallback shows the address text.

### split-screen / mirrored A-B hero (two equal stories)
- **Why:** 'split' covers image+text. It does NOT cover two equal-weight competing messages side-by-side — Men/Women, Plan A/Plan B pricing, This-or-That, two product lines, his/hers gifting. This is a recurring fashion/retail/pricing pattern with its own stacking-order and 'two CTAs of equal weight' rules (which otherwise violate the one-CTA default and need explicit handling).
- **Best for / build:** Fashion gender/category splits, two-tier pricing or plan comparison heroes, gift-guide 'for him / for her', this-or-that product votes, two simultaneous collection drops. Build: two 50/50 cells each with its own image+label+CTA, controlled mobile stack order, and an explicit exception to the single-CTA rule (two co-equal CTAs are the point) — distinct from the editorial split's single primary CTA.

## Section blocks (compose the body from these)

> **Block #0 — preheader / preview-text:** a hidden span right after `<body>` controlling the inbox snippet, plus a whitespace spacer so footer text doesn't leak in. Always include it.

- **Announcement / Utility Bar** (`announcement-bar`) — Slim full-width top strip for an offer line ('UP TO 50% OFF — ENDS SUNDAY'), free-shipping note, points/loyalty balance, or 'Presented by' sponsor line. Live text on a solid band; sits above the logo.
- **Logo / Masthead Bar** (`logo-bar`) — Brand wordmark (centered for premium, left for utility), optionally with minimal text nav or a status icon. Tiny + lots of whitespace signals luxury; can invert to sit on a colored hero.
- **Hero Block** (`hero`) — The dominant top visual/message moment — instantiated as one of the nine hero archetypes. Carries the campaign statement, offer, product reveal, status, or personal greeting and the primary CTA (or hands it to the block immediately below).
- **Headline + Subhead + Primary CTA** (`headline-cta`) — Live-HTML value statement (H1 + 1-2 line subhead) plus the single high-contrast primary CTA button, placed on a solid background below an image hero. The reliability backbone for image-only/text-over-image patterns — keeps the message and action legible regardless of image state.
- **Product Grid** (`product-grid`) — 2-3 column matrix of product cells (image + name + struck/sale price), all identical crop/padding/background. For sales, best-sellers, 'most-loved'. Reflows to one column on mobile; each cell self-contained with its own price as live text.
- **Category / Department Mosaic** (`category-mosaic`) — 2x2 / 2x3 equal-weight tiles (image + label + 'Shop') for multi-department 'New In'. Labels in a consistent caption bar or solid corner chip, never free-floating over busy imagery. Cap at ~6 tiles.
- **Feature Trio / Value-Prop Icon Row** (`feature-trio`) — 3-across (sometimes 3-4) icon-or-crop + short label + one line each. Used for hardware feature trios, D2C value props (clinically-backed / no junk / free shipping), and 'how it works' steps. Stacks on mobile; icons carry alt text.
- **Alternating Feature / Story Split** (`feature-split-block`) — Repeating left/right image-text rows (UI screenshot + label, or editorial photo + standfirst, or step icon + action link) that alternate sides for rhythm. The core body module for SaaS launches, editorial edits, onboarding checklists, and hardware zig-zag spec rows.
- **Benefit / Ingredient Education Stack** (`benefit-stack`) — Vertically stacked [icon or ingredient image] + [bold benefit label] + [1-2 line explanation] rows, 3-6 deep. The scannable workhorse for supplement/CPG science explainers and SaaS 'what it does' — all live text on solid background.
- **Product Detail Stack (mini-PDP)** (`detail-stack`) — Vertical stack of macro close-up crops (texture, hardware, fit, materials), each paired with one short benefit caption beneath. Justifies price/craft for single-product fashion & hardware spotlights; eye moves image→caption down one column (mobile-perfect).
- **Testimonial / Social-Proof Strip** (`testimonial`) — Star rating + short review quote, founder/expert quote with portrait, 'as seen in' press-logo row, or customer-logo wall. Builds credibility for D2C, SaaS announcements, and review-request emails.
- **UGC / Review Collage** (`ugc-collage`) — Grid of customer photos (or prior-event photos for FOMO) used by playful CPG community roundups and event invites. Solid-bounded tiles; energy contained so surrounding live-text blocks stay calm.
- **Lifestyle / Editorial Band** (`lifestyle-band`) — Secondary full-width in-context photo (styling shot, product-in-use, 'here's the actual device' render mid-email) that re-establishes mood between text blocks. Decorative — text stays off it or sits in its own caption block below.
- **Stats / Big-Number Row** (`stats-row`) — Oversized numerals as visual anchors — hardware specs (battery hrs, accuracy %, sample rate), credibility numbers ('75+ ingredients', '$X raised'), or fintech KPI cards (balance, net change) with up/down deltas. Carries meaning even with images off; pair color with arrows/signs.
- **Comparison / Spec Table** (`comparison-table`) — Live-HTML rows/cells (feature | previous gen | new gen, or old | new fee/term) with checkmarks, 'NEW' badges and numeric deltas. Built as real table structure (never a flattened image) so it reflows on mobile and stays accessible. Used for hardware upgrades and fintech policy changes.
- **Data Visualization Block** (`data-viz`) — Bar/donut/line chart for fintech statements, newsletter briefs, and 'wrapped' recaps. Built as bulletproof CSS bars or a baked static image with alt text; 2-3 colors, labeled directly (no legend reliance), figures right-aligned where tabular.
- **Line-Item / Itemized Table** (`line-item-table`) — Order/charge rows: thumbnail + name/variant/qty (left/middle) + price (right, right-aligned). Real HTML table with hairline dividers and generous row padding. The functional core of confirmations, receipts and invoices; condensed (no prices) for shipment emails.
- **Order / Price Summary Block** (`order-summary`) — Subtotal, shipping, tax, discount, grand total as label/right-amount rows with the total bold as the visual anchor. Tabular-lining numerals; document-grade dark-on-light contrast even inside a colored brand palette.
- **Two-Column Info / Facts Grid** (`info-grid`) — Compact label-value pairs: shipping + billing address, check-in | check-out dates, confirmation #, guest/seat/room, carrier + tracking number (monospaced). Recedes behind the totals/itinerary; the reference data people copy or screenshot.
- **Itinerary / Ticket / Boarding-Pass Card** (`itinerary-card`) — Bordered high-contrast card rendering origin→destination, dates/times (with timezone), party/seat/room as structured live text so it reads like a ticket and survives images-off. Pairs a QR/barcode or Wallet button with the confirmation code as text.
- **Status / Progress Stepper** (`progress-stepper`) — Horizontal Ordered→Packed→Shipped→Out-for-delivery→Delivered (or transfer Initiated→Converting→Sent→Arrived) with the active stage emphasized. CSS/icon-built so it degrades gracefully with images off; node labels in small caps.
- **OTP / Verification Action Block** (`otp-action-block`) — Large monospaced, letter-spaced, SELECTABLE code in a bordered box, OR a 'Yes this was me / Secure my account' button pair, plus expiry note and a plain-text URL fallback under any button. The single dominant element in security mail.
- **Onboarding / Setup Checklist** (`setup-checklist`) — 2-4 numbered/checkbox step rows (icon + label + inline action link) giving a sense of finishable progress, with one dominant 'Finish setup' CTA. Mirrors the in-app checklist; used by SaaS and fintech activation (verify / fund / activate).
- **Issue / Trip / Event Meta + Countdown** (`countdown-meta`) — Date/edition line, issue number, trip dates, or 'Your trip starts in 3 days' / 'ends Sunday' countdown. Live text (never a baked countdown graphic for live urgency — pair any graphic with text).
- **Section Divider / Category Label** (`section-divider-label`) — ALL-CAPS colored category heading (MARKETS, READS, NEW IN) or a hairline rule that chunks multi-story digests, link roundups and edits. CSS color blocks + type, not images; the primary wayfinding device in newsletters.
- **Editor / Founder Note** (`editor-note`) — Voice-forward signed intro monologue (digest 'Good morning', creator letter, welcome note, founder launch). Conversational live text that sets brand personality before the news/offer.
- **Curated Link Item** (`link-roundup-item`) — Bold linked title + 1-2 line editor annotation, optional small left thumbnail (split). Grouped under category labels for skimming; the link title is the boldest scannable element. The repeating unit of recommendation newsletters.
- **Quick-Hits / 'Also Shipped' List** (`quick-hits`) — Bulleted one-liner links — 'Grab Bag', 'Catch up quick', or the plain 'smaller improvements' list after the featured changelog items. Text-only, dense, no images; absorbs the long tail so featured items stay focused.
- **Urgency / Scarcity Strip** (`urgency-strip`) — 'While stocks last', ship-date/allocation ribbon ('Pre-order · Ships March'), or live countdown reminder. Live text for anything that changes; reinforces deadline near the CTA. Avoided entirely in transactional/regulatory mail.
- **Reassurance / Trust Bar** (`reassurance-bar`) — Free-shipping/easy-returns icons, warranty/what's-in-the-box, regulator/deposit-insurance (FDIC/FSCS)/encryption badges, 'data carries over' for upgrades. Small, grey, beneath the action; establishes credibility at the conversion or onboarding moment.
- **Subscribe-and-Save / Upgrade Panel** (`subscribe-upsell-panel`) — Visually distinct bordered/tinted panel with a % off + perks bullets, kept SECONDARY to the primary reorder/setup CTA. Used by replenishment reminders and free→paid creator/SaaS nudges so the upsell reads as a separate actionable block.
- **Cross-Sell / 'You Might Also Like' Strip** (`cross-sell-strip`) — Recommendation or rebook tiles (name + price + rating as live text below image) placed strictly BELOW the order/booking details behind a divider and a clear label, so it reads as 'extra', never as the transaction. Also the rebook driver in post-stay emails.
- **Map + Directions Block** (`map-directions`) — Static map thumbnail (Google/Mapbox tile) + 'Get directions' link + address as live text, for physical stays, bookings and check-in reminders. Map links out rather than embedding heavy interactive tiles.
- **Policy / Need-to-Know Strip** (`policy-strip`) — Cancellation/change/check-in rules (travel), settlement/dispute window (fintech receipt), or 'what's changing / what you need to do' for regulatory notices. The reference fine print people return to; plain live text.
- **Whitelist / Deliverability Tip Box** (`whitelist-tip`) — Bordered/tinted box isolating a 'move us to Primary / add us to contacts' instruction in welcome/onboarding emails to protect inbox placement.
- **Referral / Share Module** (`referral-share`) — 'Share the brew' / refer-a-friend / 'share your experience' block with share buttons or a referral incentive. Engagement/growth driver, placed after the primary content.
- **Support / Help Strip** (`support-strip`) — 'Questions? Contact us', 'Where is my order?', 'Didn't request this?', 'something wrong with your trip?' — a low-friction escape hatch / safety line near the end (and the negative-review diverter in post-stay emails).
- **Footer** (`footer`) — Company legal entity + physical address, account/manage-subscription/order-lookup links, social row, app-store badges, and unsubscribe (omitted/limited for purely transactional mail; mandatory for marketing). Fintech/regulatory adds license/registration numbers and anti-phishing note.

## Genre → default archetype map

| Genre | Use case | Default | Alternates |
|---|---|---|---|
| Ecommerce / fashion / apparel | New collection drop / lookbook / brand-moment campaign | `image-only` | `split`, `text-over-image` |
| Ecommerce / fashion / apparel | Seasonal sale / markdowns / Black Friday (promotion-led) | `text-over-image` | `color-block`, `product-cutout` |
| Ecommerce / fashion / apparel | Editorial 'The Edit' / trend round-up / gift guide | `split` | `image-only` |
| Ecommerce / fashion / apparel | Single-product / new-arrival spotlight / restock | `image-only` | `product-cutout`, `split` |
| Ecommerce / fashion / apparel | Flash promo / last chance / asset-light offer | `color-block` | `product-cutout` |
| Ecommerce / fashion / apparel | Multi-department 'New In' mosaic | `product-cutout` | `color-block`, `image-only` |
| D2C / CPG / supplement / beauty / food | Lifestyle newsletter / brand-moment / 'day with the product' | `image-only` | `split`, `text-only` |
| D2C / CPG / supplement / beauty / food | Product launch / new flavor or shade / hero-SKU promo | `product-cutout` | `color-block`, `baked-art` |
| D2C / CPG / supplement / beauty / food | Ingredient / science education / 'how it works' explainer | `split` | `image-only`, `text-only` |
| D2C / CPG / supplement / beauty / food | Replenishment / subscription reorder reminder | `product-cutout` | `text-only`, `color-block` |
| D2C / CPG / supplement / beauty / food | Minimalist beauty announcement / back-in-stock / waitlist | `color-block` | `text-only`, `image-only` |
| D2C / CPG / supplement / beauty / food | Playful sale / variety-pack / community-UGC (challenger brand) | `baked-art` | `product-cutout`, `color-block` |
| SaaS / B2B tech | Major product / platform launch (flagship) | `illustrated-banner` | `color-block`, `split` |
| SaaS / B2B tech | Changelog / 'what's new' recurring roundup | `color-block` | `split` |
| SaaS / B2B tech | Single-feature or integration spotlight | `text-over-image` | `image-only`, `split` |
| SaaS / B2B tech | Onboarding day-0 / nurture / founder note | `text-only` | `split` |
| SaaS / B2B tech | Company announcement (funding / pricing / event / waitlist) | `color-block` | `baked-art`, `illustrated-banner` |
| SaaS / B2B tech | Activation onboarding checklist / getting-started | `split` | `text-only` |
| SaaS / B2B tech | Transactional / system notice (receipt, invite, security) | `status-card` | `color-block`, `text-only` |
| Consumer hardware / wearables / electronics | Product reveal / 'it's here' launch | `image-only` | `text-over-image`, `color-block` |
| Consumer hardware / wearables / electronics | Spec showcase / 'meet the technology' (performance, dark-mode) | `text-over-image` | `split`, `image-only` |
| Consumer hardware / wearables / electronics | Pre-order / reservation drive (scarcity + ship date) | `split` | `color-block`, `image-only` |
| Consumer hardware / wearables / electronics | Lifestyle-led / aspirational launch | `image-only` | `text-over-image`, `split` |
| Consumer hardware / wearables / electronics | Comparison / gen-over-gen upgrade nudge | `split` | `baked-art`, `image-only` |
| Consumer hardware / wearables / electronics | Pre-launch teaser / countdown | `color-block` | `baked-art`, `image-only` |
| Transactional (commerce, account, booking) | E-commerce order confirmation | `status-card` | `color-block`, `baked-art` |
| Transactional (commerce, account, booking) | Shipping / delivery / tracking notification | `status-card` | `illustrated-banner` |
| Transactional (commerce, account, booking) | Receipt / invoice (SaaS, payment, subscription) | `status-card` | `color-block`, `text-only` |
| Transactional (commerce, account, booking) | Security / account action (password, OTP, sign-in alert) | `status-card` | `text-only` |
| Transactional (commerce, account, booking) | Reservation / booking confirmation (travel, ticket, appointment) | `split` | `status-card`, `image-only` |
| Transactional (commerce, account, booking) | Brand-forward order confirmation (D2C / fashion) | `baked-art` | `status-card`, `color-block` |
| Newsletter / media / editorial / content | Sectioned daily digest (multi-story, sponsor slots) | `color-block` | `text-over-image`, `split` |
| Newsletter / media / editorial / content | Smart-brevity brief (skimmable, 'why it matters') | `text-over-image` | `image-only`, `color-block` |
| Newsletter / media / editorial / content | Single-essay creator letter (long-form) | `image-only` | `text-only` |
| Newsletter / media / editorial / content | Curated link roundup (recommendations by category) | `split` | `text-only`, `color-block` |
| Newsletter / media / editorial / content | Designed editorial magazine issue (art-directed) | `illustrated-banner` | `split`, `color-block` |
| Newsletter / media / editorial / content | Welcome / subscriber onboarding | `text-over-image` | `color-block`, `text-only` |
| Travel / hospitality / events | Booking confirmation receipt (stay / flight) | `image-only` | `status-card`, `split` |
| Travel / hospitality / events | Trip itinerary / pre-arrival timeline | `text-over-image` | `illustrated-banner`, `split` |
| Travel / hospitality / events | Event invite (ticketed event / conference / experience) | `text-over-image` | `baked-art`, `split` |
| Travel / hospitality / events | Pre-event / check-in reminder (time-sensitive) | `color-block` | `status-card` |
| Travel / hospitality / events | Destination inspiration / promotional discovery | `image-only` | `text-over-image`, `product-cutout` |
| Travel / hospitality / events | Post-stay / post-event follow-up (review + rebook) | `color-block` | `text-over-image`, `image-only` |
| Fintech / banking / insurance | Transactional receipt / money-moved confirmation | `color-block` | `status-card`, `text-only` |
| Fintech / banking / insurance | Security / account alert (login, OTP, frozen card) | `status-card` | `color-block`, `text-only` |
| Fintech / banking / insurance | Periodic statement / account summary / 'wrapped' | `color-block` | `baked-art`, `stats-row` |
| Fintech / banking / insurance | Product / feature announcement | `illustrated-banner` | `color-block`, `split` |
| Fintech / banking / insurance | Onboarding / welcome setup checklist (KYC, fund, activate) | `split` | `text-only`, `illustrated-banner` |
| Fintech / banking / insurance | Promotional offer / incentive (referral, APY boost) | `baked-art` | `color-block` |
| Fintech / banking / insurance | Regulatory / policy / document notice | `text-only` | `color-block` |

## Build & image guardrails (apply to every email)

1. FORMATS: use jpg (photos/gradients) and png (cutouts, flat graphics, transparency, crisp type) only. NEVER webp or svg in email bodies — Outlook, Gmail and several clients won't render them; convert any svg logo/icon to png. Animated GIF is allowed (replenishment loops, feature demos) but must carry a meaningful first static frame.
2. DIMENSIONS + RETINA: every <img> gets explicit width and height attributes and display:block to prevent reflow and gaps; export source assets at 2x the display size (e.g. 1200px wide for a 600px container) and constrain with width/max-width for crisp retina rendering. Cap hero file size for load/deliverability.
3. BLOCKED-IMAGE FALLBACK: images are blocked by default in many clients, so every image cell gets a bgcolor fallback (sampled near the image's dominant tone) and every <img> gets descriptive alt text. The email's core message, offer and especially the CTA must be fully understandable with all images off.
4. CTA / CRITICAL INFO IS NEVER IMAGE-ONLY: CTAs are live HTML/bulletproof buttons, never baked into a hero. Prices, dates, ship dates, rates, order/confirmation numbers, OTP codes and totals are always selectable live text — never rendered as images (they go stale, break in dark mode, and fail screen readers and autofill).
5. NO PLACEHOLDER BOXES IN FINAL HTML: never ship grey 'image placeholder' rectangles, lorem-ipsum tiles, or empty src. Either use a real brand/catalog/generated asset, or fall back to a CSS color-block / cutout-on-tile / icon — but never a dummy box. If a product image is genuinely missing, use a neutral on-brand fallback tile, not a stretched or broken graphic.
6. OVERLAY TEXT NEEDS BAKED SCRIM + SAFE-ZONE + SHADOW: any text set over a photo requires a darkening scrim/gradient baked INTO the image asset (not a CSS overlay Outlook may drop), the text confined to a pre-cleared safe zone (sky/wall/negative space), and a text-shadow as backup. Contrast must pass AA against the darkest region the text can touch; keep overlay copy short (2-6 words).
7. OUTLOOK BACKGROUND IMAGES NEED VML: CSS background-image is ignored by Outlook (Word engine), so any text-over-image or full-bleed background-image hero must include a VML <v:rect>/<v:fill>/<v:textbox> fallback to render the image and overlaid text in Outlook. When in doubt, prefer the caption-below-image (image-only) pattern, which needs no VML.
8. USE REAL FIRST-PARTY ASSETS WHERE CREDIBILITY MATTERS: transactional receipts, travel booking confirmations and fintech mail use real catalog/property/inventory photos or NO photo at all — never generic stock or generated lifestyle imagery, which erodes trust and can trip spam filters. Listing/price cards always keep facts as live text beneath the photo so prices stay accurate.
9. GRID & TILE CONSISTENCY: within any product grid or category mosaic, every cell shares identical aspect ratio, crop, padding, background and label placement; never mix on-white and on-model thumbnails in the same grid. Cutouts must be clean-edged with a soft baked contact shadow and no leftover white halo.
10. DARK MODE: don't bake body/spec/headline copy into images (inverts/clashes in dark mode); keep copy as live text. Provide transparent-PNG logos/icons that read on both light and dark backgrounds, and design dark-native emails dark end-to-end rather than inverting a light layout.

### Additional guardrails (from the completeness review)

1. **Preheader is mandatory** — ship the hidden preview-text span + spacer (block #0); never let the client auto-pull body/footer text into the snippet.
2. **Ship a real `text/plain` MIME part** of the multipart/alternative (not just an HTML email that looks plain) — affects deliverability/spam scoring and is the true fallback.
3. **Dark-mode mechanics:** declare `<meta name="color-scheme">` + `supported-color-schemes`; expect Gmail-app / Outlook.com **forced inversion** to flip brand colors — pad transparent-PNG logos / add a 1px off-white stroke so they survive; avoid pure #000/#FFF; design dark-native emails dark end-to-end.
4. **Gmail clips past ~102KB** of HTML ('[Message clipped]') — keep total HTML small, avoid giant base64-inlined images, keep the primary CTA + unsubscribe above the clip threshold.
5. **Accessibility semantics:** `role="presentation"` on layout tables; `lang` on `<html>`; one logical H1, no skipped heading levels; `aria-hidden` on decorative spacers; real data tables (comparison / line-item / order-summary) use `scope`/`caption`; descriptive link text (never 'click here').
6. **Motion safety:** with GIF/animation, respect `prefers-reduced-motion`, no flashing >3x/sec (WCAG 2.3.1), and never put essential content only on a mid-loop frame (Outlook + Apple Mail Low-Power freeze on frame 1 — frame 1 must be the money frame).
7. **Merge-tag fallbacks:** every personalization token has a default (never 'Hi ,'); never bake personalized/localized copy into an image; never reflect unsanitized user data.
8. **Internationalization:** support `dir="rtl"` (mirror layout/CTA/steppers/itinerary) for Arabic/Hebrew/Farsi; don't bake localized copy (German/CJK overflow the cleared safe zone); localize number/date/currency/timezone in status-card/itinerary/order-summary.

## Notes on the synthesis

Synonym merges across the 8 genres: the research named many hero variants that collapse into 9 canonical archetypes. 'destination banner', 'dark-mode device-on-black hero' → text-over-image. 'boarding-pass card', 'gen-over-gen side-by-side', 'value-prop+CTA stack' → split. 'status header band', 'gradient announcement', 'masthead color block', 'hero-amount/KPI block' → color-block. 'product-cutout on color block', 'cutout grid/mosaic', travel 'photo-on-top facts-beneath' → product-cutout. 'illustrated launch banner', 'designed magazine masthead', 'shipping progress banner', 'sticker collage' → illustrated-banner. 'typographic offer panel', 'teaser silhouette', 'wrapped stat panel' → baked-art. 'plain-text founder note', 'minimalist type-forward', 'document-plain regulatory/receipt' → text-only. 'icon-led centered card', 'confirmation header + checkmark', 'OTP card', 'transactional system notice' → status-card (added as the canonical transactional hero; the research often labeled these 'color-block', 'baked-art', 'text-over-image' or 'icon-led card', but they share one CSS-only, table-and-type, no-marketing-imagery build).

Key cross-genre principle that drove the de-duplication: the SINGLE most repeated reliability win is the caption-below-image pattern (image-only) and its corollary that any baked/overlaid text must be mirrored as live HTML — this recurs verbatim in fashion, D2C, hardware, SaaS, travel and newsletter research. Archetypes are deliberately ordered from most-reliable (image-only, color-block, status-card, text-only) to highest-risk (text-over-image, baked-art) so a builder can default to the safe option and only reach for VML-dependent overlays when the genre truly demands atmosphere. Two non-hero blocks (stats-row, status-card) also appear in genreDefaults as fallbacks where a genre's hero IS a data/status moment rather than a marketing visual."

---

*Generated from the `email-structure-research` workflow (8-genre research → synthesis → completeness critique). Extend it: when the run-time research agent derives a new structure not here, add it as a new archetype/block so it's reused next time.*