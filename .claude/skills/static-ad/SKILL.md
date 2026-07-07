---
name: static-ad
description: "Static ad images from a brand kit. Concept → optional synthesized foundation ref → final ad image rendered with gpt-image-2-image-to-image. Variable count, variable aspect ratio per ad, optional reference to 1+ of 40 catalogued ad formats."
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task, mcp__kie__kie_post, mcp__kie__kie_get, mcp__kie__kie_upload_file, mcp__kie__kie_download, mcp__kie__kie_fetch_model_docs, mcp__ffmpeg__shell_run, mcp__ffmpeg__download
---

# Recipe: static-ad

**Version:** v1

Produces one or more static ad images for a campaign — clean product hero, social-proof card, comparison split, faux UGC screenshot, etc. The brand kit supplies product truth; the catalogue at `ad-types.md` supplies 40 reference ad formats the agent can mix into the plan; gpt-image-2-image-to-image does the rendering. Invoke when the user says *"make me a static ad"*, *"build me a campaign"*, or *"I want a social ad set"*.

You (the Director) follow these steps in order. Each step has a gate — never skip the gate.

---

## What this recipe produces

| Output | Format | Generator | Step |
|---|---|---|---|
| Locked brief | `brief.md` | `ad-director-kit-scripter` subagent | 1 |
| Concept plan (per-ad concept + optional ad-type ref + ratio) | `plan.md` | Director (you) writes directly | 2 |
| Foundation refs (optional, only when needed) | PNG | `ad-director-kit-prompt-craftsman` → gpt-image-2-image-to-image | 3 |
| Final static ad images | PNG | `ad-director-kit-prompt-craftsman` → gpt-image-2-image-to-image | 4 |
| Variants (optional — different ratio, alt headline) | PNG | `ad-director-kit-prompt-craftsman` → gpt-image-2-image-to-image | 5 |
| Curated final folder + gallery | PNG copies + `gallery.html` | `ad-director-kit-composer` (or Director) | 6 |

## Folder layout for one campaign

```
brands/<brand>/ads/<ad-slug>/
  brief.md
  plan.md
  generations.md            ← job board (per rules/kie-and-files.md §4)
  foundation/   + archive/  ← only if Step 3 runs; each file has a .gen.json sidecar
  ads/          + archive/  ← all final static ad generations + sidecars
  final/
    AD-A.png  AD-B.png …    ← curated copies of approved ads
    gallery.html
```

**Handle naming convention:**

- Foundation refs (Step 3): `F-<NAME>.png` — e.g. `F-PLATE-FAMILY.png`, `F-LIFESTYLE-CAFE.png`
- Final ads (Step 4): `AD-A.png`, `AD-B.png`, `AD-C.png` … one letter per distinct concept in the plan
- Variants (Step 5): `AD-A-9x16.png`, `AD-A-alt.png` — suffix describes the variation

Archive convention: `<step-folder>/archive/<YYYY-MM-DDTHH-MM>-<short-reason>/<filename>` plus a `WHY.md` in the same archive subfolder with the user's verbatim critique.

---

## Inherited conventions

This recipe follows the project-wide rules. Don't restate them.

- **Prompt-writing** → `.claude/rules/prompt-conventions.md` (anchor to refs · physics invariants verbatim · positive phrasing · scale-anchor · slot-legend · SCALE LOCK · NEVER SHOW)
- **KIE + files** → `.claude/rules/kie-and-files.md` (sidecar schema · submit/poll discipline · batching · job lifecycle + board · archive flow · ensure-URL)
- **Brand scope** → `.claude/rules/brand-scope.md` (PRODUCT-TRUTH authoritative · BRAND.md voice · `reference-map.json` for role→file · file scope · reference labels must match reality)
- **Director standing orders** → `CLAUDE.md` (read-before-acting · gate every artefact · sidecars mandatory · do not auto-poll · file scope · comms style · preflight)

---

## Step 0a — Preflight

Standard preflight per `CLAUDE.md` (MCPs reachable · brand exists · reference uploads lazy · memory loaded). Recipe-specific addition: verify `.claude/skills/static-ad/ad-types.md` and `.claude/skills/static-ad/references/` both exist and the references folder has 40 files. If either is missing, stop and tell the user to restore them.

## Step 0b — Ad folder setup

Propose an ad slug (kebab-case, descriptive of the campaign), confirm with the user, then create the folder structure:

```sh
mkdir -p brands/<brand>/ads/<slug>/{foundation,ads,final} \
         brands/<brand>/ads/<slug>/{foundation,ads}/archive
```

The `foundation/` folder is created even if Step 3 doesn't run — it's cheap and keeps layouts uniform across campaigns. If it ends up empty when the campaign is done, leave it.

---

## Step 1 — Brief

**Inputs:** user's campaign idea, brand context loaded in Step 0a.
**Run by:** `ad-director-kit-scripter`
**Output:** `brands/<brand>/ads/<slug>/brief.md`

Dispatch the scripter with the campaign idea. It returns a locked `brief.md` covering:

- **Campaign goal** — what the ad set is trying to do (drive trial, reactivate cart abandoners, launch a new SKU, etc.)
- **Audience** — who these ads target (one paragraph; pulled from `BRAND.md` if not user-specified)
- **Product hook** — the one true thing about the product the campaign leans on (pulled from `PRODUCT-TRUTH.md` + the user's angle)
- **Tone notes** — anything sharper than `BRAND.md` defaults
- **Output set** — how many ads, what aspect ratios, where they're running (Meta / TikTok / display / etc.)
- **No-go list** — claims, words, visual treatments to avoid for this campaign

**Gate:** user approves the brief before Step 2 starts.

---

## Step 2 — Concept plan

**Inputs:** approved `brief.md`, `ad-types.md` catalogue, brand kit.
**Run by:** Director (you)
**Output:** `brands/<brand>/ads/<slug>/plan.md`

For each ad in the output set, draft a concept entry. Read `ad-types.md` and decide whether referencing one of the 40 catalogued formats would strengthen the concept — sometimes yes (a "before/after UGC" or a "comparison grid" is exactly the right format); sometimes no (the concept is too specific to the brand and a generic format would dilute it). If the user wants to browse, paste the catalogue summary (titles + "when to use") and let them pick.

Each concept entry in `plan.md` records:

```markdown
## AD-A — <short concept name>
- **Format:** <free-form description, OR reference one of the 40 e.g. "16. Curiosity Gap / Hook Quote Testimonial">
- **Aspect ratio:** <1:1 | 4:5 | 9:16 | 16:9>
- **Headline / copy beats:** <verbatim copy that should render in the image>
- **Foundation ref needed?** <yes — describe the synthesized scene | no>
- **Brand refs to feed:** <list role names from reference-map.json — e.g. `plate-finish-macro`, `plate-mounted-portrait`>
- **Notes:** <anything the prompt-craftsman needs to know — mood, props, what NOT to show>
```

**Foundation-ref decision rule:** if the concept needs a product/scene image that doesn't exist in the brand kit and can't be approximated by combining existing brand refs (e.g. *"a metal plate showing a specific family photo"*, *"the product in a cafe setting with a barista"*), mark it `yes` and add a Step 3 entry for it. If everything the prompt needs is already a brand ref or generated via composition inside the final ad prompt itself, mark `no`.

**Gate:** user approves the plan before any generation runs. They can re-order, swap formats, drop ads, or rewrite copy beats.

---

## Step 3 — Foundation refs (optional, only for concepts that need them)

**Inputs:** concepts in `plan.md` with `Foundation ref needed? yes`.
**Run by:** `ad-director-kit-prompt-craftsman` → `gpt-image-2-image-to-image`
**Output:** `brands/<brand>/ads/<slug>/foundation/F-<NAME>.png` + sidecar

**Model: `gpt-image-2-image-to-image`** (locked — same model as Step 4 so foundation refs and final ads stay visually consistent).

For each foundation ref needed, dispatch the prompt-craftsman with:
- The brand refs the foundation needs (resolved through `reference-map.json` + the ensure-URL flow in `rules/kie-and-files.md §6`)
- The concept's description of what the synthesized image should depict
- Whatever physics from `PRODUCT-TRUTH.md` / `PRODUCT-SPEC.md` applies (inject verbatim)

Use the slot-legend convention (`rules/prompt-conventions.md §5`) whenever a foundation ref takes more than one input image.

If multiple foundation refs are needed across the campaign, submit them all in one batch per `rules/kie-and-files.md §3`. Then STOP and surface the board (`generations.md`). Status checks happen on user demand.

**Gate:** user approves each foundation ref before it's used in Step 4. Reject → archive flow (`rules/kie-and-files.md §5`).

---

## Step 4 — Final static ad image

**Inputs:** approved `plan.md`, approved foundation refs (if any), brand kit refs.
**Run by:** `ad-director-kit-prompt-craftsman` → `gpt-image-2-image-to-image`
**Output:** `brands/<brand>/ads/<slug>/ads/AD-<X>.png` + sidecar

**Model: `gpt-image-2-image-to-image`** (locked).

For each concept (AD-A, AD-B, …), dispatch the prompt-craftsman with the inputs assembled per this slot pattern:

```
REFERENCE IMAGE SLOTS (array order):
Image 1 = <primary product or Step 3 foundation ref>
Image 2 = <ad-type reference from references/NN-slug.png, if the concept uses one>
Image 3+ = <additional brand refs — actor, lifestyle, packaging back, etc.>
```

Slot order MUST match the URL array order. Use plain `Image 1`, `Image 2`, `Image 3` references inside the prompt body (no `@` prefix — see `rules/prompt-conventions.md §5`).

The prompt body adapts the source template from `ad-types.md` (if one is used). **Do not paste the source template verbatim** — it's written for single-ref prompting. Rewrite as a slot-legend prompt, inject `PRODUCT-TRUTH.md` physics verbatim, replace every `[PLACEHOLDER]` with concrete values from `plan.md`, and add SCALE LOCK / NEVER SHOW blocks where the shot needs them.

Pass the `aspect ratio` from the concept entry as the model's `size` param (e.g. `2048x2048` for 1:1, `1024x1280` for 4:5 — fetch the exact accepted sizes via `kie_fetch_model_docs` before the first call this session).

Submit all approved concepts in one batch per `rules/kie-and-files.md §3`, persist taskIds to sidecars, surface the board, STOP.

**Gate:** user approves each ad before it lands in `final/`. Reject → archive flow.

---

## Step 5 — Variants (optional)

**Inputs:** an approved ad from Step 4, the user's variant request.
**Run by:** `ad-director-kit-prompt-craftsman` → `gpt-image-2-image-to-image`
**Output:** `brands/<brand>/ads/<slug>/ads/AD-<X>-<suffix>.png` + sidecar

When the user wants the same concept in a different aspect ratio (`AD-A-9x16.png`), with a different headline (`AD-A-alt.png`), or with a tone shift, this is a small-delta regen: take the original Step 4 prompt, edit only what changed, submit a fresh task. The variant gets its own sidecar — lineage lives in the handle suffix, not in a field.

**Gate:** user approves each variant.

---

## Step 6 — Final folder

**Inputs:** all approved ads + variants from Steps 4–5.
**Run by:** `ad-director-kit-composer` (or Director directly — this step has no ffmpeg work, just file ops + an HTML page)

For each approved final from `ads/`, copy it to `final/` keeping its handle name. Then write `final/gallery.html` — a single self-contained page that shows every final at its rendered ratio, with the handle name, the ad-type used (if any), and the headline copy underneath each. This is what the user shows the client / saves to disk for handoff.

**Gate:** user reviews the gallery. If they want to swap any ad in/out of the final set, run Step 5 or re-run Step 4 for that handle.

---

## Done criteria

- `brief.md` and `plan.md` are user-approved
- Every approved ad lives in `ads/<ad-slug>/final/` with its handle name
- `final/gallery.html` exists and renders every final at its rendered ratio
- Every generated file (foundation + final + variant) has a `.gen.json` sidecar (5 fields per `rules/kie-and-files.md §1`)
- Every reject lives in an `archive/<ts>-<reason>/` folder with `WHY.md`

---

## Model defaults (v1)

| Step | Model | Why |
|---|---|---|
| Foundation refs (Step 3) | `gpt-image-2-image-to-image` | Locked. Strong multi-reference composite + clean rendering of fine text, logos, packaging labels. Same model as Step 4 so refs + finals stay visually consistent. |
| Final static ad (Step 4) | `gpt-image-2-image-to-image` | Locked. Required for legible headlines, review-card UI, faux-app screenshots, ticker bars, and all the other text-heavy formats in the catalogue. |
| Variants (Step 5) | `gpt-image-2-image-to-image` | Same model as the original it varies from. |
| Final gallery (Step 6) | none — file copy + static HTML | Deterministic. |

Aspect ratio is **never** baked into the model lock — it comes from the per-concept entry in `plan.md` and is passed via the model's `size` param. Fetch the accepted sizes via `kie_fetch_model_docs` once per session before the first Step 4 call.

---

## Recipe-specific overrides

**The 40 source templates in `ad-types.md` are starting points, not paste-ready prompts.** They were written assuming a single-reference, "use the attached images as brand reference" prompting pattern. To use one in this recipe:

1. Read its source template from `ad-types.md`.
2. Identify which `[PLACEHOLDERS]` map to brand kit refs vs. literal text vs. concept-specific values from `plan.md`.
3. Rewrite the prompt with the slot-legend block at the top (`rules/prompt-conventions.md §5`) — explicitly bind Image 1 = product/foundation, Image 2 = the ad-type reference photo, Image 3+ = additional brand refs.
4. Inside the prompt body, reference each slot by number (`Image 2`), not by description.
5. Inject the relevant physics block from `PRODUCT-TRUTH.md` / `PRODUCT-SPEC.md` verbatim.
6. Add SCALE LOCK + NEVER SHOW where the shot warrants them.
7. The catalogue's suggested aspect ratio is just a default — override with what the campaign asked for.

**Catalogue browsing:** if the user asks *"what are my options?"* during Step 2, you can list the 40 titles with their "When to use" lines (compact summary), or paste 3–5 entries with their reference image paths so they can see the visual format before picking.

---

## Gating rules (one-line summary)

Every brief, every plan, every generated artefact (foundation, ad, variant), and the final gallery is a gate. **No silent generation.** Rejects never delete — they go to `archive/<ts>-<reason>/` with a `WHY.md`. See `rules/kie-and-files.md §5` for the archive flow.
