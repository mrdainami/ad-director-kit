---
name: onboard-brand
description: "Conversational skill for setting up a brand-new brand in this ad-director-kit project. Use when the user says they want to add their brand, or when preflight finds the `brands/` folder empty. Creates the brand folder structure, asks the user about their product / audience / tone / links / what AI commonly gets wrong, and writes BRAND.md + PRODUCT-TRUTH.md from the conversation."
allowed-tools: Read, Write, Edit, Glob, Bash, mcp__ffmpeg__shell_run
---

# Skill: onboard-brand

Use this skill when a user wants to add a new brand to this ad-director-kit project. It produces a brand kit complete enough that the `seedance-storyboard` recipe can run against it.

## Inputs you collect by chat

1. **Brand slug** — kebab-case, short, no spaces. (Suggest one from the brand name; confirm with user.)
2. **Brand name** + tagline / one-line positioning.
3. **Category** — what kind of product (physical product, digital, service, etc).
4. **Audience** — who buys it; primary + secondary if relevant; geography.
5. **Tone of voice** — 2–3 sentences. What it sounds like, what it avoids.
6. **Funnel positioning** — TOFU / MOFU / BOFU jobs if the brand thinks in those terms.
7. **Price / offers** — price points, any standing promos.
8. **Links** — site, social handles.
9. **Product photos** — confirm the user has dropped them into `brands/<slug>/references/` (organise into component subfolders — e.g. `references/<part>/` — or flat for a simple product).
10. **Logo** — confirm a brand-mark file is in `brands/<slug>/logo/`.
11. **What's special about this product for AI generation** — what AI commonly hallucinates wrong about it, what physical details must be locked. (This becomes `PRODUCT-TRUTH.md`.)
    - Materials, surface finish, edges, joints
    - Mechanism (how it works, opens, mounts, attaches)
    - Sizes / proportions
    - Colors / typography on the product itself
    - What it is NOT (common AI mistakes)

## Sequence

### 1. Set up the folder

```sh
mkdir -p brands/<slug>/references brands/<slug>/{logo,notes,ads}
```

(Run via `mcp__ffmpeg__shell_run`.)

### 2. Ask the user to drop in their assets

> "Drop your product photos into `ad-director-kit/brands/<slug>/references/` (group them into component subfolders, or flat for a simple product) and your logo into `ad-director-kit/brands/<slug>/logo/`. Tell me when done."

Wait for confirmation. Then `ls` the folders and report what you see. If `references/` is empty or `logo/` is empty, push back — the recipe needs at least a few reference photos and one logo.

### 3. Ask the brand-card questions (one or two at a time, conversational)

Don't dump all 11 items as a form. Walk through it like a discovery call:
- "Tell me about your brand in one or two sentences."
- "Who buys this? Walk me through your typical customer."
- "How would you describe the voice? Any words or styles you avoid?"
- "What's the standing offer / price point?"
- "Site + socials?"
- "Last big one — what does AI usually get wrong about your product? Material, mechanism, sizes, anything visual?"

If the user gives vague answers, probe one level deeper before moving on.

### 4. Write `brands/<slug>/BRAND.md`

Use this structure:

```markdown
# <Brand name> — Brand Card

> Plain-English brand context for the Director and specialist agents. Pairs with `PRODUCT-TRUTH.md` (the physical truth) and the photos in `references/` / `logo/`.

## Who they are
<1–2 paragraphs from the user's answers>

## Audience
<Primary paragraph + optional secondary>

## Tone
<Bullet list of voice traits + things to avoid>

## Funnel positioning (campaign-level)
<Table or paragraphs by stage, if the brand thinks in those terms; skip if not>

## Price + offers
<bullets>

## Links
- Site:
- IG / TikTok / etc.:

## What's special about this brand for AI generation
<2–4 paragraphs summarising the AI-hallucination gotchas; the detail lives in PRODUCT-TRUTH.md>

## Brand mark
<path to the logo file in logo/>
```

### 5. Write `brands/<slug>/PRODUCT-TRUTH.md`

Use this skeleton and adapt the sections to the user's product. Common sections:

- **Identity** — product, category, market, price anchor
- **Physical anatomy** — per major part (e.g. "the plate", "the bracket"): surface, dimensions, key visual truth, photo anchor filenames, common AI hallucinations
- **Sizes / variants** — if applicable
- **Mechanism** — how the product works, mounts, opens, attaches
- **Tone & brand voice** — short recap
- **Physics invariants** — bullets of "must NEVER appear" (e.g. visible screws, wrong colour, wrong proportion)
- **How to use this file** — instructions to agents reading it later

The detail level should match the user's product. A simple product (e.g. a candle) may need a 100-line PRODUCT-TRUTH; a complex product with multiple parts (e.g. a furniture system with brackets, panels, fittings) may need 300+ lines.

If the user wants a deeper version with action-realism libraries, offer to generate a `PRODUCT-SPEC.md` as a follow-up. Most brands don't need it.

### 5b. Write `brands/<slug>/reference-map.json`

Build the role → file manifest the recipe uses to pick references (this replaces any hardcoded folder lookups). For each reference role the product needs, map it to its file under `references/`, and record `provenance` (`real` | `aigen` | `unknown`) per role. A simple product may have ~3 roles; a complex one 15+. Shape: `{ "roles": { "<role-name>": { "file": "references/<file>", "provenance": "real" }, ... } }`.

### 6. Show, gate, refine

Present BRAND.md and PRODUCT-TRUTH.md back to the user in chat (paste the file paths + a summary of each section). Ask for corrections. Edit until they approve.

### 7. Run preflight against the new brand

Once approved, run the `seedance-storyboard` preflight against the new brand (verify the asset URL map covers it, etc.). Confirm everything's ready and tell the user they can now make ads:

> "Brand '<slug>' is set up. To make an ad, say something like: 'Make an ad for <slug>. Idea: …'."

## What this skill does NOT do

- Does not write or modify any recipe / agent / system file
- Does not generate ads (that's `seedance-storyboard`)
- Does not create or edit other brands
- Does not pre-generate any AI imagery — brand kit is human-written + photo refs only

## Anti-patterns

- **Don't write generic copy.** If the user says their tone is "modern and friendly", probe: what does that mean *for them*? Examples? What it sounds like vs. what it doesn't. Generic answers produce generic ads.
- **Don't skip PRODUCT-TRUTH.** It's the most load-bearing file in the whole project. AI prompt outputs degrade fast without it. If the user is reluctant, walk them through ONE part of their product in detail to model how it's done.
- **Don't invent facts.** If the user doesn't know an answer (e.g. exact dimensions), say "not specified" in the file rather than guess. PRODUCT-TRUTH is authoritative; wrong facts there poison every downstream prompt.
