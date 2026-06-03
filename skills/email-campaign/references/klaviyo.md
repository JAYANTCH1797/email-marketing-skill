# Phase 7 — Klaviyo playbook

Concrete steps for delivering into **Klaviyo** when it's the connected CLM. This is the
provider-specific expansion of the email-campaign skill's Phase 7. The generic Phase 7 rules still
apply: **defer to the end, create drafts only, never send/activate without explicit go-ahead.**

> Tool names below are described by **capability**, not exact MCP method names — Klaviyo MCP
> servers vary. Inspect the connected server's schema and match by capability (list/segment read,
> template create, campaign create, message-content set). Confirm before each create call.

## Klaviyo object model → our concepts

| Our concept | Klaviyo object | Notes |
|---|---|---|
| Audience | **List** or **Segment** | List = static/opt-in; Segment = dynamic rule. Campaigns + flows both target these. |
| Email content | **Template** | HTML template object; reused by a campaign message. |
| One-shot | **Campaign** (channel = email) | Has one or more **Campaign Messages**; targets included/excluded lists/segments. |
| Journey | **Flow** | Triggered by a metric (e.g. "Placed Order") or list/segment join; has time-delay + email actions. |
| Trigger | Flow **trigger** | Metric trigger or list/segment-join trigger. |
| Personalization | `{{ first_name|default:'there' }}` | Django-style. Always give a `default:`. |
| Sender | Account **sender** (verified from-address) | Must already be verified in the Klaviyo account; we don't create it. |

## A. One-shot campaign

1. **Resolve audience** — read existing Lists and Segments; have the user pick the target (and any
   exclusions). If the brief specified a condition and the MCP supports segment-create, offer to create
   the segment; otherwise ask the user to pick an existing one. **Confirm the exact audience + size.**
2. **Create the Template** — create a Klaviyo Template from the approved HTML (one per email).
3. **Create the Campaign** (status stays **draft**):
   - channel = email
   - audiences = included list/segment (+ exclusions)
   - send strategy = leave **unscheduled** (or set the brief's send time as a *schedule*, not an immediate send)
4. **Set the message content** — on the campaign's message, set subject, preview text, from-name/from-email
   (a verified sender), and assign the Template (or inline the HTML).
5. **STOP.** Do **not** create a send job. The campaign now sits as a draft in Klaviyo.
6. **Report** the campaign ID + dashboard link, the audience, and recipient count.

> Sending in Klaviyo = creating a **send job** (a separate action). Never do this without an explicit,
> audience-confirmed go-ahead from the user.

## B. Journey (flow)

Klaviyo Flows are triggered automations (metric or list/segment join) with delays and email steps.

1. **Resolve the trigger** — map the brief's trigger to a Klaviyo metric (e.g. signup → list-join;
   abandoned cart → "Started Checkout"; post-purchase → "Placed Order").
2. **Create each email's Template** from the approved HTML.
3. **Build the Flow** — if the connected MCP supports flow-create: create the flow on its trigger, add
   each email action with its delay and the matching template, and leave it **manual/paused** (not live).
4. **If flow-create is NOT supported** (common — Klaviyo's flow-build is often UI-only via the API/MCP):
   - Create all the Templates (so the content exists in Klaviyo), then
   - Hand the user an **ordered, click-by-click guide** to assemble the flow in Klaviyo's Flow Builder:
     trigger → email (pick template) → time delay → next email → … → exit/smart-sending settings.
   - Clearly state which parts you automated (templates) and which are manual (flow wiring).
5. **Report** template IDs/links + the manual steps remaining. Leave the flow **paused** if created.

## Compliance (Klaviyo specifics)

- Every email must include an unsubscribe link — use Klaviyo's `{% unsubscribe %}` tag (or its
  managed unsubscribe footer). Confirm the org's mailing address is set in account settings.
- Respect **Smart Sending** (Klaviyo's dedup/quiet-hours) — leave it on unless the user opts out.
- The from-address must be a **verified sender** in the account; if none exists, flag it — don't invent one.

## Never-send checklist

- [ ] Campaign created with status = draft (no send job created)
- [ ] Flow left paused / not set live
- [ ] Audience + recipient count shown and confirmed before any send is even discussed
- [ ] Final action handed back to the user with the Klaviyo link to review and send themselves
