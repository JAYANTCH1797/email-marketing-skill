---
name: email-templates
description: "Build production HTML email templates for any brand and use case. Resolves the brand's design system first (installed brand/design-system skill, else the brand-kit-extractor agent scrapes the brand's website), then applies a universal quality baseline plus a use-case recipe (ecommerce sale, SaaS launch, consumer hardware, D2C supplement, transactional, or default editorial) to produce agency-grade, Outlook-safe HTML. Use for: building/designing a single email template, generating campaign HTML, coding a branded email, hero/promo/launch/receipt/welcome emails. Triggers: build email template, design an email, create email HTML, email template for, branded email, sale email, product launch email, order confirmation email, welcome email, newsletter template, transactional email."
---

# Email Templates

Build a complete, production-ready HTML email that looks like a top-tier agency made it —
**on-brand**, never a generic AI-chatbot template.

Work in three layers, in order:
1. **Brand kit** (Step 1) — what the brand looks and sounds like. Resolved per-brand; never assumed.
2. **Quality baseline** (Step 3) — universal rules applied to every email.
3. **Use-case recipe** (Step 5) — the layout/genre for this specific email.

## Step 1 — Resolve the brand kit (do this FIRST)

Never invent a brand's look. Resolve it in this priority order:

**A. Installed brand / design-system skill.** If a skill for this brand's design system is
available (e.g. an `*-design-system` skill), use it — it's the source of truth for colors,
fonts, components, and voice. Skip to Step 2.

**B. The `brand-kit-extractor` agent.** Otherwise, hand the brand URL to the `brand-kit-extractor`
agent. It scrapes the site in an isolated context and returns a structured brand kit:
```
BRAND KIT — {brand}
  logo:    {hosted URL}
  colors:  primary {#}, secondary {#}, accent {#}, bg {#}, surface {#}, text {#}
  fonts:   heading "{Google Font}"  body "{Google Font}"  (orig: {brand font})
  button:  {radius}px, {fill|outline}, {uppercase|title-case}, accent fill
  imagery: {photography|illustration}, {light|dark}, {lifestyle|product}, {mood}
  voice:   {2–4 adjectives}
```

**C. Fallback.** If there's no URL and nothing scrapeable, ask the user for colors/fonts/logo,
or use a neutral high-quality default (one confident accent, a distinctive Google Font heading +
clean sans body, generous spacing). Flag clearly that you used defaults.

**Font caveat:** emails can only use web-safe or Google Fonts via `@import`. If the brand uses a
licensed/custom font not on Google Fonts, the kit maps it to the **closest Google Font** and keeps
the brand font only in the Outlook fallback stack.

Everything downstream reads from this brand kit. The named fonts/colors in the recipes are
**genre fallbacks** for tokens the kit doesn't pin down — the brand kit always wins.

## Step 2 — Classify the use case

| Campaign type / description | Recipe | Hero mode |
|---|---|---|
| sale, flash sale, promotional, ecommerce discount | **Ecommerce Hero** | overlay-text |
| product launch, feature announcement (software/tech) | **SaaS Launch** | illustration |
| smart ring, wearable, device / gadget launch | **Consumer Hardware** | overlay-text photo |
| protein, supplement, nutrition, wellness D2C | **D2C Supplement** | overlay-text photo |
| order confirmation, receipt, shipping, account email | **Transactional** | none |
| welcome, newsletter, general, anything else | **Default Editorial** | standard-photo |

When unsure, use **Default Editorial**.

## Step 3 — Universal quality baseline (ALWAYS apply, reading from the brand kit)

### Typography
- Import the brand kit fonts (or genre fallbacks) in `<head>`: `<style>@import url('https://fonts.googleapis.com/css2?family=...&display=swap');</style>`.
- NEVER use Inter, Roboto, Arial, Helvetica, or system-ui as the *named design font* (Outlook fallback stacks only). Exception: transactional emails may use system fonts.
- ONE display font (headings) + ONE body font. Never more than 2 active.
- Hierarchy: hero headline 48–64px, section headline 28–36px, body 15–16px, line-height 1.5–1.65.

### Color
- Build the palette from the brand kit. Apply the **60-30-10 rule**: dominant (60%) + supporting (30%) + accent (10%).
- Every section needs a clear background color — no unbroken walls of white.
- NEVER timid equally-distributed schemes or generic purple→blue gradients.

### Spatial composition
- Generous padding: ≥48px vertical between sections, ≥24px horizontal.
- ONE thing dominates each section. Vary rhythm (full-bleed / narrow / text-only).

### Visual quality
- Full-width section backgrounds via `<table width="100%">` with `bgcolor` + inline `background-color`.
- border-radius 4–8px on buttons/images (mirror the brand's button radius); subtle `box-shadow: 0 2px 8px rgba(0,0,0,0.15)` inline.
- Thin 1px accent-color dividers — never thick grey bars.

### Layout structure
- Max content width 600px (640px for editorial recipes), centered in a full-width wrapper table.
- **Tables for layout** (Outlook-safe), **inline CSS throughout**, no external stylesheets.
- Single-column below 480px. Alt text on every `<img>`.

### NEVER
- Generic equal-grey-block layouts. Centered body paragraphs (left-align body; center only headings + CTAs). Clipart icons or boilerplate filler text.

## Step 4 — Technical fundamentals

### Base skeleton
```html
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:v="urn:schemas-microsoft-com:vml">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="x-apple-disable-message-reformatting">
  <title>{SUBJECT}</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=DISPLAY_FONT&family=BODY_FONT&display=swap');
    @media only screen and (max-width:480px){
      .col{display:block!important;width:100%!important}
      .px{padding-left:20px!important;padding-right:20px!important}
    }
  </style>
</head>
<body style="margin:0;padding:0;background:#PAGE_BG;">
  <div style="display:none;max-height:0;overflow:hidden;opacity:0;">{PREVIEW_TEXT}</div>
  <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0" bgcolor="#PAGE_BG">
    <tr><td align="center">
      <table role="presentation" width="600" cellpadding="0" cellspacing="0" border="0" style="max-width:600px;width:100%;">
        <!-- sections go here -->
      </table>
    </td></tr>
  </table>
</body>
</html>
```

### Bulletproof button (Outlook-safe) — fill with the brand accent + brand button shape
```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0">
  <tr><td align="center" bgcolor="#ACCENT" style="border-radius:6px;">
    <a href="{CTA_URL}" target="_blank"
       style="display:inline-block;padding:14px 32px;font-family:sans-serif;font-size:16px;
              font-weight:700;color:#ffffff;text-decoration:none;letter-spacing:0.05em;">
      {CTA_TEXT}
    </a>
  </td></tr>
</table>
```
CTA rules: 200–300px wide (full-width only when the recipe says so), action verb + outcome,
**one primary CTA per email**, repeat at the bottom of long emails.

### Subject + preview
- Subject 30–50 chars (mobile truncates ~35). No ALL CAPS, no spam triggers in the subject.
- Preview text extends the subject — never "View in browser".

### Mobile + deliverability
- Single column, ≥14px body, ≥44px tap targets, images scale to 100%.
- Image-to-text ratio ≤40% images / ≥60% text; alt text on all images.
- Unsubscribe link in footer; real reply-to.

## Step 5 — Use-case recipes

Pull colors / fonts / logo / button shape from the **brand kit** first. The named fonts/colors are
genre fallbacks. For copy, use the brief's headline/CTA verbatim where provided.

### Ecommerce Hero — Dark Luxury Editorial
ZARA / NET-A-PORTER / Mytheresa. 640px. Headline baked ON the hero image.
- **Layout:** full-width hero (headline on image) → subheadline + 2-sentence narrative → 3-col product grid (image / title-case name / `<s>`orig` → sale in accent) → full-width uppercase CTA → urgency strip → dark footer.
- **Type (fallback):** Cormorant Garamond / Playfair Display headings; clean sans body 15px.
- **Color (fallback):** dark hero (`#0a0a0a`) → light content → dark footer. One sharp accent. 1px accent rule between hero and grid.
- **Copy:** headline is on the image — no `<h1>` repeating it.

### SaaS Launch — Tech-Forward Illustration
Notion / Linear / Stripe / Vercel. 600px. Flat illustration or UI mockup — **never photography/people**.
- **Hero patterns:** floating icons on dark · product UI in device frame · abstract glowing nodes.
- **Layout:** logo header → hero illustration (rounded container, radius 16px, dark/brand bg) → badge + 44–52px H1 → 3-item feature grid (icon + bold title + 1-line) → centered CTA → optional social proof → footer.
- **Type (fallback):** Space Grotesk (400/500/700) + DM Sans. H1 44–52px/700/lh 1.1; badge 11px/600/2px/uppercase.
- **Color (fallback):** dark (`#0d0d0d`/`#111827`), brand accent CTA, muted gray `#6b7280` body. CTA radius 8px or 50px pill.
- **Don't:** stock photos/people, >3 colors, justified text, cheap full-width CTA.

### Consumer Hardware — Premium Editorial
EightSleep / Oura / WHOOP. 600px. Lifestyle/product photo with headline baked on.
- **Layout:** minimal logo (light on dark) → full-bleed hero (headline on image) + CTA only below → 3-col specs grid (big stat + label + 1-line) → key-differentiator sentence (centered quote) → CTA banner (accent bg, scarcity) → dark footer.
- **Type (fallback):** Space Grotesk (700/400) + DM Sans. Spec labels uppercase, 2px tracking, 11–12px.
- **Color (fallback):** deep dark throughout (`#0a0a0a`/`#0f172a`), ONE accent. White text on dark — never gray on dark.
- **Don't:** white hero, generic desk/phone stock, text-heavy sections, lifestyle without the product visible.

### D2C Supplement — Bold Results-Driven
MuscleBlaze / AG1 / Huel. 600px. Energy + credibility.
- **Layout:** logo + credibility badge → hero (product in frame, real context, headline baked) + CTA only → benefits block (3× icon + bold title + 1–2 sentence) → social proof (one metric or quote) → CTA banner (offer line) → footer.
- **Type (fallback):** Montserrat (800/600/400) or Barlow Condensed + DM Sans. Headline 40–52px bold.
- **Color (fallback):** dark → deep base + bold accent; light → white base + brand primary. Never clinical all-white.
- **Don't:** clinical product-only shots, product without label, long copy, generic gym stock without product.

### Transactional — Clean Data-First
Order confirmations, receipts, shipping. 600px. **No hero image. No marketing copy.** Function over form.
- **Layout:** logo + status headline → confirmation block (order #, date, status, bordered box) → order summary table (item/qty/price/total, alternating rows) → details block (address, delivery, payment) → single utility CTA → footer.
- **Type:** clean system fonts acceptable. Data labels bold 13px; values 14px; headline 24–28px bold.
- **Color:** white / `#fafafa`, black/near-black text. Brand accent ONLY on CTA and status badge. No dark sections.
- **Don't:** hero images, promotional copy mixed in, dark editorial sections.

### Default Editorial — Clean Editorial
Catch-all: newsletters, welcome, announcements. 640px. Clean editorial photo, NO text overlay.
- **Layout:** minimal brand header (logo on brand-color bg) → full-width hero (no text on image) → H1 + subheadline in HTML → 2–3 left-aligned, benefit-led paragraphs → centered primary CTA → footer.
- **Type (fallback):** serif brands → Lora; tech/modern → DM Sans / Space Grotesk. H1 40–48px, body 15–16px/1.6.
- **Color:** brand primary for header bg + CTA. Content white or `#FAFAFA`. Footer medium-dark. Minimal decoration.

## Step 6 — Hero & lifestyle images

For every recipe except Transactional, you need imagery. Two sources, in order:
1. **Brand-provided asset** — if the brief includes a product/hero image URL, use it directly.
2. **Generate it** — otherwise invoke the companion image-generation skill with the **brand kit**
   (colors, mood, imagery style) + the recipe's hero mode (`overlay-text` / `standard-photo` /
   `illustration`). Specify lighting, mood, and text-overlay zone. Insert the returned URL with alt text.

## Step 7 — Self-check before delivering

- [ ] Brand kit resolved (design-system skill → brand-kit-extractor → fallback) and colors/fonts/logo come from it?
- [ ] Licensed brand fonts mapped to the closest Google Font for the `@import`?
- [ ] Correct recipe chosen and applied on top of the baseline?
- [ ] Distinctive design font — no Inter/Roboto/Arial as the named font (except transactional)?
- [ ] Intentional 60-30-10 palette; every section has a background?
- [ ] Tables for layout, all CSS inline, ≤600/640px wrapper, single-column on mobile?
- [ ] Bulletproof button matching the brand's shape; ONE primary CTA, ≥48px tall?
- [ ] Subject ≤50 chars + intentional preview text + alt text on every image?
- [ ] Overlay-text heroes: no `<h1>` repeating the baked-in headline?
- [ ] Unsubscribe link present in footer?
