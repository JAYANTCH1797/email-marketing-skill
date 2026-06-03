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

## Step 2 — Classify the use case (each type carries an image pipeline)

Pick the type. Each one comes with a default **image pipeline** — what imagery it needs and how the
hero is built. This is a contract, not a suggestion: it drives Step 2.5 and Step 6.

| Campaign type / description | Recipe | Needs hero | Default hero mode | Lifestyle imgs | Max images |
|---|---|---|---|---|---|
| sale, flash sale, promotional, ecommerce discount | **Ecommerce Hero** | yes | `overlay-text` | yes | 3 |
| product launch, feature announcement (software/tech) | **SaaS Launch** | yes | `illustration` | no | 1 |
| smart ring, wearable, device / gadget launch | **Consumer Hardware** | yes | `overlay-text` | no | 1 |
| protein, supplement, nutrition, wellness D2C | **D2C Supplement** | yes | `overlay-text` | yes | 1–3 |
| order confirmation, receipt, shipping, account email | **Transactional** | **no** | `none` | no | 0 |
| welcome, newsletter, general, anything else | **Default Editorial** | yes | `standard-photo` | no | 1 |

When unsure, use **Default Editorial**.

### Hero mode glossary

- **`overlay-text`** — headline is **baked onto the image** (the image + type are one art-directed asset). Best for D2C/ecommerce/fashion lifestyle, where generated imagery shines. Requires the readability discipline in Step 2.5.
- **`standard-photo`** — clean photo, **no text on it**; the headline is live HTML below/over it.
- **`illustration`** — flat illustration or UI mockup, **no photography/people** (SaaS/tech).
- **`none`** — no hero image; the hero is built in HTML/CSS (transactional).

The default hero mode is a starting point — the **blueprint (Step 2.5)** can override it (e.g. switch
`overlay-text` → `standard-photo` for accessibility/localization). Decide there, with the user, before building.

## Step 2.5 — Email blueprint (decide & review BEFORE you build)

**Do not generate any image or write any HTML until this blueprint is approved by the user.**
Structure first; build second. This is what prevents the two classic failures: the hero headline
**repeating** body copy, and **unreadable text** dropped onto a busy image.

Produce this blueprint and get a thumbs-up:

```
EMAIL BLUEPRINT — {email name}
  Type / recipe:   {from Step 2}
  Section map:     (each section = ONE job; no text appears in two places)
    1. Header      — logo
    2. Hero        — {headline text} · {hero mode: overlay-text | standard-photo | none}
    3. {sub/body}  — {what copy lives here — must NOT restate the hero headline}
    4. {grid/feature/benefits} — {…}
    5. CTA         — {button text} → {destination}
    6. Footer      — unsubscribe + address
  Hero strategy:   {A | B | C — see decision tree}
  Image plan:      per slot → source + spec (see below)
  Dedup check:     {confirm hero text ≠ any body text}
  Readability:     {for baked/overlay text: zone, scrim, max words, min size}
```

### Hero-text strategy — pick ONE (this is the decision the user must see)

| | Strategy | How text is handled | Use when | Cost / risk |
|---|---|---|---|---|
| **A** | **Bake text into the image** (`overlay-text`) | Headline is rendered INTO the generated image by the image-gen skill | Art-directed D2C/fashion/ecommerce lifestyle; you control generation | Not editable/translatable, not selectable text, must verify legibility; **no `<h1>` repeating it in HTML** |
| **B** | **Live HTML text over a plain image** | Image generated/uploaded with a deliberate **empty safe-zone + dark scrim**; HTML places the headline as real text on top | Need accessibility, localization, editable copy, or a brand-provided photo | Image must be prompted/chosen with negative space + contrast; align text to the safe-zone on mobile too |
| **C** | **No overlay** (`standard-photo`) | Clean image; headline is HTML **below** it | Safest default; best deliverability + accessibility | Less "campaign-y" punch |

Rules that fall out of this:
- **Lock the content first.** Decide the headline and every body line in the section map *before*
  choosing A/B/C — so the hero never duplicates the body, and (for A) the baked text matches final copy.
- **A and B both require a readability spec:** text zone (e.g. lower-third / centered), a contrast
  scrim or gradient over the image, max headline length (≤8 words), and a minimum legible size.
- **If in doubt, choose C.** Only use A when the image is generated (so composition is controllable)
  and the copy is final.

### Cross-email check (journeys)

For a sequence, the blueprint set must be reviewed together: **no two emails reuse the same hero
headline or the same hero image treatment.** Vary the angle per email.

→ **Gate:** present the blueprint(s). Only after approval proceed to Step 6 (images) then build the HTML.

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

## Step 6 — Image pipeline (only after the blueprint is approved)

For every recipe except Transactional you need imagery. **Generate only after Step 2.5 is signed off** —
the hero strategy and final copy must be locked first (so baked text matches and nothing is repeated).

Two sources, in order:
1. **Brand-provided asset** — if the brief includes a product/hero image URL, use it directly (and, for
   strategy B, confirm it has a usable text safe-zone; if not, add a scrim in the HTML or fall back to C).
2. **Generate it** — invoke your installed **image-generation skill** (e.g. `image-gen`, or a
   creative-designer skill for art-directed baked-text heroes). Pass the **brand kit** (palette, mood,
   imagery style) plus a per-mode spec:

| Hero strategy / mode | What to tell the image skill |
|---|---|
| **A · `overlay-text`** (bake text in) | The exact final headline to render IN the image (≤8 words), its placement (e.g. lower-third), and a contrast treatment (dark gradient/scrim behind the text). Subject, lighting, mood from the brand kit. **Verify the returned image is legible** before using; if not, regenerate or switch to B. |
| **B · `standard-photo` + HTML overlay** | Generate WITHOUT any text, but compose with **negative space + a darker region** where the HTML headline will sit. Then place live text over that zone with a CSS scrim. |
| **C · `standard-photo`** (no overlay) | Clean image, no text, no reserved zone — headline goes in HTML below. |
| **`illustration`** | Flat illustration / UI mockup, no photography, no people (SaaS/tech). |

Notes:
- **Lifestyle generation is strongest for D2C / ecommerce** — product-in-context shots. For SaaS use
  illustration; for transactional, generate nothing.
- Respect the type's **max images** from Step 2. Insert every image with descriptive **alt text**.
- For strategy A, the HTML must **not** contain an `<h1>` repeating the baked headline (see self-check).

## Step 7 — Self-check before delivering

- [ ] Blueprint (Step 2.5) was approved before any image/HTML, hero strategy (A/B/C) chosen, dedup + readability confirmed?
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
