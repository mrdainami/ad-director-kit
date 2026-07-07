---
name: scrape-brand
description: "Use this skill when the user wants to onboard a brand from a URL instead of self-uploading. Fetches the brand's site via curl (through ffmpeg-mcp shell_run), extracts product images + brand copy + tone, downloads images to `brands/<slug>/references/`, drafts `BRAND.md` + `PRODUCT-TRUTH.md` from the synthesis. User reviews, refines, approves."
allowed-tools: Read, Write, Edit, Glob, Bash, mcp__ffmpeg__shell_run, mcp__ffmpeg__download
---

# Skill: scrape-brand

Alternative to `onboard-brand` for users who'd rather give a URL than upload files. Same end state: a populated `brands/<slug>/` folder ready for the `seedance-storyboard` recipe.

## When to use

User says any of:
- *"Scrape my brand from \<URL\>"*
- *"Set up my brand from this site"*
- *"Onboard this product page"*

If no URL: route to `onboard-brand` instead.

## Inputs you collect

1. **The brand URL** (product page, brand homepage, or both)
2. **A short brand slug** to file under (kebab-case; suggest one from the domain name + confirm with user)

## Limitations to flag upfront

Tell the user before starting:
- The scrape uses `curl` (no JavaScript rendering). Most product pages work — Shopify, BigCommerce, WooCommerce, Webflow, plain HTML all expose product images in the initial HTML or as `og:image`. Some heavy SPAs (Wix-with-lots-of-JS, certain headless stores) may hide everything behind JS. If the scrape comes back empty, fall back to `onboard-brand` and self-upload.
- We respect `robots.txt` informally — don't scrape sites that block you.
- Brand voice / tone is inferred from public copy on the site. If the user has internal style guides, ask them to paste relevant excerpts during the review.

## Sequence

### 1. Create the folder structure

Via `mcp__ffmpeg__shell_run`:

```sh
mkdir -p brands/<slug>/references brands/<slug>/{logo,notes,ads}
```

### 2. Fetch the page HTML

```sh
curl -sL \
  -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36" \
  -H "Accept: text/html,application/xhtml+xml" \
  --max-time 15 \
  "<URL>" > brands/<slug>/notes/_scrape-raw.html
```

If `curl` exits non-zero or the file is empty, retry once with `--retry 2 --retry-delay 1`. If still failing, surface the error to the user and offer `onboard-brand` as fallback.

Save the raw HTML in `notes/_scrape-raw.html` for diagnosis (prefix with `_` so it sorts to the top + signals "intermediate"). Do not commit it as a permanent brand asset.

### 3. Read the HTML and extract signal

You (the agent) read `_scrape-raw.html` directly. Pull out:

- **Brand name** — from `<title>`, `og:site_name`, `<h1>`, or domain
- **Product name** — `<title>`, `og:title`, primary `<h1>` on a product page
- **Product copy** — meta descriptions, `<meta name="description">`, prominent paragraphs
- **Product images** — `<img>` `src` attributes + `og:image` + `<link rel="image_src">`. Resolve relative URLs against the base URL. Filter out obvious junk:
  - data URIs (start with `data:`)
  - tracking pixels (`pixel`, `analytics`, `tracking` in the path)
  - icons / favicons (`favicon`, `.ico`, `1x1`, `spacer`)
  - SVG flags / badges / icons (`flag`, `icon`, `badge` in the SVG path)
- **Logo candidates** — look for `<img>` with `logo` in the path, class, or alt; SVG logos in the `<header>`; the favicon as a last-resort fallback
- **Brand colors** — CSS variables in `<style>` tags, or major hex codes in inline styles
- **Tone signals** — read the actual copy: is it formal? Conversational? Heavy with adjectives? Bullet-list-driven?
- **Price** — look for `$`, `€`, `£`, `SGD`, `MYR`, etc.
- **Links** — collect IG / TikTok / FB handles from `<a href="https://instagram.com/...">` etc.

Don't over-engineer the parsing — just read the HTML and pull what you see. The output of this step is a mental model, not a parsed JSON blob.

### 4. Show the user the inventory before downloading

Present the user with:
- Detected brand name + product name
- Top 10–15 image URLs with their best-guess role (`product-front`, `product-angle`, `product-lifestyle`, `logo`, `package`, `unknown`)
- Detected brand colors
- Detected tone summary

Ask: which images should we download? Default suggestion: top 8–12 product images + 1 logo. The user can edit the list before we download.

### 5. Download approved images

For each approved image URL, use `mcp__ffmpeg__download`:

```
download(
  url: "<image URL>",
  destPath: "brands/<slug>/references/<component>/<sanitised-filename>.<ext>"
)
```

For the logo: `destPath: "brands/<slug>/logo/<filename>"`.

Organise `references/` into component subfolders if there's a clear anatomy (e.g. `references/plate/`, `references/mount/`, `references/packaging/`). For simpler brands, flat `references/` is fine.

If a download fails, log it and continue with the rest.

### 6. Synthesise BRAND.md + PRODUCT-TRUTH.md drafts

Write `brands/<slug>/BRAND.md` and `brands/<slug>/PRODUCT-TRUTH.md` from what you learned. Structure them the same way `onboard-brand` does — see the `onboard-brand` SKILL.md for the templates. Also write `brands/<slug>/reference-map.json` (role → file), per `onboard-brand` step 5b.

**Crucially:** mark these as **drafts**. Add at the top of each:

```
> Draft generated from scrape on <date>. Source: <URL>. Review carefully — anything inferred from the site may be wrong about details (materials, dimensions, mechanism). Correct what's wrong before approving.
```

The scrape will produce a *plausible* BRAND.md but the PRODUCT-TRUTH details (especially physics invariants — what AI commonly hallucinates wrong) are inferences from photos and copy. The user knows their product better than the site does; they need to fix the truth.

### 7. Walk the user through the drafts

Show each file's content in chat. Ask section-by-section:
- "Audience — is this right? Anything missing?"
- "Tone — does this sound like you? Any phrases you'd avoid?"
- "Physics — I inferred the edge is ~5mm from the photos. Confirm the real measurement."
- "Mechanism — I said it 'sticks via adhesive strips' from the copy. Is that right?"

Edit until the user approves both files.

### 8. Clean up

Once approved, delete `notes/_scrape-raw.html` (it's no longer needed and may contain tracking junk):

```sh
rm brands/<slug>/notes/_scrape-raw.html
```

Optionally keep a 1-page summary at `notes/scrape-source.md` recording: source URL, scrape date, number of images downloaded, anything notable about the site (e.g. "Shopify store", "modern Webflow", "had to fall back to og:image only").

### 9. Confirm done

Tell the user:

> "Brand '<slug>' is set up from <URL>. <N> product photos + logo downloaded. BRAND.md + PRODUCT-TRUTH.md drafted and approved. Ready to make ads with: 'Use seedance-storyboard for <slug>. Idea: …'."

## When the scrape fails / partially fails

| Failure | Fallback |
|---|---|
| Curl returns 403 / 429 (blocked) | Try with a different User-Agent, then offer `onboard-brand` |
| HTML has 0 useful images | Likely JS-heavy site. Offer `onboard-brand` so user self-uploads |
| Logo not found | Skip logo step; ask user to drop one in `logo/` after |
| Tone unclear from copy | Write a placeholder + ask the user to revise |

## What this skill does NOT do

- Does not bypass `robots.txt`, login walls, or anti-bot CAPTCHAs
- Does not render JavaScript (no Playwright/headless Chrome)
- Does not generate any AI imagery — only downloads what's on the brand's site
- Does not modify any recipe / agent / system file
- Does not replace `onboard-brand` — it's an alternative onramp

## Anti-patterns

- **Don't trust the scraped PRODUCT-TRUTH without user review.** Drafts are starting points. AI inference from photos and copy can be wrong about materials, dimensions, mechanism. Always walk the user through.
- **Don't over-download.** 8–12 images is usually plenty. Downloading every `<img>` on a site bloats `references/` with junk.
- **Don't keep `_scrape-raw.html`.** It's intermediate. Delete it after you're done synthesising.
- **Don't skip the gate.** The user MUST approve BRAND.md + PRODUCT-TRUTH.md before this skill exits. Bad brand truth poisons every downstream prompt.
