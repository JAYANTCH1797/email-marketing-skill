# Email Marketing — Claude plugin

End-to-end email marketing inside Claude: take a brief, decide one-shot vs. journey, design
on-brand emails, sharpen the copy, and push **reviewable drafts** into whatever email CLM you've
connected (Klaviyo, Mailchimp, HubSpot, or any email MCP). Tool-agnostic over the CLM. **Never auto-sends.**

## What's inside

| Component | File | Role |
|---|---|---|
| **email-campaign** skill (the one skill) | `skills/email-campaign/SKILL.md` | The front door / orchestrator — owns the full lifecycle, brief → design → CLM |
| **email-design** reference | `skills/email-campaign/references/email-design.md` | The design engine (brand kit, 3-axis intake, 6 recipes, image pipeline), loaded at the design phase |
| 3-axis references | `.../references/{email-types,template-structures,layout-patterns,intake-questions}.md` | Emailer type · template structure · hero archetype library · guided intake |
| Klaviyo reference | `.../references/klaviyo.md` | Concrete Phase-7 delivery playbook |
| **brand-kit-extractor** agent | `agents/brand-kit-extractor.md` | Scrapes a brand site → structured brand kit (isolated context) |
| **copy-critic** agent | `agents/copy-critic.md` | Independent copy review/rewrite + subject A/B variants |
| **email-structure-researcher** agent | `agents/email-structure-researcher.md` | Derives a new hero/structure when the library doesn't cover the case |
| **/email-campaign** command | `commands/email-campaign.md` | Entry point that kicks off the workflow |
| **send-guard** hook | `hooks/hooks.json` | Forces a confirmation prompt on send/activate tool calls |

> **One skill, references for depth.** The plugin exposes a **single skill** (`email-campaign`); everything else is a reference it loads on demand (progressive disclosure) or an agent it invokes.

## How it composes (lazy, one source of truth per capability)

```
/email-campaign ─▶ email-campaign skill (the whole lifecycle)
                      │  Phase 5 design  ─▶ references/email-design.md (3-axis intake → blueprint → build)
                      │                        └─ brand kit ─▶ brand-kit-extractor agent
                      │                        └─ imagery   ─▶ image-generation skill (companion)
                      │                        └─ new structure ─▶ email-structure-researcher agent
                      │  Phase 6 copy    ─▶ copy-critic agent
                      └─ Phase 7 deliver ─▶ connected CLM MCP (drafts only)
```

- **email-campaign** is brand-agnostic until the design phase, and defers all CLM/MCP calls to the end.
- The **email-design** reference resolves the brand kit lazily: installed `*-design-system` skill → `brand-kit-extractor` agent (scrape) → ask.
- Brand-specific design systems (e.g. an `inito-design-system` skill) live **outside** this plugin and are auto-discovered.

## Companion skills (recommended, not bundled)

- An **image-generation** skill for hero/lifestyle/illustration assets.
- A per-brand **design-system** skill if you have one (optional — scraping is the fallback).

## Install

**From this GitHub marketplace (recommended):**

```
/plugin marketplace add jayaNTCH1797/email-marketing-skill
/plugin install email-marketing
```

**From a local checkout (for development):**

```bash
# from the directory containing this plugin folder
claude --plugin-dir ./email-marketing
# (v2.1.128+ also accepts a .zip directly: claude --plugin-dir ./email-marketing.zip)
```

Validate the manifests any time with:

```bash
claude plugin validate ./email-marketing
```

## Quickstart

Once installed, start from the command — or just describe what you want:

```
/email-campaign welcome series for gruns.co with a 10% first-order offer
```

The **email-campaign** skill drives the flow:

1. **Intake** — answers a few questions (or reads your brief). For a single email you just want
   *designed* (not delivered), it offers a fast path straight to the design engine.
2. **One-shot vs. journey** — single broadcast, or a triggered multi-email sequence.
3. **Brief** — a short written brief you approve.
4. **Structure** — the one-shot plan or the full journey map (sign-off gate).
5. **Design** — each email built on-brand via `references/email-design.md` (brand kit resolved by the
   **brand-kit-extractor** agent, or an installed `*-design-system` skill).
6. **Copy** — the **copy-critic** agent tightens copy and proposes subject A/B variants.
7. **Deliver** — drafts created in your connected CLM (Klaviyo / Mailchimp / HubSpot / any email MCP).

Nothing is ever sent automatically — Phase 7 produces **drafts / paused flows** only.

## Connect your email CLM

Delivery (Phase 7) uses whatever email MCP you've connected — the plugin detects it and maps
generic concepts (audience, email, one-shot, journey, trigger) onto that tool's primitives. Connect
your Klaviyo / Mailchimp / HubSpot MCP separately; this plugin doesn't bundle credentials.

After connecting, **tune the send-guard** in `hooks/hooks.json` to match your CLM's exact send/
activate tool names so the confirmation prompt fires precisely.

## Distribute

Zip the `email-marketing/` folder, or push to GitHub and install via the marketplace
(`.claude-plugin/marketplace.json`).

Repo: https://github.com/jayaNTCH1797/email-marketing-skill

## Safety

Every phase ends in a confirmation gate. Phase 7 creates **drafts / paused flows only**; sending or
activating requires an explicit, audience-confirmed go-ahead. The send-guard hook is a backstop —
tune its matcher in `hooks/hooks.json` to your CLM's exact tool names.
