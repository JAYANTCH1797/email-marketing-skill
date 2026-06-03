# Email Marketing — Claude plugin

End-to-end email marketing inside Claude: take a brief, decide one-shot vs. journey, design
on-brand emails, sharpen the copy, and push **reviewable drafts** into whatever email CLM you've
connected (Klaviyo, Mailchimp, HubSpot, or any email MCP). Tool-agnostic over the CLM. **Never auto-sends.**

## What's inside

| Component | File | Role |
|---|---|---|
| **email-campaign** skill | `skills/email-campaign/SKILL.md` | The front door / orchestrator — owns the full lifecycle, brief → CLM |
| **email-templates** skill | `skills/email-templates/SKILL.md` | Designs a single on-brand, Outlook-safe HTML email (6 use-case recipes) |
| **brand-kit-extractor** agent | `agents/brand-kit-extractor.md` | Scrapes a brand site → structured brand kit (isolated context) |
| **copy-critic** agent | `agents/copy-critic.md` | Independent copy review/rewrite + subject A/B variants |
| **/email-campaign** command | `commands/email-campaign.md` | Entry point that kicks off the workflow |
| **send-guard** hook | `hooks/hooks.json` | Forces a confirmation prompt on send/activate tool calls |

## How it composes (lazy, one source of truth per capability)

```
/email-campaign ─▶ email-campaign skill (process)
                      │  Phase 5 design  ─▶ email-templates skill
                      │                        └─ brand kit ─▶ brand-kit-extractor agent
                      │                        └─ imagery   ─▶ image-generation skill (companion)
                      │  Phase 6 copy    ─▶ copy-critic agent
                      └─ Phase 7 deliver ─▶ connected CLM MCP (drafts only)
```

- **email-campaign** is brand-agnostic until the design phase, and defers all CLM/MCP calls to the end.
- **email-templates** resolves the brand kit lazily: installed `*-design-system` skill → `brand-kit-extractor` agent (scrape) → ask.
- Brand-specific design systems (e.g. an `inito-design-system` skill) live **outside** this plugin and are auto-discovered.

## Companion skills (recommended, not bundled)

- An **image-generation** skill for hero/lifestyle/illustration assets.
- A per-brand **design-system** skill if you have one (optional — scraping is the fallback).

## Install / test locally

```bash
# from the directory containing this plugin folder
claude --plugin-dir ./email-marketing
# (v2.1.128+ also accepts a .zip directly)
```

## Distribute

Zip the `email-marketing/` folder, or push to GitHub and list it in a marketplace
(`.claude-plugin/marketplace.json`).

Repo: https://github.com/jayaNTCH1797/email-marketing-skill

## Safety

Every phase ends in a confirmation gate. Phase 7 creates **drafts / paused flows only**; sending or
activating requires an explicit, audience-confirmed go-ahead. The send-guard hook is a backstop —
tune its matcher in `hooks/hooks.json` to your CLM's exact tool names.
