---
description: Universal prompt-writing conventions. Apply to every KIE generation in any cowork recipe.
---

# Prompt conventions

These rules apply to every KIE prompt you write — image, video, or music — regardless of recipe or model.

Recipe choreography (which step, what gates, what file lands where) and per-recipe model locks live in each recipe's `SKILL.md`. Concrete prompt structures that only one recipe uses (e.g. how a seedance-storyboard segment combines a storyboard PNG + a VO block) live in that recipe too. This file is the durable cross-cutting layer.

---

## 1. Anchor product visuals to reference photos

Pass real product photos via the model's reference-image slot (`input_urls` for gpt-image-2, `reference_image_urls` for Seedance, `image_input` for nano-banana-pro single-ref). Never describe what a photo can show — pass the photo. The reference is authoritative; your text is supplementary.

## 2. Inject physics invariants verbatim

For any shot involving the brand product, copy the relevant block from the brand's `PRODUCT-SPEC.md` (or the condensed version in `PRODUCT-TRUTH.md`) into the prompt **verbatim**. Never paraphrase — paraphrasing softens what the model treats as a constraint.

Brand-specific invariants like "matte cream back with dark typography (not silver)", "4–5mm razor-thin edge", "two parallel foam strips (not a single adhesive strip)" go in unchanged every time.

## 3. Positive phrasing wherever possible

"Matte black 3D-printed bracket" beats "not a magnetic disc". Models follow what you describe better than what you negate. Reserve negatives for audio cues ("no music, no whistle") and hallucination guards (§7 NEVER SHOW), where positive phrasing fails.

## 4. Scale-anchor when size matters

Any shot where the product's true size matters needs an explicit anchor — a familiar-object comparison or a measured dimension. "Pillowcase-wide A3 plate held in two hands" beats "plate in hands".

For multi-product shots, escalate to the SCALE LOCK pattern (§6).

## 5. Multi-reference: slot-legend convention

When the model accepts more than one reference image, the prompt MUST bind each slot to a named role and reference it by number in the instructions. With several images in the array the model cannot tell which is which and will blend or guess.

**Required structure at the top of the prompt:**

```
REFERENCE IMAGE SLOTS (array order):
Image 1 = <role + what to take from it>
Image 2 = <role>
Image 3 = <role, with the exact detail to reproduce>
```

Slot order MUST match the order of URLs in the array.

Inside each shot or panel block: "use Image 2 for the plate", **not** "match the plate reference".

Applies to image prompts (gpt-image-2 `input_urls`, up to 16 refs) and video prompts (Seedance `reference_image_urls`) identically. There is no `@` prefix — slot references are plain text: `Image 1`, `Image 2`.

_Why this exists: an earlier storyboard panel with a generic "match the back reference" rendered a wrong, simplified product back — dropped the printed text, QR, wordmark, and dashed magnet outlines — because the references weren't slot-bound._

## 6. SCALE LOCK (product shots with 2+ elements, or where true size matters)

After the slot legend, add a `SCALE LOCK` block listing the true dimensions of every product visible, **copied verbatim** from `PRODUCT-TRUTH.md` / `PRODUCT-SPEC.md`. Include relative-scale anchors whenever two objects appear together.

SCALE LOCK handles **dimensions and relative size**. It does not replace NEVER SHOW.

## 7. NEVER SHOW (hallucination guards)

Use a `NEVER SHOW` block to call out things the model commonly invents that the product doesn't have. Bare negatives are fine here — this is the one place positive phrasing fails.

`NEVER SHOW` handles **hallucinations**. `SCALE LOCK` handles **dimensions**. When both apply, both blocks are required.
