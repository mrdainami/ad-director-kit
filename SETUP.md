# Setup — read me first

This is the **blank** brand-ad template. It makes brand video ads with Claude, but
it starts with **no brand loaded** — you add your own. About 5 minutes to set up.

Everything you need already ships inside this folder (in `tools/`). You do NOT
need to install ffmpeg or anything else from the internet.

---

## Step 1 — Install the two connectors

In the `tools/` folder there are two installer files:

- `kie-mcp.mcpb`  → talks to KIE.ai to generate the images, video, and music
- `ffmpeg-mcp.mcpb` → stitches the clips + music into the final video

Install each by double-clicking it (Claude will ask to confirm), or from Claude's
settings: **Settings → Connectors/Extensions → Install → pick the file**.

The ffmpeg installer is large (~112 MB) because the ffmpeg program is packed
**inside** it. That's normal — you never install ffmpeg separately.

## Step 2 — Add your KIE API key + credits

The ffmpeg connector needs no key. The KIE connector does:

1. Get a KIE.ai account + API key from [kie.ai](https://kie.ai?ref=41abfa41934c4f15a97d88d2d4f8162a), and load some credits
   (generating video costs credits).
2. Open the KIE connector's settings in Claude and paste your API key in.

An API key is personal — each person who uses this folder adds their own.

## Step 3 — Add your brand

This template ships empty. Open the folder in Claude Cowork and say
**"add my brand"** (or give Claude your brand's website URL). Claude will walk you
through creating the brand kit — product truth, photos, tone — under `brands/`.

## Step 4 — Make an ad

Once a brand exists, say what you want, e.g.
**"make a video ad for <brand> on the beach."** Claude runs the recipe step by
step (brief → plan → images → video → music → final cut), pausing for your approval
at each stage.

---

## If ffmpeg errors about a "workdir" or a missing directory

The ffmpeg connector may ship with an empty "working folder" setting. You do **not**
need to fix it — the recipe already tells Claude to pass this project folder to
ffmpeg on every command. If you ever see an error mentioning `workdir` or
`${user_config.workdir}`, just tell Claude to "pass an explicit workdir (this folder)
on every ffmpeg command" and it resolves.

---

## What's here

- `brands/` — your brands live here (empty until you add one in Step 3)
- `.claude/skills/` — the ad recipes (e.g. `seedance-storyboard` for video ads)
- `tools/` — the two connector installers above
- `CLAUDE.md` — Claude's standing instructions for running this project
