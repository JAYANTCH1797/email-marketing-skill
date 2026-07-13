# Design intake — the guided 3-axis questionnaire

> The **front door to building any email.** Before generating a single image or line of HTML, walk
> the user through the three design axes **one at a time**, plus the image-source decision. This is
> what turns "make me an email" into a deliberate, on-brand, reproducible build.
>
> Axes (each has its own reference):
> 1. **Emailer Type** → `email-types.md`
> 2. **Template Structure** → `template-structures.md`
> 3. **Hero Image Structure** → `layout-patterns.md`
> 4. **Image Source** (cross-cutting) → below
>
> Implemented as **separate `AskUserQuestion` rounds** (guided per-axis), each pre-seeded with the
> recommended default from the type taxonomy so the user confirms or overrides with full context.

## When to ask (the ask rule)

**Ask the axes whenever the user has NOT provided design guidelines.** In practice this is *almost
always* — only skip an axis when its answer is genuinely pinned, namely:

- An **installed brand / design-system skill** (e.g. `*-design-system`) fixes structure/hero conventions, **or**
- The user **explicitly stated** the choice ("use a plain text-only welcome", "no images", "use our sale template"), **or**
- The type is **unambiguously transactional** (receipt/OTP/shipping) — structure + hero are fixed by the trust genre; still confirm the image-source = none and the facts.

If none of those apply to an axis, **ask it.** Never silently pick structure or hero for a marketing
email. When in doubt, ask. (Detecting the *type* to seed defaults is fine and expected — but still
**confirm** it in Step A.)

## Sequencing

```
detect type ─▶ A. confirm Emailer Type ─▶ B. choose Template Structure
            ─▶ C. choose Hero Image Structure ─▶ D. choose Image Source
            ─▶ lock copy ─▶ assemble the BLUEPRINT (email-design.md Step 2.5) ─▶ get sign-off ─▶ build
```

Run A→D as four short, sequential `AskUserQuestion` rounds (not one mega-screen) so each choice is
made with the previous ones visible. Every round: **recommended option first, labelled "(Recommended)"**,
2–3 real alternates from the references, and the implicit "Other" for a free-text answer.

---

## Step A — Emailer Type

**Goal:** confirm what kind of email this is, because it seeds the next two axes.
**Seed:** detect the row from `email-types.md` ("Type detection").

> **AskUserQuestion** — header `Email type`
> *question:* "What kind of email is this? I detected **{detected type}** ({genre} · {occasion})."
> *options:* the detected type (Recommended) + the 2–3 nearest occasions in the same genre + "A different genre" → if chosen, follow up with the genre list.
> Each option's description = the one-line occasion description from `email-types.md`.

Carry forward the chosen type's **default structure**, **default hero**, **text strategy**, and
**image-source** as the seeds for Steps B–D.

## Step B — Template Structure

**Goal:** choose the body skeleton.
**Seed:** the type's default structure key from `email-types.md` → look up its sequence + alternates in `template-structures.md`.

> **AskUserQuestion** — header `Structure`
> *question:* "How should the body be structured? Recommended for a {type}: **{default-structure-key}** — {plain-English block list}."
> *options:*
> 1. `{default-structure-key}` **(Recommended)** — *description:* the readable block sequence (e.g. "sale hero → product grid → urgency strip").
> 2.–3. the structure's **alternates** (each with its readable sequence).
> 4. (only if relevant) "Minimal / shorter" — drop optional blocks.
>
> *preview (optional but encouraged):* render the block sequence as a vertical labelled outline so the user sees the skeleton.

If the user wants something outside the menu → take their free-text, map it onto the closest blocks
in `layout-patterns.md`, and if it's a genuinely new pattern, note it for later addition to `template-structures.md`.

## Step C — Hero Image Structure

**Goal:** choose the hero archetype **and** how text + image combine.
**Seed:** the type's default hero archetype + text strategy (A/B/C/CSS) from `email-types.md`; full menu in `layout-patterns.md`.

> **AskUserQuestion** — header `Hero`
> *question:* "What should the hero (the top moment) be? Recommended: **{archetype}** with {strategy explained}."
> *options:*
> 1. `{default-archetype}` **(Recommended)** — *description:* one-line from `layout-patterns.md` + the text strategy in plain words (e.g. "photo with the headline baked onto it").
> 2.–3. the **alternates** from that genre-row (e.g. `image-only`, `color-block`).
> 4. "No hero image (CSS-only)" — when they want maximum reliability / a text-forward email.

**Then resolve the text strategy** if the chosen archetype supports more than one (image-bearing
archetypes → A/B/C). Make the trade-off explicit — this is the readability decision:

| | Strategy | Plain-English | Trade-off |
|---|---|---|---|
| **A** | bake headline into the image | "the words are part of the picture" | most striking; not editable/translatable/selectable; needs legibility check; **generated images only** |
| **B** | live text over a plain image | "real text sits on top of the photo" | accessible + localizable; image needs an empty dark safe-zone |
| **C** | no overlay | "headline is below the photo" | safest; best deliverability; less 'campaign-y' |

> Default to the type's strategy; **if in doubt choose C.** Only choose **A** when the image is
> generated (so composition is controllable) AND the copy is final (Step "lock copy").

## Step D — Image Source

**Goal:** decide where every image comes from — **AI-generated vs. user-provided** are the two main paths.
**Seed:** the type's image-source default from `email-types.md` (e.g. transactional → none; D2C lifestyle → generate; hardware/travel/fintech → user-asset).

> **AskUserQuestion** — header `Images`
> *question:* "Where should the images come from?"
> *options:*
> 1. **Generate with AI** — *description:* "I'll create the hero (and any lifestyle/illustration images) with the image-gen skill, on-brand." (Recommended when no assets + genre suits generated imagery: D2C, ecommerce lifestyle, SaaS illustration, editorial.)
> 2. **I'll provide the images** — *description:* "Use your product shots / brand photography / logo. Best for real products, and **required** for transactional, travel, and fintech credibility." → then ask for the URLs/uploads.
> 3. **Mix** — *description:* "Generate the hero/background; use my real product images in the grid." (or vice-versa)
> 4. **No images (CSS-only)** — *description:* "Build the hero from type + brand color. Most reliable; great for flash promos, receipts, founder notes."

Rules that follow from the choice:
- **Real assets beat generated** for any product the user actually sells, and are **mandatory** for transactional / travel / fintech (real catalog/property photos or none — never generated lifestyle, which erodes trust + trips spam).
- **Strategy A (baked text)** requires **Generate** (or Mix where the hero is generated) — you can't reliably bake legible text onto a user photo without an editing pass.
- For **Strategy B** over a user-provided photo, confirm it has a usable empty safe-zone + contrast; if not, edit one in or fall back to C.
- Every image — generated or provided — ends as a **hosted HTTPS URL** (email-design.md Step 6 "Hosting"); never inline base64.

## Lock copy before generating (gate between D and the blueprint)

Decide the **headline and every body line** before generating any image — so:
- the hero never duplicates body copy (dedup check), and
- for **Strategy A**, the baked text matches the final headline exactly.

## Then: assemble the blueprint and get sign-off

Feed all four answers into the **EMAIL BLUEPRINT** (email-design.md Step 2.5) — type / structure / hero +
text strategy / image plan per slot / dedup check / readability spec — and **get explicit approval
before building.** For journeys, blueprint all emails together and vary structure + hero per email.

## Fast-path exceptions (don't over-ask)

- **User already gave full design direction** → skip the matching axes; confirm in one line and proceed.
- **Installed brand/design-system skill** → it wins on structure/hero/colors; only ask what it leaves open (often just Image Source + copy).
- **Pure transactional** → structure + hero fixed; confirm image-source = none and the facts, skip A–C.
- **Journey of N emails** → ask the axes **once for the set** (with per-email hero variation), not N times.

## Worked example (D2C supplement launch, no assets)

```
detect → D2C · Product launch  → seeds: d2c-launch / product-cutout / C / generate
A Email type:   "D2C product launch?"            → ✓ confirmed
B Structure:    d2c-launch (Recommended)         → user picks d2c-launch
                  hero → benefits → social proof → upsell
C Hero:         product-cutout (Recommended)     → user switches to image-only (lifestyle)
                  text strategy C (headline below) → kept
D Images:       Generate with AI (Recommended)   → user picks Generate
lock copy → blueprint → sign-off → build
```
