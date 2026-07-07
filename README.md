# cowork

A self-contained Claude project for making brand video ads. Drop a folder for each brand into `brands/`, open `cowork/` in **Claude Cowork**, and chat with the Director to onboard the brand + generate ads. Every artefact lands in `brands/<brand>/ads/<ad-slug>/`.

No app, no UI, no deploys, no scripts to maintain. Just a folder + two drag-and-drop MCPs.

---

## What the user installs

| Thing | Why | How |
|---|---|---|
| **Claude Cowork** | The client (desktop app) | Download from https://claude.com/download |
| **kie-mcp.mcpb** (16 MB) | All AI generation: images, video, music via KIE.ai | Drag `cowork/tools/kie-mcp.mcpb` onto Cowork → paste KIE API key |
| **ffmpeg-mcp.mcpb** (107 MB) | Compose: concat, BGM mix, end-card. **Bundles ffmpeg + ffprobe binaries.** Also runs `curl` / `mkdir` / etc. | Drag `cowork/tools/ffmpeg-mcp.mcpb` onto Cowork — no key |
| **KIE API key** | Generation budget (pay-as-you-go, ~$5–15 per ad) | [kie.ai](https://kie.ai?ref=41abfa41934c4f15a97d88d2d4f8162a) → Dashboard → API Keys |

That's the **complete install list**. No Node.js, no ffmpeg system install, no Python, no Docker, no cloud storage.

---

## First-time setup (≈ 5 minutes)

1. Install **Claude Cowork** if you don't have it.
2. Sign up at **[kie.ai](https://kie.ai?ref=41abfa41934c4f15a97d88d2d4f8162a)** and copy an API key.
3. Open Cowork. Drag `cowork/tools/kie-mcp.mcpb` onto the window → paste your KIE key when prompted.
4. Drag `cowork/tools/ffmpeg-mcp.mcpb` onto the window → no key needed.
5. Restart Cowork so both bundles register.
6. In Cowork: **Work in a project → Select folder → pick this `cowork/` folder**.
7. Send your first message (see below).

After that, the two MCPs are registered at the **user level** — they're available in every Cowork project, not just this one. Install once, use forever.

## Your first message

You're now talking to the Director (driven by `CLAUDE.md`). This template ships with no brand — your first step is to add yours. Two onramps:

*Onramp A (scrape from URL):* tell the Director *"Scrape my brand from \<URL\>"*. The `scrape-brand` skill pulls product images + brand copy from the site, downloads them into `brands/<slug>/brand-kit/`, drafts `BRAND.md` + `PRODUCT-TRUTH.md` from the synthesis. You review + refine. Works on most static + Shopify / Webflow / WooCommerce stores. Falls back to self-upload if the site is too JS-heavy.

*Onramp B (self-upload):*
1. `mkdir cowork/brands/<your-slug>/brand-kit/{products,logo,notes}`
2. Drop your product photos in `brand-kit/products/` (organise however makes sense — by anatomy, by SKU, flat)
3. Drop your logo in `brand-kit/logo/`
4. In Cowork: *"Onboard my brand \<your-slug\>"*
5. The Director (via `onboard-brand` skill) walks you through it, writes `BRAND.md` + `PRODUCT-TRUTH.md` from conversation

Either way, then: *"Use seedance-storyboard for \<your-slug\>. Idea: …"*

The Director runs preflight automatically on first message (verifies both MCPs, uploads brand-kit images to `catbox.moe`, caches URLs in `.claude/agent-memory/`), then walks you through the recipe step by step.

---

## What's in the folder

```
cowork/
  CLAUDE.md                                ← Director persona (auto-loaded by Claude)
  README.md                                ← this file
  .mcp.json.example                        ← Claude Code users only (not needed for Cowork)
  .gitignore                               ← excludes .mcp.json + .env
  .claude/
    settings.json                          ← tool allowlist
    agents/                                ← specialist subagents (Task-dispatchable)
    skills/
      seedance-storyboard/SKILL.md         ← the ad recipe
      onboard-brand/SKILL.md               ← new-brand setup
    rules/brand-scope.md                   ← auto-loaded when working in brands/
    agent-memory/                          ← persistent decisions across sessions
  brands/<brand-slug>/
    brand-kit/ {BRAND.md, PRODUCT-TRUTH.md, products/, logo/, notes/}
    ads/<ad-slug>/
      brief.md, plan.md
      refs/, storyboards/, segments/, music/, final/  (each + archive/)
  tools/
    README.md                              ← drag-and-drop walkthrough
    kie-mcp.mcpb                           ← drag onto Cowork
    ffmpeg-mcp.mcpb                        ← drag onto Cowork
```

## How a session works

1. You tell the Director the brand + recipe + idea
2. Preflight runs once (verifies MCPs, uploads brand-kit photos to catbox, caches URLs)
3. Director walks the recipe step by step, dispatching specialist subagents:
   - **Scripter** writes the brief
   - **Prompt-craftsman** writes prompts + calls KIE
   - **Composer** runs ffmpeg via the MCP
4. After each generation, the Director shows you the artefact and waits for approval. Approve → file locks. Reject → file moves to `archive/<ts>-<reason>/` with a `WHY.md`, regenerated with your critique
5. Final cut + gallery preview in `brands/<brand>/ads/<ad>/final/`

---

## Alternative: Claude Code (for developers)

If you'd rather use Claude Code (the CLI), do this instead of steps 3–5 above:

```bash
cp cowork/.mcp.json.example cowork/.mcp.json
# edit cowork/.mcp.json, paste your KIE key
cd cowork
claude
```

Claude Code requires Node 18+ installed locally (it's itself a Node app). The Cowork path bundles everything inside the `.mcpb` files.

## Brands

`brands/` ships empty — you add your own (see **Your first message** above). Onboarding writes each brand a `BRAND.md`, `PRODUCT-TRUTH.md`, `reference-map.json`, and drops your product photos + logo under `brands/<slug>/`. That folder is what makes the ads on-brand instead of generic.

## Recipes available

| Recipe | What it makes |
|---|---|
| `seedance-storyboard` | Vertical Reel (9:16). N segments (3–15s each, Seedance Pro) + 3s ffmpeg end-card + separate Suno BGM bed. Storyboard-first workflow. |

## Skills available

| Skill | When it activates |
|---|---|
| `seedance-storyboard` | User asks to make a brand ad |
| `onboard-brand` | User asks to add their brand via self-upload (drop photos + chat) |
| `scrape-brand` | User gives a brand URL and asks to onboard from there |

## Troubleshooting

- **"KIE MCP not responding"** → the `.mcpb` didn't install. Re-drag `cowork/tools/kie-mcp.mcpb` onto Cowork, paste key, restart.
- **"ffmpeg-mcp shell_run failed: command not found"** → the bundled binary's PATH prepend isn't working. Check ffmpeg-mcp installed cleanly; reinstall the `.mcpb` if not.
- **"asset upload failed"** → check internet (catbox.moe must be reachable); if catbox is blocked in your region, see `cowork/tools/README.md` for alternative backends.
- **"the agent isn't following the recipe"** → verify `CLAUDE.md` and the skill markdown loaded — restart the Cowork session if you just edited them.
