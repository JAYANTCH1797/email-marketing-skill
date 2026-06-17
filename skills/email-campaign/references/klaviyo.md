# Phase 7 — Klaviyo playbook

Concrete steps for delivering into **Klaviyo** when it's the connected CLM. This is the
provider-specific expansion of the email-campaign skill's Phase 7. The generic Phase 7 rules still
apply: **defer to the end, create drafts only, never send/activate without explicit go-ahead.**

> **Real Klaviyo MCP tools** (confirm against the connected server's schema before each call):
> - Audience: `get_lists`, `get_list`, `get_segments`, `get_segment`
> - Templates: `list_email_templates`, `get_email_template`, `create_email_template`, `update_email_template`, `clone_email_template`, `create_dnd_email_template`, `render_email_template`
> - Campaign: `create_campaign`, `assign_template_to_campaign_message`, `get_campaign(s)`
> - Images: `upload_image_from_url` (host generated/edited images to get a stable URL)
> - Profiles/metrics (for segments/flows): `get_profiles`, `get_metrics`, `get_flows`
> There is **no send tool here** — sending is a separate send-job action; never trigger it from this skill.

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

## Reuse the brand's header/footer + host images (do this during design, before delivery)

- **Header/footer:** call `list_email_templates`, then `get_email_template` on the brand's existing
  campaigns and **lift the header (logo lockup, nav) and footer (social, legal/address, unsubscribe)**.
  These are standardized per brand — reusing them keeps consistency and gets the legally-required footer
  right. Build new ones from the brand kit only if no template exists. `clone_email_template` is the
  fastest way to start from a known-good brand template and swap the body.
- **Image hosting:** every generated/edited/user image must be a stable URL. `upload_image_from_url`
  hosts it on Klaviyo's CDN and returns a durable link to drop into the `<img src>` — do this instead of
  inlining base64 (Gmail clips >102KB).

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
