---
name: brand-scope
description: "Auto-loaded when Claude reads or writes anything under brands/<brand>/. Ensures brand truth is always in context before generation."
appliesTo:
  - "brands/**"
---

When working inside any `brands/<brand>/` folder, you MUST:

1. **Treat `PRODUCT-TRUTH.md` (at the brand root) as authoritative** on product material, physics, and AI-hallucination gotchas. If a script, plan, or prompt contradicts it, flag the contradiction and use the truthful phrasing.

2. **Treat `BRAND.md` (at the brand root) as authoritative** on audience, tone, and brand voice. Don't invent voice; honour the brand card.

3. **Treat `PRODUCT-SPEC.md` (at the brand root, if it exists) as the deeper invariants source** — physics invariants block, action-realism library. Inject relevant sections verbatim into KIE prompts. **Reference selection is driven by `reference-map.json`** (role → file), not by any in-doc lookup table.

4. **Never write outside the active brand/ad folder, `.claude/agent-memory/`, or sidecars next to generated files.** Specifically, do not modify the brand-truth files (`BRAND.md`, `PRODUCT-TRUTH.md`, `PRODUCT-SPEC.md`, `reference-map.json`) without explicit user permission — those are the brand's locked truth.

5. **Reference resolution** — when a prompt needs an image reference, run the "ensure URL" 3-step in `.claude/rules/kie-and-files.md` §6. Uploads are lazy and per-use — never bulk-upload the kit.

6. **Reference labels must match reality** — the folder/filename of a reference photo is a *claim*, not proof. If a photo's visible content contradicts what its name/folder or PRODUCT-TRUTH says it is, STOP, flag it to the user, and do not feed it as that reference. A wrong label silently poisons every downstream generation.
