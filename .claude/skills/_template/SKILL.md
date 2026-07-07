---
name: _template
description: "TEMPLATE — DO NOT INVOKE. Copy this folder to `.claude/skills/<recipe-name>/` and fill in the placeholders to define a new recipe. Then add a one-line entry in CLAUDE.md → 'Available skills' under the matching group."
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task, mcp__kie__kie_post, mcp__kie__kie_get, mcp__kie__kie_upload_file, mcp__kie__kie_download, mcp__kie__kie_fetch_model_docs, mcp__ffmpeg__shell_run, mcp__ffmpeg__download
---

<!--
HOW TO USE THIS TEMPLATE

1. Copy this folder: `cp -R .claude/skills/_template .claude/skills/<new-recipe-name>`
2. Edit the frontmatter:
   - `name:` → match the folder name (e.g. `seedance-storyboard`)
   - `description:` → one paragraph: what this recipe produces and when to invoke it
   - `allowed-tools:` → keep what you need, drop what you don't
3. Delete this HTML comment block.
4. Fill in every `<placeholder>` below.
5. Delete any sections that don't apply to this recipe (e.g. drop "Recipe-specific overrides" if there are none; drop the music step if the recipe has no audio).
6. Add a one-line entry in `ad-director-kit/CLAUDE.md` → "Available skills" under the matching group.

The template covers what's *unique* to a recipe. Everything cross-cutting (prompt conventions, KIE plumbing, file conventions, gate-every-artefact) lives in `.claude/rules/` and `CLAUDE.md` — don't restate.
-->

# Recipe: <recipe-name>

**Version:** v1

<One-paragraph description of what this recipe produces and when to invoke it. Mirror the frontmatter description but with more detail.>

You (the Director) follow these steps in order. Each step has a gate — never skip the gate.

---

## What this recipe produces

| Output | Format | Generator | Step |
|---|---|---|---|
| Locked brief | `brief.md` | `ad-director-kit-scripter` subagent | 1 |
| Recipe plan | `plan.md` | Director (you) writes directly | 2 |
| <artefact> | <format e.g. PNG, MP4, MP3> | `ad-director-kit-prompt-craftsman` → <KIE model> | <step #> |
| … | … | … | … |
| Final cut(s) | <format> | `ad-director-kit-composer` → ffmpeg | <step #> |

## Folder layout for one ad

```
brands/<brand>/ads/<ad-slug>/
  brief.md
  plan.md
  <step-folder>/   + archive/   ← each generated file has a .gen.json sidecar
  …
  final/
    compose.json (or compose-<variant>.json)
    cut-<variant>.<ext>
    gallery.html
```

**Handle naming convention** (one row per artefact type):

- `<handle-prefix>-<NAME>.<ext>` — e.g. `F-HERO.png` for foundation refs

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

Standard preflight per `CLAUDE.md` (MCPs reachable · brand exists · reference uploads lazy · memory loaded). <Add any recipe-specific preflight checks here, or delete this sentence if there are none.>

## Step 0b — Ad folder setup

Propose an ad slug (kebab-case), confirm with the user, then create the folder structure via `mcp__ffmpeg__shell_run`:

```sh
mkdir -p brands/<brand>/ads/<slug>/{<step-folder-1>,<step-folder-2>,final} \
         brands/<brand>/ads/<slug>/{<step-folder-1>,<step-folder-2>}/archive
```

---

## Step 1 — <step name>

**Inputs:** <what files/info this step needs>
**Run by:** <Director | `ad-director-kit-scripter` | `ad-director-kit-prompt-craftsman` | `ad-director-kit-composer`>
**Output:** <output file path>

<Step body. What happens in this step, any recipe-specific structure (prompt blocks, file layout, ordering). Reference the inherited rules instead of restating them.>

**Gate:** <when user approval is required and what they're approving>

---

## Step 2 — <step name>

**Inputs:** …
**Run by:** …
**Output:** …

…

---

## Step N — Compose (or final assembly)

**Inputs:** approved artefacts from prior steps
**Run by:** `ad-director-kit-composer`

<Compose step body. The final cut, optional variants, gallery preview, etc.>

**Gate:** user reviews the final cut.

---

## Done criteria

- `final/cut-<variant>.<ext>` exists, user-approved
- `final/gallery.html` exists and renders (if the recipe uses one)
- `final/compose.json` documents the cut (if the recipe uses compose)
- Every generated file has a `.gen.json` sidecar
- Every reject lives in an `archive/<ts>-<reason>/` folder with `WHY.md`

---

## Model defaults (v1)

| Step | Model | Why |
|---|---|---|
| <step name> | `<model slug>` | <one-line rationale> |
| … | … | … |

Defaults are creative choices, not hardcoded payloads. For any model, fetch its docs via `kie_fetch_model_docs` to get the exact slug + parameter list before submitting. Override per ad in `brief.md` or `plan.md` if a specific shot needs a different model.

---

## Recipe-specific overrides (optional)

<Use this section ONLY if this recipe needs to override or extend one of the inherited conventions. Examples:

- A model-specific prompt structure unique to this recipe (e.g. seedance-storyboard's SHOT N blocks, NOTE-storyboard-is-layout-hint clarifier, VOICEOVER block tied to brief's voice descriptor)
- A non-standard sidecar field for this recipe's artefact
- An additional gate beyond the universal "gate every artefact"

If nothing applies, delete this section.>

---

## Gating rules (one-line summary)

Every brief, every plan, every generated artefact, every final cut is a gate. **No silent generation.** Rejects never delete — they go to `archive/<ts>-<reason>/` with a `WHY.md`. See `rules/kie-and-files.md` §5 for the archive flow.
