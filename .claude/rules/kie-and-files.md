---
description: Universal conventions for calling KIE and for filing what you make. Applies to every recipe.
---

# KIE + files

These rules apply to every KIE generation and every saved artefact in this project, regardless of recipe. Recipe-specific filename patterns (e.g. `F-HERO`, `SB-A`, `seg-A`) and per-step gating live in each recipe's `SKILL.md`. The conventions below are the durable cross-cutting layer.

---

## 1. Sidecar — 5 fields, written immediately

Every generated file gets a sibling `<filename>.gen.json` in the same folder. Five fields — the minimum to recreate the generation. Nothing else:

```json
{
  "task_id": "abc123",
  "model": "nano-banana-pro",
  "prompt": "...verbatim prompt sent to KIE...",
  "reference_images": ["references/install/install-place-plate-on-mount.png"],
  "reference_urls": ["https://kie.../asset.png"]
}
```

- `task_id` is written **immediately on submit**, before any status check. Losing it = orphaned credits.
- `prompt` is **verbatim** — exactly what KIE saw. Not a paraphrase, not a summary.
- `reference_images` are local paths; `reference_urls` are the KIE-hosted versions of those same files (KIE only sees URLs).

Approval state, archive reason, lineage, cost estimates, timestamps — none of these go in the sidecar. The board (`generations.md`) tracks state; archived files live under `archive/<ts>-<reason>/` with a `WHY.md` next to them (that's the lineage record).

---

## 2. KIE submit + poll discipline

The kie MCP owns the KIE plumbing — submit envelope, file upload, per-model payload discovery. Its standing instructions are loaded automatically on connect; read them once per session.

Tools you use:

- `kie_fetch_model_docs({ path })` — get the live spec for any model (cached 3 days). Use whenever you're about to call a model whose payload you don't already know this session. Don't guess from memory.
- `kie_post` — submit. Capture `data.taskId` from the response and write it to the artefact's sidecar **before doing anything else**.
- `kie_get` — check status via `/api/v1/jobs/recordInfo?taskId=…`. Result URL is at `data.resultJson.resultUrls[0]`; failure reason at `data.failMsg`. Custom-envelope models (Veo, Suno, Flux Kontext) — see their docs page for the variant.
- `kie_upload_file({ localPath })` — local file → KIE-hosted URL (~3-day TTL). Use for every reference image; never hand-craft curl uploads.
- `kie_download({ url, destPath })` — pull a KIE result to disk.

The project-specific addition on top of the MCP's defaults: **write `task_id` to the file's sidecar (or `pending.json`) before any status check, every time.** A status check that runs before the persist is one disconnect away from an orphaned task.

_Background: `kie_run_and_wait` was retired — it blocked past the MCP's ~60s call ceiling and died without returning the taskId, causing a duplicate submission (double credits). Standard pattern is now submit + on-demand check; never block._

---

## 3. Batching multi-slot waves

When a caller hands you several artefacts in one wave (Step 3 refs, Step 4 storyboards across segments, Step 5 video segments):

1. Submit all of them first — `kie_post` in parallel inside one turn.
2. Capture every `taskId` and write each to its sidecar (or `pending.json`) immediately.
3. Add one row per task to `generations.md` (see §4).
4. Then proceed — don't finish one before starting the next.

Before re-submitting any slot, check its sidecar for a still-live `taskId` and poll that instead.

---

## 4. Job lifecycle — submit, board, on-demand check

Every KIE generation (image, video, music — one slot or many) follows the same lifecycle. `ads/<ad>/generations.md` is the source of truth for what's pending. **You do not auto-poll.**

**Board schema** — four columns, no overlap with the sidecar:

```
| Handle  | Task ID    | State          | URL                           |
|---------|------------|----------------|-------------------------------|
| seg-A   | 6f11610a…  | ⏳ generating  | —                             |
| seg-B   | faeb925c…  | ✅ landed      | https://tempfile.aiquickdraw… |
| seg-C   | 0d27b9fc…  | 👍 approved    | https://tempfile.aiquickdraw… |
| seg-D   | 830c57fb…  | 📦 archived    | —                             |
```

- **Handle** — slot name (e.g. `F-HERO`, `SB-A`, `seg-A`). How the user refers to it.
- **Task ID** — first 8 chars + `…`. Full ID lives in the sidecar.
- **State** — one of: ⏳ generating · ✅ landed (awaiting gate) · 👍 approved · 📦 archived.
- **URL** — KIE result URL while it's fresh (~3 days). After expiry it's dead; the local file under the conventional folder is the canonical record.

**Lifecycle:**

1. **Submit.** Fire `kie_post` for each slot — in parallel inside one turn if there's more than one. From each response, grab `data.taskId`.

2. **Persist + surface.** For each task: write the `task_id` into its sidecar (or a `pending.json` placeholder if the file doesn't exist yet) and add a row to `generations.md` marked ⏳ generating. Show the user the board + tell them to say "check" when they want a status lookup.

3. **STOP.** Do not loop `kie_get` waiting for jobs to land — you have no real clock to wait between calls, and rapid-fire polling burns tokens and time with no benefit. The user reads the board; the user asks when they want to check.

4. **On-demand check.** When the user asks ("check", "any done?", "status on seg-B"), fire one `kie_get` per outstanding `task_id` — all in parallel in a single turn. For each: update the board row (state → ✅ landed, URL filled); if `success`, `kie_download` the result and present the URL in chat; if `fail`, present `failMsg`. If anything's still generating, say so and stop again until the user asks again.

5. **Named regen.** User names a handle (*"regen F-HERO, warmer"*). Run the archive flow (§5), submit a revision (fresh `task_id`, new sidecar — lineage is captured by the archive folder + WHY.md, not by a field in the sidecar), flip the board row to ⏳ generating. Other slots untouched.

---

## 5. Archive flow

When the user rejects a generated file:

```sh
mkdir -p <step-folder>/archive/<YYYY-MM-DDTHH-MM>-<short-reason>/
mv <file> <file>.gen.json <step-folder>/archive/<ts>-<reason>/
```

Then write `<step-folder>/archive/<ts>-<reason>/WHY.md` containing the user's critique **verbatim** — not a paraphrase. Regenerate with a revised prompt that addresses the critique. The new file's sidecar is fresh (no `regen_of` field — lineage lives in the archive folder structure).

Rejects never delete. Future regens learn from past failures by reading `WHY.md` files in archive folders.

---

## 6. Reference URLs — lazy, KIE-hosted, "ensure URL"

References are uploaded to KIE **lazily, per use** — never bulk-uploaded.

`.claude/agent-memory/asset-urls.json` is a per-brand cache, keyed by the file's relative path. KIE-hosted URLs expire ~3 days, so cached entries are stale after ~2.5 days and must be re-uploaded.

**Ensure URL — run this 3-step before any generation that needs a local file as a reference:**

1. Look up the file's relative path in `asset-urls.json`. If present and `uploaded_at` is younger than ~2.5 days, **reuse** the cached `url`.
2. Otherwise call `kie_upload_file({ localPath: "<relative path>", uploadPath: "<brand>/refs" })` and read `url` from the response.
3. Write `{ "url": "<url>", "uploaded_at": "<ISO 8601>" }` back into `asset-urls.json` keyed by the relative path.

This map is the source of truth for KIE reference URLs across the session. Record both the local path and the resolved URL in the sidecar (`reference_images` + `reference_urls`).
