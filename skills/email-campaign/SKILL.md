---
name: email-campaign
description: "Run a complete email marketing campaign end-to-end: intake a brief (or interview the user), decide one-shot vs. multi-email journey, write the brief document, design each email, sharpen the copy, then push everything into the user's connected email CLM (Klaviyo, Mailchimp, HubSpot, or any email MCP) as reviewable drafts. Tool-agnostic over the CLM, never auto-sends. This is the front door for the email-marketing plugin. Use for: planning/building an email campaign, drip/journey/flow/automation, welcome or nurture or win-back sequences, one-off broadcasts, 'set up a campaign in Klaviyo/Mailchimp', 'create an email flow', 'send a newsletter to my list'. Triggers: email campaign, email journey, drip campaign, email flow, email automation, nurture sequence, welcome series, win-back, broadcast, Klaviyo campaign, Mailchimp campaign, send to my list/segment."
---

# Email Campaign

Orchestrate a marketing email campaign from brief to a draft sitting in the user's email tool,
ready for their final review. This skill owns the **process**; it delegates the **craft** to
companion components and the **delivery** to whatever CLM MCP is connected.

- **Brand kit** (when the design phase needs it) → the `email-templates` skill, which uses the `brand-kit-extractor` agent.
- **Design** of each email → the `email-templates` skill.
- **Imagery** → the image-generation skill.
- **Copy review** → the `copy-critic` agent.
- **Delivery** → the connected email CLM MCP.

If a component or MCP isn't available, degrade gracefully (noted per phase) — never block.

## Core principles (read first)

1. **Never auto-send.** Everything is created as a **draft / paused flow**. Sending or
   activating requires an explicit, separate "yes, send it" from the user — confirmed against
   the exact audience and send time. State the recipient count before any send. (A send-guard
   hook also forces a confirmation prompt, but don't rely on it — design for human approval.)
2. **Defer MCP calls.** Don't touch the CLM until content is ready. Gather audience/segment
   *intent* in the brief; resolve the real list/segment and create objects in Phase 7.
3. **Brand-agnostic until Phase 5.** Don't load brand tokens or read a design-system skill
   during intake/brief/structure. The brief only carries the brand **URL/name** as a pointer;
   the brand kit is resolved once when the design phase starts, then reused across all emails.
4. **Tool-agnostic CLM.** Discover the connected CLM and map generic concepts onto its
   primitives (see the CLM map). Detect it — don't assume Klaviyo vs. Mailchimp.
5. **One gate per phase.** Each phase ends with a short confirmation before the next begins.
6. **Compliance is non-negotiable.** Every email needs a working unsubscribe, a real sender
   identity, and a physical mailing address. Flag missing pieces; don't fabricate them.

## The workflow

```
1 Intake ─▶ 2 One-shot vs journey ─▶ 3 Brief doc ─▶ 4 Campaign structure
        ─▶ 5 Design each email ─▶ 6 Sharpen copy ─▶ 7 Push to CLM (drafts)
```

### Phase 1 — Intake (front door + fast-path)

This skill is the plugin's front door. First, read intent:

- **Single email, design only** (e.g. "build/design a welcome email") → offer the **fast path**:
  hand straight to the `email-templates` skill, produce the HTML, and stop. No brief, no MCP.
- **A campaign / journey, or anything to be delivered** ("create / set up / send / launch", "in
  Klaviyo", "series/flow") → run the full workflow below.

If intent is ambiguous, ask which they want before proceeding.

For the full workflow, interview the user — ask only what you can't infer, in one batched round:

- **Goal** — what should this achieve? (sales, signups, re-engagement, onboarding, announcement) + the **one metric** for success.
- **Audience** — existing list/segment, or a condition ("trialed but didn't convert")? Rough size if known.
- **Brand / product** — the brand's website URL (pointer for the design phase) and what's being promoted.
- **Offer / message** — the core thing to say.
- **Constraints** — deadline/send window, sender name & reply-to, legal/medical review needs, frequency caps.

→ **Gate:** play back a one-paragraph summary; confirm.

### Phase 2 — One-shot or journey?

Ask explicitly: **a single email (one-shot broadcast), or a multi-email journey (sequence/flow over time)?**

- **One-shot** — a single moment: sale, newsletter, event invite, launch blast. Sent once to a list/segment.
- **Journey** — a triggered, time-spaced sequence: welcome, nurture, abandoned-cart, post-purchase, win-back. Each email has a trigger, a delay, and exit conditions.

Recommend by goal if unsure (onboarding/nurture/retention → journey; announcement/sale/newsletter → one-shot).

→ **Gate:** confirm. This determines the structure in Phase 4 and the CLM object in Phase 7.

### Phase 3 — Brief document

Produce (or complete) a written brief. Fill from Phases 1–2; ask only for missing essentials.

```
CAMPAIGN BRIEF
  Name:            {campaign name}
  Type:            one-shot | journey ({N} emails)
  Goal:            {objective} · Success metric: {metric}
  Audience:        {list/segment name or condition} · est. size {n}
  Brand:           {website URL}   (pointer — resolved into a brand kit at Phase 5)
  Offer/message:   {the core value/offer}
  Voice:           {2–4 adjectives}
  Primary CTA:     {action} → {destination URL}
  Send window:     {date/time or trigger} · From: {sender} · Reply-to: {address}
  Compliance:      unsubscribe ✓ · mailing address {…} · review needed? {y/n}
  Per-email plan:  {see Phase 4 structure}
```

→ **Gate:** share the brief; get a thumbs-up or edits.

### Phase 4 — Campaign structure

**One-shot:** define the single send — use-case/recipe, subject + preview direction, primary CTA, target segment, send time.

**Journey:** map the full sequence before designing anything:

| # | Purpose | Trigger / delay | Subject angle | Primary CTA | Exit / goal condition |
|---|---------|-----------------|---------------|-------------|-----------------------|

Use the journey patterns reference for sensible defaults. Keep journeys as short as the goal allows.

**Audience intent (defer the MCP call):** record which list/segment each send targets and the
exit/suppression rules — but do **not** create or query the CLM yet. Note "resolve in Phase 7".

→ **Gate:** confirm the one-shot plan or the journey map. The most important sign-off.

### Phase 5 — Design each email

Build the emails **one at a time**, in order. For each:

1. Invoke the **`email-templates`** skill with: the brand URL (→ brand kit), the use case/recipe,
   and this email's subject/headline/CTA/offer from the brief. It returns production HTML and
   resolves the brand kit once (installed design-system skill → `brand-kit-extractor` agent → ask).
2. Reuse the same brand kit across all emails in a journey — derive it once.

If `email-templates` isn't installed, fall back to Outlook-safe, inline-CSS, 600px table HTML, and say so.

→ **Gate:** show each email (or a batch) for approval. Nothing reaches the CLM yet.

### Phase 6 — Sharpen the copy

After an email's design exists, run the **`copy-critic`** agent on its copy. The agent returns
subject-line variants, preview text, tightened body, a single clear CTA, and compliance flags.
Apply its output (use judgment; the brief's voice wins on conflicts).

→ **Gate:** confirm final copy per email.

### Phase 7 — Push to the CLM (as drafts)

Only now touch the MCP:

1. **Discover** the connected CLM (search tools for klaviyo, mailchimp, hubspot, sendgrid,
   customer.io, …). If none is connected, deliver the HTML files + a copy/paste setup checklist, and stop.
2. **Resolve audience** — list existing lists/segments; have the user pick, or create the segment
   from the brief's condition if supported. Confirm the match.
3. **Create content** — create each email/template with its HTML, subject, preview, sender identity.
4. **Assemble**:
   - One-shot → create a **campaign/broadcast** in **draft**, bound to the segment.
   - Journey → create the **flow/automation** with each email, trigger, delays, exit conditions, left **paused**.
     If the MCP can't create flows, create the individual templates and give the user an ordered,
     click-by-click guide to wire the flow in the UI — and say which parts you couldn't automate.
5. **Report** exactly what was created (IDs/links), what's draft vs. paused, what remains manual.

→ **Final gate:** the campaign is a draft. **Do not send or activate.** Explain how to review;
send only if explicitly asked, restating recipient count and audience first.

## Reference — Journey patterns (defaults; tune to the brief)

| Journey | Emails | Trigger | Typical cadence | Goal / exit |
|---|---|---|---|---|
| Welcome / onboarding | 3–5 | Signup | Day 0, 1, 3, 7 (, 14) | First key action taken |
| Lead nurture | 4–6 | Lead magnet / form fill | Every 3–5 days | MQL → demo/trial |
| Abandoned cart | 3 | Cart abandon | 1h, 24h, 72h | Purchase |
| Post-purchase | 3–4 | Order placed | Confirm → ship → use → review/cross-sell | Repeat purchase / review |
| Win-back / re-engagement | 3 | N days inactive | Day 0, 7, 14 | Re-open / purchase; suppress if cold |
| Product launch | 3 | Date-based | Tease → launch → last-call | Conversion before deadline |

## Reference — CLM concept map (detect, don't assume)

| Generic concept | Klaviyo | Mailchimp | HubSpot |
|---|---|---|---|
| Audience | List / Segment | Audience / Tag / Segment | List |
| Email content | Template | Template / Campaign content | Marketing Email |
| One-shot | Campaign | Campaign (Regular) | Marketing Email (send) |
| Journey | Flow (triggered) | Customer Journey / Automation | Workflow |
| Trigger | Metric / list-join | Journey starting point | Enrollment trigger |
| Personalization | `{{ first_name }}` | `*|FNAME|*` | `{{ contact.firstname }}` |

Confirm exact tool names/params from the connected MCP's schema at call time. Many MCPs expose
read + campaign-create but **not** flow-create; when flow-create is missing, automate the emails
and document the manual flow assembly.

## What this skill does not do

- It doesn't invent audience data, prices, or claims — it asks.
- It doesn't manage deliverability infra (SPF/DKIM/DMARC) or post-send analytics — flag as follow-ups.
- It doesn't send or activate anything without an explicit, audience-confirmed go-ahead.
