---
name: seedance-storyboard
description: "Recipe for producing a vertical brand video Reel from a brand kit + an ad idea. Storyboard-first: brief → plan → foundation refs → multi-panel storyboards (typically 3–8 panels per segment) → Seedance video segments (3–15s each, variable count) → BGM (Suno, 2 variants) → ffmpeg compose with BGM bed + end-card. Use when the cowork user asks to make a brand ad with the seedance-storyboard recipe."
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task, mcp__kie__kie_post, mcp__kie__kie_get, mcp__kie__kie_upload_file, mcp__kie__kie_download, mcp__kie__kie_fetch_model_docs, mcp__ffmpeg__shell_run, mcp__ffmpeg__download
---

# Recipe: seedance-storyboard

**Version:** v1

Vertical Reel from a brand kit. Storyboard-first → video → compose with separate BGM bed. Variable duration, variable segment count.

You (the Director) follow these steps in order. Each step has a gate — never skip the gate.

---

## What this recipe produces

| Output | Format | Generator | Step |
|---|---|---|---|
| Locked brief | `brief.md` | `cowork-scripter` subagent | 1 |
| Recipe plan | `plan.md` | Director (you) writes directly | 2 |
| Foundation refs | PNG, variable count | `cowork-prompt-craftsman` → KIE image model | 3 |
| Storyboards (one per segment) | Multi-panel PNG (typically 3–8 panels per segment, agent picks N to match the segment's shot count) | `cowork-prompt-craftsman` → KIE image model | 4 |
| Segments | MP4 (3–15s each, 9:16, VO+SFX embedded, NO music) | `cowork-prompt-craftsman` → Seedance | 5 |
| BGM variants | MP3, 2 variants | `cowork-prompt-craftsman` → Suno | 6 |
| Final cut(s) | MP4 (total length variable, 9:16) | `cowork-composer` → ffmpeg | 7 |
| Gallery preview | HTML | `cowork-composer` | 7 |

## Folder layout for one ad

```
brands/<brand>/ads/<ad-slug>/
  brief.md
  plan.md
  refs/          + archive/      ← each generated file has a .gen.json sidecar
  storyboards/   + archive/
  segments/      + archive/
  music/         + archive/
  final/
    compose.json (or compose-<variant>.json)
    cut-<variant>.mp4
    gallery.html
```

Handle naming convention:
- Refs: `F-<HANDLE>.png` (e.g. `F-HERO.png`, `F-ROOM.png`)
- Storyboards: `SB-<SEGMENT>.png` (`SB-A.png`)
- Segments: `seg-<SEGMENT>.mp4` (`seg-A.mp4`)
- Music: `bgm-variant-<A|B>.mp3`
- Cuts: `cut-<variant>.mp4` (default `cut-default.mp4`)

Archive convention: `<step-folder>/archive/<YYYY-MM-DDTHH-MM>-<short-reason>/<filename>` plus `WHY.md` in the same archive subfolder with the user's verbatim critique.

---

## Step 0a — Preflight (run on every fresh session)

Before starting any work, verify the project is ready. **See CLAUDE.md preflight section for the canonical sequence.** Summary:

1. **MCPs reachable.** `mcp__kie__kie_get` no-op; `mcp__ffmpeg__shell_run "ffmpeg -version | head -1"`.
2. **Brand exists.** `brands/<brand>/{BRAND.md,PRODUCT-TRUTH.md}` readable; `references/` has ≥1 image; `logo/` has ≥1 image; `reference-map.json` present.
3. **Reference uploads are lazy.** No bulk upload. `.claude/agent-memory/asset-urls.json` is a per-file cache; each reference is uploaded to KIE on first use via `kie_upload_file` (see CLAUDE.md preflight §3 "ensure URL"). Nothing to do here at preflight beyond confirming the file is readable/parseable if it exists.
4. **Memory loaded.** Read everything in `.claude/agent-memory/` for prior cross-session decisions for this brand.

> KIE calls: the kie MCP's standing instructions cover submit/poll. Before calling a model whose payload you don't know this session, fetch its docs via `kie_fetch_model_docs`.

If anything fails, stop and tell the user exactly what to fix. Don't proceed.

If preflight passes, write `.claude/agent-memory/setup-done.md` with timestamp + brand list.

## Step 0b — Ad folder setup

Before dispatching Scripter, establish the ad's identity:

1. From the user's idea, **propose an ad slug** (kebab-case, descriptive, e.g. `02-what-makes-metal-print-different`). Sometimes a prefix number is useful for ordering; ask the user if they want one.
2. **Confirm the slug with the user.** Don't proceed silently.
3. **Create the ad folder structure** via `mcp__ffmpeg__shell_run`:
   ```sh
   mkdir -p brands/<brand>/ads/<slug>/{refs,storyboards,segments,music,final} \
            brands/<brand>/ads/<slug>/{refs,storyboards,segments,music}/archive
   ```
4. Pass the slug + folder path to Scripter in Step 1.

The folder owns the entire ad's working set. Once created, every artefact for this ad lands inside it.

---

## Step 1 — Idea → Brief

**Inputs:** user's ad idea (free-form chat), `BRAND.md`, this skill.
**Subagent:** dispatch `cowork-scripter` via the Task tool.
**Output:** `brands/<brand>/ads/<ad-slug>/brief.md`.

The brief format is specified in the Scripter's spec. Key requirement: it records `Recipe: seedance-storyboard (v1)` at the top so future re-runs are deterministic against this recipe version.

The brief **must** include a `## Voiceover voices` section that locks the voice descriptor(s) for the entire ad. Example:

```markdown
## Voiceover voices

**Voice 1 — Main narrator:** warm Singaporean female, 28–35, clear unhurried delivery, confident but approachable, no heavy Singlish particles, slight natural warmth.

_(If a second voice is needed, define it here the same way — e.g. "Voice 2 — Character: …")_
```

This section is the canonical source for every video segment prompt. The Scripter should derive it from the brief's tone/audience notes. Once approved at this gate, the descriptor(s) are **never rewritten** — only the lines and beat cues change per segment.

The brief **must** also include a `## Audio` section that locks the music decision for the whole ad. Two options:

```markdown
## Audio

**BGM:** handled separately via Suno (Step 6) — do NOT embed music in Seedance generation. `generate_audio: true` (VO + SFX only, no music).
```

or, if the ad has no voiceover and no Suno BGM step:

```markdown
## Audio

**BGM:** embedded in Seedance generation. `generate_audio: true`.
```

The Craftsman reads this section before writing every segment prompt. It sets the `generate_audio` value and whether `NO background music.` appears in the VO block. Never infer this from context — it must be explicit in the brief.

**Gate:** present the brief to the user. Approve → continue. Reject → Scripter rewrites with critique. Only proceed once the user explicitly approves.

---

## Step 2 — Brief → Plan

**Inputs:** `brief.md`, brand kit (`BRAND.md` + `PRODUCT-TRUTH.md` + `PRODUCT-SPEC.md` if it exists + `reference-map.json` + `references/` listing).
**Run by:** you (the Director) — no subagent. This is reasoning + a file write.
**Output:** `brands/<brand>/ads/<ad-slug>/plan.md`.

Plan structure:

```markdown
# <ad-slug> — Plan

**Recipe:** seedance-storyboard (v1)
**Source brief:** brief.md

## Per-segment storyboard intent

### Segment A (<X>s)
1. Shot 1 (~Ys) — <plain English what you see>; scene-state: <e.g. "bracket-installed-empty-wall">
2. Shot 2 …
… up to ~5 shots per segment …

### Segment B (<X>s) …
### Segment C (<X>s) …

## Reference picks per shot

| Shot | Existing reference (role → file via reference-map.json) | Generated `F-*` ref needed |
|---|---|---|
| A.1 | install-place-plate → references/install/install-place-plate-on-mount.png | — |
| A.2 | — | F-HERO (fresh hero photo) |
| … | … | … |

## Generated refs to produce (Step 3)

| Handle | Purpose | Source model |
|---|---|---|
| F-HERO | Fresh hero photo for the plate | nano-banana-pro |
| F-ROOM | Warm cream HDB living room composite | nano-banana-pro |
| … | … | … |

## Music brief (Step 6)

- Vibe: <e.g. warm acoustic, minimal>
- Instrumentation: <e.g. felt piano, soft strings>
- Energy curve: <e.g. quiet build to mid, gentle fade>
- Length: ~<total cut length>s
- Variants: 2 (default)

## Compose plan (Step 7)

- Segment order: A → B → C
- End-card duration: 3s
- End-card CTA (from brief): <text>
- Brand mark: <path>
- BGM bed level: -18 dB (default)
- Variants planned: 1 (default cut)
```

**Gate:** present plan to user. Approve → continue. Reject → revise plan.

---

## Step 3 — Generate foundation refs

**Inputs:** `plan.md` (the generated-refs list), reference photos (resolved by role via `reference-map.json`), `PRODUCT-TRUTH.md`, `PRODUCT-SPEC.md`, `.claude/agent-memory/asset-urls.json`.
**Subagent:** `cowork-prompt-craftsman` — **batched** (all refs in one wave, not one at a time).
**Batched flow** — see `.claude/rules/kie-and-files.md` §3 (batching) + §4 (job lifecycle):

1. Craftsman writes each ref's prompt (resolving image refs by role via `reference-map.json`, enforcing physics invariants) and **submits all of them** via `kie_post`, returning every `taskId`.
2. Write each `taskId` into its `refs/F-<HANDLE>.gen.json` sidecar immediately, and add a row to `ads/<ad>/generations.md` (⏳ generating).
3. **STOP** and surface the board. When the user asks "check", run one `kie_get` per outstanding task — in parallel, single turn. Save each landed ref as `refs/F-<HANDLE>.png`, update the board row to ✅ landed, paste its URL.
4. **Gate each ref as it lands** — approve → board row → 👍 approved; reject → archive flow (§5) and submit a revision. Other refs untouched.

**Model: `gpt-image-2-image-to-image`** (locked — same model as Step 4 storyboards, so refs and storyboards stay visually consistent and any fine text / logo / packaging label renders correctly). Apply the slot-legend convention from `.claude/rules/prompt-conventions.md` §5 whenever a ref takes more than one input image.

**Resolution: `1K`** (locked for all refs — refs are inputs to later steps, never final outputs; 1K costs 6 credits vs 10 for 2K with no meaningful quality difference for reference use).

**Reference completeness check — run before moving to Step 4:** After all refs are approved, review the brief and ask: *"For every distinct product state in the ad — open lid, closed lid, in-use grip, any special mechanism, each colourway featured — does a dedicated ref exist?"* If any state is unanchored, generate the missing ref now as a Step 3 extension before proceeding. Catching a gap here costs 6 credits. Catching it at storyboard review costs 6 (regen ref) + 6 (regen storyboard) = 12 credits minimum, plus extra turns.

**Gate:** every ref. The cheapest step before compounding cost.

---

## Step 4 — Storyboards (one per segment)

**Inputs:** approved refs from Step 3 + `plan.md` (per-segment storyboard intent).
**Subagent:** `cowork-prompt-craftsman` — **batched** across segments. Follow the lifecycle in `.claude/rules/kie-and-files.md` §3–4: submit all, persist task IDs, STOP. The user asks "check"; you check; gate each as it lands.

Per segment, Craftsman writes a composite prompt for a **multi-panel storyboard PNG**:

- Single image, N panels arranged left-to-right, each cell portrait 9:16, consistent lighting across panels.
- **Pick N to match the segment's shot count** (from `brief.md` / `plan.md`). Typical range **3–8 panels**. More than 8 risks gpt-image-2 hallucinating (skipped numbers, blurred panel boundaries, text drift).
- Each panel must render **four visible elements** inside it:
  1. **Panel number** — large, clearly legible number in the top-left corner ("1", "2", "3", …)
  2. **Timestamp** — the panel's time range within the segment (e.g. "0:00–0:03"), shown near the panel number or as a small label at the top
  3. **Scene description** — one-line caption of what's happening visually in the panel (e.g. "Dad lifts camera to eye"), rendered as a small caption bar at the bottom or overlaid in small text
  4. **Voiceover line** — the VO playing during that panel, shown as a subtitle strip at the very bottom of the panel, in quotes. If the panel has no VO, leave the strip blank or write "(no VO)".
- Use a clean sans-serif, white text on a semi-transparent dark bar at the bottom — the VO and scene label must be legible.
- Pass the refs that should appear in each panel as numbered slots — apply the slot-legend convention from `.claude/rules/prompt-conventions.md` §5.

The storyboard's job is to make every shot legible enough that Step 5's motion prompt can be written from it: timestamp for shot sync, VO for audio sync, scene description for composition.

Saves to `storyboards/SB-<SEGMENT>.png` + sidecar. **Model: `gpt-image-2-image-to-image`** (locked). **Resolution: `1K`** (locked — storyboards are layout references only, never final outputs; 1K costs 6 credits vs 10 for 2K with no quality difference for this purpose).

**Narrative logic check — run before submitting storyboards:** Read `brief.md` and check: does every character's prop make logical sense across all segments? Specifically — if a character reacts with surprise to something (cold water, a beautiful bottle, a product feature), would that surprise be plausible given what they're shown holding in earlier segments? If there's an inconsistency, fix `brief.md` and `plan.md` now before generating. Fixing at brief costs 0 credits; fixing post-storyboard costs 2+ storyboard regens (12–20 credits) plus extra turns.

Then you gate with the user.

The storyboard is the **last cheap step before video gen** — get it right here.

---

## Step 5 — Video segments

**Inputs:** approved storyboard for the segment, the refs called out in panels, the segment's VO line from `brief.md`, the segment's duration from `brief.md`.
**Subagent:** `cowork-prompt-craftsman` — **batched** across segments. Follow the lifecycle in `.claude/rules/kie-and-files.md` §3–4. This is the slow step (often 10–15 min per segment), so submitting all at once and letting the user say "check" later is essential — never auto-poll.

Per segment:
1. Craftsman writes the Seedance Pro motion prompt. Duration matches `brief.md` (3–15s window). Numbers the shots, calls transitions, anchors physics from `PRODUCT-SPEC.md`.

   **Reference image slot-legend** — apply the convention from `.claude/rules/prompt-conventions.md` §5. Concrete example for this recipe:
   - Open the prompt with a `REFERENCE IMAGE SLOTS` block that names every slot in the exact order they appear in `reference_image_urls`. Example:
     ```
     REFERENCE IMAGE SLOTS:
     Image 1 = SB-A storyboard — 4-panel shot sequence layout, match these visual beats
     Image 2 = F-PLATE-STUDIO — hero Ace of Plates metal print with family photo, plate identity throughout
     Image 3 = plate-finish-edge-thin — razor-thin aluminium edge cross-section
     Image 4 = plate-finish-corner-macro — glossy print surface and corner detail
     ```
   - Inside **each shot description**, explicitly call out which image slot to use for the key product/person element. Never say "match the plate reference" — say "match Image 2 for the plate." Example:
     ```
     SHOT 2 (3–6s): Camera pushes in to the razor-thin edge (match Image 3 — edge cross-section geometry).
     SHOT 4 (9.5–13s): Extreme close detail of the glossy print surface (match Image 4 — surface and colour).
     ```
   - This applies to video prompts exactly as it does to image prompts — with multiple references in the array the model cannot tell which image is which and will blend or guess.

   **Literal panel transcription (required — the storyboard PNG is a layout hint only):**
   - Seedance is a one-take video model. It cannot "read" a multi-panel storyboard image and reproduce its composition. The storyboard's job is to help the Craftsman *write the words*; the model needs the composition in the prompt text.
   - Inside every `SHOT N` block, after the slot binding, **literally describe the matching storyboard panel's composition** — object positions, framing, camera angle, hand positions, where each product element sits in the frame. Use spatial language ("left 2/3 of frame", "upper-right corner", "framed from waist up"). **Never paraphrase intent** — "The plate clicks onto the bracket" describes intent; "Hand grips lower-left edge of plate; plate enters frame from below-left at a slight upward angle; bracket visible as small dark rectangle pre-mounted at upper-centre of cream wall; plate moves up and across to meet bracket" describes composition.
   - Open the prompt with this clarifying line right after the slot legend:
     ```
     NOTE: Image 1 (storyboard) is a layout reference only — do NOT render panel grids, borders, or thumbnail frames in the output. The shot compositions are described in the SHOT blocks below.
     ```

   **SCALE LOCK block** — apply the pattern from `.claude/rules/prompt-conventions.md` §6. For this recipe, the block goes **after the slot legend and after the storyboard-note** (so the model sees: slots → "Image 1 is layout only" → dimensions → SHOT blocks).
   - Example for an Ace of Plates segment with bracket + plate:
     ```
     SCALE LOCK:
     - Plate: A4–A2 size (21×30cm to 42×60cm). Razor-thin 4–5mm edge, single solid sheet.
     - Bracket: Rectangular block, ~5cm × 7cm × 6mm — about the size of a matchbox or a playing-card half. Much smaller than the plate.
     - Relative scale when shown together: the bracket is roughly 1/4 the plate's height, never larger than 1/3. In any side-by-side or system-laid-out shot, the bracket reads as a small dark accessory next to the much larger plate.
     ```
   - The `NEVER SHOW` block stays separate and handles hallucination negatives; SCALE LOCK handles dimensions and relative size. Both blocks are required when applicable.
2. **VO delivery block** — every segment prompt that has voiceover must include this block, placed **after the shot sequence**, assembled as follows:
   - Open with `VOICEOVER —` followed by the **locked voice descriptor copied verbatim from `brief.md` `## Voiceover voices`**. Never rephrase it — identical string in every segment.
   - List each line from `brief.md` with a **shot timing tag** directly after it (e.g. `— plays over Shot 1 and Shot 2`). This is how the Craftsman maps speech to motion — it goes in the prompt, not the brief.
   - Weave SFX beats from `brief.md` into the flow as natural language cues on their own lines (e.g. `— pause one beat, then a soft shimmer sound —`). Do NOT use bare bracket tags like `[1s pause]` floating alone — they may be ignored. Describe the beat as a natural instruction.
   - If the brief defines multiple voices, label each line with the speaker name from the brief (e.g. `Narrator:` / `Character:`).
   - Close with `NO background music.` on its own line.
   - Example block (single voice, with shot timing + SFX woven in):
     ```
     VOICEOVER — warm Singaporean female, 28–35, clear unhurried delivery, confident but approachable, conversational Singapore English without heavy particles, slight natural warmth, measured pace with deliberate pauses at key beats:
     "Most people put their photos in a frame. Here's why that's about to change." — plays over Shot 1 and Shot 2
     — pause one beat, then a soft gloss shimmer sound as light sweeps the plate face —
     "This is a metal print from Ace of Plates. High-gloss aluminium — not paper, not canvas, not acrylic." — plays over Shot 3
     "The difference? Your colours are more vivid than any paper print you've ever seen." — plays over Shot 4
     NO background music.
     ```
3. Set `generate_audio: true` in the KIE request body. This is **not optional** — VO is baked into the segment so the Composer can mix it with BGM in Step 7.
4. Calls `kie_post` with `bytedance/seedance-2` (frame-to-frame, 9:16). Verify the current Seedance slug + field names via `kie_fetch_model_docs({ path: "bytedance/seedance-2" })` if unsure.
5. Saves `segments/seg-<SEGMENT>.mp4` + sidecar
6. You gate with user

This is the highest-cost generation in the recipe — **never skip the gate**.

---

## Step 6 — BGM

**Inputs:** music brief from `plan.md`, total cut length.
**Subagent:** `cowork-prompt-craftsman`.

1. Craftsman writes the Suno prompt — vibe + instrumentation + length-to-fit + no vocals (default)
2. Generates 2 variants (default — user can request more)
3. Saves `music/bgm-variant-A.mp3`, `bgm-variant-B.mp3` + sidecars
4. You gate: user picks one (or rejects both → regenerate)

The pick is recorded in `.claude/agent-memory/<ad-slug>-music-pick.md` so the Composer knows which one to use.

---

## Step 7 — Compose

**Inputs:** approved segments (in segment order), picked BGM, end-card spec.
**Subagent:** dispatch `cowork-composer`.

1. Composer reads `brief.md` for end-card CTA and brand-mark path; reads `.claude/agent-memory/<ad-slug>-music-pick.md` for BGM choice
2. Writes `final/compose.json` (or `compose-<variant>.json`)
3. Runs the ffmpeg pipeline via `mcp__ffmpeg__shell_run` — four serial calls: probe → concat segments → render end-card → mix BGM. Details in the Composer agent spec.
4. Saves `final/cut-<variant>.mp4`
5. Writes `final/cut-<variant>.gen.json` sidecar
6. Updates `ads/<ad>/generations.md` — adds a `## Final Output` section at the top with the cut filename, state (✅ rendered), and local path
7. Returns paths to you

**Gate:** user reviews the cut. Reject → revise compose.json + rerun. Approve → done.

User can request additional variants ("same cut but with BGM A and a shorter end-card") — Composer generates `cut-<label>.mp4` alongside the default, accumulating.

---

## Done criteria

- `final/cut-<variant>.mp4` exists, user-approved
- `final/compose.json` documents the cut
- `ads/<ad>/generations.md` has a `## Final Output` section with the cut listed
- Every generated file has a `.gen.json` sidecar (5 fields per `rules/kie-and-files.md` §1)
- Every reject lives in an `archive/<ts>-<reason>/` folder with `WHY.md`
- `.claude/agent-memory/<ad-slug>-music-pick.md` records the BGM choice

---

## Model defaults (v1)

| Step | Model | Why |
|---|---|---|
| Refs | `gpt-image-2-image-to-image` | Locked. Strong multi-reference composite + clean text/logo/packaging-label rendering. Same model as storyboards so refs + storyboards stay visually consistent. Docs path: `gpt/gpt-image-2-image-to-image`. |
| Storyboards | `gpt-image-2-image-to-image` | Locked. Required for legible panel numbers, timestamps, and VO subtitle strips. |
| Segments | `bytedance/seedance-2` (Pro tier) or `bytedance/seedance-2-fast` (cheaper, no `reference_video_urls`/`reference_audio_urls`) | Best frame-to-frame video; native VO+SFX; flexible 3–15s. Verify current slug + fields via `kie_fetch_model_docs`. |
| BGM | Suno (latest V4 / V4_5 / V5 / V5_5) — custom endpoint, see `kie_fetch_model_docs({ path: "suno-api/generate-music" })` | Instrumental by default; variant count from `brief.md` / `plan.md`. |
| Compose | bundled `ffmpeg` via `mcp__ffmpeg__shell_run` | Deterministic |

These are creative defaults, not hardcoded payloads. For any model, fetch its docs via `kie_fetch_model_docs` to get the exact slug + parameter list before submitting. Override per ad in `brief.md` or `plan.md` if a specific shot needs a different model.

---

## Gating rules (one-line summary)

Every brief, every plan, every ref, every storyboard, every segment, every BGM pick, every final cut is a gate. **No silent generation.** Rejects never delete — they go to `archive/<ts>-<reason>/` with a `WHY.md`. Future regens learn from past failures.
