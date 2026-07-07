# Static-Ad Type Catalog

40 reference ad formats. Each entry pairs a reference image (in `references/`) with a one-line "when to use" and a source template prompt.

**How to use this file (Director):**

- During Step 2 (Concept plan), scan the catalog and propose 1–3 types that fit the brief.
- Show the user the reference image (paste the path or load it) and the "When to use" line.
- The **Source template** is the original parameterized prompt from `_experiments/REFERENCE ADS/template-prompts.md`. It assumes single-reference, "use the attached images as brand reference" prompting — **do not paste it verbatim**. Adapt it:
  - Rewrite as a slot-legend prompt (`rules/prompt-conventions.md §5`) — Image 1 = product, Image 2 = this ad-type reference, Image 3+ = additional brand refs.
  - Inject brand voice from `BRAND.md` and physics from `PRODUCT-TRUTH.md` / `PRODUCT-SPEC.md` verbatim.
  - Replace `[PLACEHOLDERS]` with concrete values from the brief + brand kit.
  - Add SCALE LOCK and NEVER SHOW blocks where the shot needs them.
- Aspect ratio in the source template is a default suggestion — override with whatever the campaign asked for and pass via the model's `size` param.

---

## 01 — Headline

**When to use:** Tests text rendering. If this comes back clean, the model handles copy.
**Reference:** `references/01-headline.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product colors, typography style, and brand tone precisely. Create: a static ad with a [BACKGROUND] background. Top third: large bold sans-serif headline reading "[YOUR HEADLINE, under 10 words]". Below in smaller text: "[YOUR SUBHEAD, one sentence]". Bottom half: [YOUR PRODUCT] on the surface with [DETAILS]. Shot at 50mm f/2.8 from slightly above. [BRAND] logo bottom right. Clean, authoritative. 4:5 aspect ratio.

---

## 02 — Offer / Promotion

**When to use:** The money-maker. Test your core offer.
**Reference:** `references/02-offer-promotion.png`
**Source template:**
> Use the attached images as brand reference. Match exact brand colors and typography style. Create: a promotional ad with a split background. Top 60% is [PRIMARY BRAND COLOR] and bottom 40% is [CONTRAST COLOR like warm cream]. [YOUR PRODUCT] sits centered where colors meet, soft studio lighting. Upper area: large [CONTRAST TEXT] sans-serif reading "[YOUR OFFER like YOUR FIRST MONTH FREE]". Below: "[OFFER DETAILS]". Lower section: small [BRAND COLOR] text with [VALUE ADDS]. [BRAND] logo bottom right. 9:16 aspect ratio.

---

## 03 — Testimonials

**When to use:** Real environments + text overlays. Tests composition depth.
**Reference:** `references/03-testimonials.png`
**Source template:**
> Use the attached images as brand reference. Create: a testimonial ad set in [SETTING like bright bathroom / kitchen] with warm natural light. [YOUR PRODUCT] on [SURFACE], slightly out of focus. Overlaid: large bold white sans-serif "[SHORT HEADLINE]". Below: "[FULL QUOTE 2-3 sentences]. [NAME], [CREDENTIAL]." Five filled [BRAND COLOR] stars. [BRAND] logo bottom right in white. Shot on 35mm f/2.0. 9:16 aspect ratio.

---

## 04 — Features / Benefits Point-Out

**When to use:** Educational diagram-style layout.
**Reference:** `references/04-features-benefits-pointout.png`
**Source template:**
> Use the attached images as brand reference. Create: an educational diagram-style ad on white background. Top: bold [BRAND COLOR] text "[HEADER like What Makes [PRODUCT] Different]". Below: [YOUR PRODUCT] centered, even studio lighting. Four callout boxes with connecting lines: "[BENEFIT 1-4]". Each has a small [BRAND COLOR] circle. "[WEBSITE]" bottom center. [BRAND] logo bottom right. Scientific diagram redesigned by a luxury agency. 4:5 aspect ratio.

---

## 05 — Bullet-Points

**When to use:** Split composition. Product left, benefits right.
**Reference:** `references/05-bullet-points.png`
**Source template:**
> Use the attached images as brand reference. Create: a benefit-list ad, split composition on [BACKGROUND] background. Left 40%: [YOUR PRODUCT] on [SURFACE], shot at 85mm f/2.8. Right 60%: vertical stack of five lines with filled [BRAND COLOR] circles: "[BENEFIT 1-5]". Clean sans-serif, generous spacing. [BRAND] logo bottom right. 4:5 aspect ratio.

---

## 06 — Social Proof

**When to use:** Member count + review card + press logos. The trust stack.
**Reference:** `references/06-social-proof.png`
**Source template:**
> Use the attached images as brand reference. Create: a social proof ad on [BACKGROUND like warm cream]. Top: "[HEADLINE like Join 1,000,000+ Members]" in bold [BRAND COLOR]. Five filled stars with "Rated [X] out of 5". Center: [YOUR PRODUCT] at 50mm f/4. Below: frosted white card with five-star rating, "[REVIEW TITLE]", "[2-3 SENTENCE REVIEW]", "[ATTRIBUTION]" in italic. Below card: "As Featured In" with five grayscale logos. [BRAND] logo bottom right. 4:5 aspect ratio.

---

## 07 — Us vs Them

**When to use:** Side-by-side comparison. Photography quality gap IS the argument.
**Reference:** `references/07-us-vs-them.png`
**Source template:**
> Use the attached images as brand reference. Create: a side-by-side divided vertically. Left: muted gray-blue background. Right: [PRIMARY BRAND COLOR]. Center top: white circle with "VS". Left header: "[COMPETITOR CATEGORY]" + generic competitor product + list with X marks: "[WEAKNESS 1-5]". Right header: "[YOUR BRAND]" + [YOUR PRODUCT] + list with checkmarks: "[STRENGTH 1-5]". [BRAND] logo bottom right. 4:5 aspect ratio.

---

## 08 — Before & After (UGC Native)

**When to use:** Mirror selfie transformation. Must look like a real person posted it.
**Reference:** `references/08-before-after-ugc-native.png`
**Source template:**
> Use the attached images as brand reference for product color ONLY. This should look like a real person's post. Create: TikTok before-and-after. LEFT: grainy iPhone mirror selfie, [PERSON] in dimly lit bathroom, [BEFORE STATE], harsh lighting. White handwritten text: "[BEFORE DATE]". RIGHT: same person, same bathroom, bright natural light, [AFTER STATE], [PRODUCT] visible on counter. White text: "[AFTER DATE]". Top center: "[TIMEFRAME] on [BRAND]" with emoji. Should look stitched in CapCut. 9:16 aspect ratio.

---

## 09 — Negative Marketing (Bait & Switch)

**When to use:** Fake bad review that's actually a rave. Scroll-stopper.
**Reference:** `references/09-negative-marketing.png`
**Source template:**
> Use the attached images as brand reference. Create: Background is close-up of [PRODUCT], slightly blurred. Center: white rounded-rectangle review card (Amazon-style). Gray user icon, "[NAME]", one gold star + four gray, orange "Verified Purchase" badge, bold text: "[BAIT that sounds negative but is positive]". Bottom: bold white sans-serif "[PUNCHLINE like THE REVIEWS ARE IN.]". [BRAND] logo bottom right. 4:5 aspect ratio.

---

## 10 — Press / Editorial

**When to use:** Authority play. Vogue back-page energy.
**Reference:** `references/10-press-editorial.jpeg`
**Source template:**
> Use the attached images as brand reference. Create: a press ad on off-white linen background. Top: "As Featured In" in small [BRAND COLOR] uppercase wide-tracked text. Below: five grayscale publication logos. Center: italic serif pull-quote in [BRAND COLOR]: "[PRESS QUOTE]" with attribution. Lower third: [PRODUCT] at 85mm f/2.8, soft side light. [BRAND] logo bottom left. Generous white space. Full-page Vogue energy. 4:5 aspect ratio.

---

## 11 — Pull-Quote Review Card

**When to use:** Emotional quote headline over a truncated review card on a color block. The "...Read more" creates an open loop.
**Reference:** `references/11-pull-quote-review-card.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product colors and brand tone precisely. Create: a review-driven ad with a solid [BRAND COLOR with hex — a soft, muted tone works best] color block background filling the entire image. Top half: large bold italic serif text in white with curly quotation marks reading "[PULL-QUOTE — the most emotional 4-8 word phrase from the review, e.g., "I finally found something that works!"]". Directly below the quote: five large filled gold/yellow star icons in a horizontal row. Bottom left, overlapping the color background: a white rounded-corner review card with subtle shadow, containing: a small gray circular default avatar icon, beside it "[FIRST NAME + LAST INITIAL]" in bold dark sans-serif with a small [FLAG EMOJI matching target market — e.g., 🇺🇸], below the name a blue checkmark icon with "[VERIFIED REVIEWER / VERIFIED BUYER]" in small blue text. Below the reviewer info: the review body text in medium-weight dark sans-serif, 4-6 lines of authentic-sounding customer voice that trails off mid-sentence, ending with "...Read more" in bold [BRAND COLOR] text. Below the review text: "Was this review helpful?" in small gray text with a thumbs-up icon and "[HELPFULNESS COUNT — e.g., 150 / 2.4K]" beside it. Bottom right, overlapping both the card and the color background: [YOUR PRODUCT — full packaging description] angled slightly toward the viewer, sitting on the color block surface with a subtle shadow beneath. No brand logo needed if the product packaging already shows it. 1:1 or 4:5 aspect ratio.

---

## 12 — Lifestyle Action + Product Colorway Array

**When to use:** Action hero shot sells the use case. Fanned product lineup sells the range. Two conversion jobs in one frame.
**Reference:** `references/12-lifestyle-action-colorway-array.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and visual tone precisely. Create: a static ad with a [LIFESTYLE PHOTO DESCRIPTION like man mid-golf-swing in tropical patterned polo and khaki pants] occupying the left two-thirds of the frame, shot outdoors in [SETTING like golf course with palm trees], bright natural daylight. [BRAND] logo top center in bold. Below logo: large bold sans-serif quote text reading "[ENDORSEMENT HEADLINE like THE GREATEST PANTS IN GOLF]" in [TEXT COLOR like white or black]. Bottom right foreground: three [PRODUCT VARIANTS like folded pairs of shorts/pants] fanned in an overlapping arrangement showing [COLOR 1], [COLOR 2], and [COLOR 3]. Products are crisp and studio-lit against the lifestyle background. Shot on 50mm f/2.0, lifestyle background slightly softer than foreground product. [MOOD like confident, athletic, aspirational but accessible]. 1:1 aspect ratio.

---

## 13 — Stat Surround / Callout Radial (Product Hero)

**When to use:** Product is the sun. Stats are the planets. Arrows make it scannable in under 2 seconds.
**Reference:** `references/13-stat-callout-radial-product-hero.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and typography style precisely. Create: a static ad on a white-to-[LIGHT GRADIENT COLOR like warm golden beige] gradient background, gradient fading from top to bottom. Top: large bold [TEXT COLOR like dark brown] sans-serif headline reading "[HEADLINE like So tasty you'll forget it's actually healthy.]" Center: [YOUR PRODUCT like single stand-up pouch] on white background, soft studio lighting. Floating near the product: a small circular badge reading "[PRICE POINT like AS LOW AS $2.63 PER MEAL!]" in [BADGE COLOR]. Flanking the product on both sides: four stat callouts with curved arrows pointing toward the product. Left side top: "[STAT 1 like 20g]" in oversized bold text with "[LABEL like PROTEIN]" below. Left side bottom: "[STAT 2 like 280]" with "[LABEL like CALORIES]". Right side top: "[STAT 3 like 900k+]" with "[LABEL like HAPPY CUSTOMERS]". Right side bottom: "[STAT 4 like 30k+]" with "[LABEL like 5-STAR REVIEWS]" and five filled gold stars beneath. Arrows are simple hand-drawn-style curved lines in [ARROW COLOR like black]. Bottom foreground: [FLAVOR PROPS like scattered chocolate chip cookie dough balls and chocolate chips] adding appetite appeal. No brand logo. Clean, informational, appetizing. 1:1 aspect ratio.

---

## 14 — Bundle Showcase + Benefit Bar

**When to use:** Sells the system, not the SKU. The open box is the hero. The benefit bar handles the "what's in it for me."
**Reference:** `references/14-bundle-showcase-benefit-bar.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and typography style precisely. Create: a static ad on a [BACKGROUND like soft pink-to-hot-pink gradient] background. Top: oversized bold white all-caps sans-serif headline reading "[HEADLINE like 24/7 PEAK FEMALE PERFORMANCE]". Below headline: a horizontal [ACCENT COLOR like purple/violet] banner bar divided into [NUMBER like five] equal segments separated by thin vertical lines, each containing a two-word benefit label in white text: "[BENEFIT 1 like Morning Energy]", "[BENEFIT 2 like Focus Amplifier]", "[BENEFIT 3 like Deep Sleep]", "[BENEFIT 4 like Ultimate Beauty]", "[BENEFIT 5 like Metabolism Booster]". Center-to-bottom: an open [PACKAGING like branded gift box] photographed at a slight overhead angle showing [NUMBER like three] [PRODUCTS like supplement jars] nestled inside, each a different [COLOR-CODED VARIANT]. In the lower foreground: a [LIFESTYLE PROP like woman's hand holding a pastel dumbbell] entering the frame from bottom. [BRAND] logo bottom left corner. Bright studio lighting, saturated color, energetic and empowering. 1:1 aspect ratio.

---

## 15 — Social Comment Screenshot + Product

**When to use:** Screenshotted comment = instant credibility. Looks organic. Bold hook headline does the scroll-stopping.
**Reference:** `references/15-social-comment-screenshot.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and colors precisely. Create: a static ad on a clean white background. Top: oversized bold black sans-serif headline reading "[HOOK HEADLINE like IF YOU'RE GOING THROUGH PERI..]" with [EMOJI like 😅] at the end. Center: a social media comment card with light gray rounded-rectangle background containing: a small circular profile avatar (top left), bold name "[REVIEWER NAME like Elaine McLean]", and a multi-sentence review in regular-weight sans-serif: "[FULL REVIEW TEXT, 3-4 sentences, conversational and emotional]". Small gray timestamp "[TIMESTAMP like 2d]" below the comment. Bottom center: [YOUR PRODUCT like product box and bar/bottle] photographed at a slight angle on white, soft studio lighting. No brand logo. No stars. The rawness is the point. 1:1 aspect ratio.

---

## 16 — Curiosity Gap / Hook Quote Testimonial

**When to use:** The bait-and-switch testimonial. Provocative headline forces the double-take. The reveal reframes it. Scroll-stop machine.
**Reference:** `references/16-curiosity-gap-hook-quote.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and typography style precisely. Create: a static ad on a clean white background. Top center: large [ACCENT COLOR like periwinkle blue] opening quotation marks. Below: mixed-weight headline in black — the first line in italic serif or semi-bold reading "[SETUP LINE like I've been]", the next two lines in enormous heavy-weight bold all-caps sans-serif reading "[BAIT PHRASE like FAKING IT / WITH MY HUSBAND]", followed by a smaller sentence-case line reading "[REVEAL like since perimenopause — with [BRAND] I don't have to]". Closing quotation marks and "[ATTRIBUTION like - Erin D.]" in regular weight. Left side bottom third: [YOUR PRODUCT like supplement bottle] at a slight angle with [PRODUCT DETAILS like capsules scattered nearby]. To the left of the product: a [TRUST BADGE like circular seal reading "Happiness 60 DAY Guaranteed"]. Right side bottom third: [NUMBER like five] filled [ACCENT COLOR] stars and bold text reading "[REVIEW COUNT like 3,600+] 5-Star Reviews" with a [BRAND ICON]. Bottom edge: small disclaimer text. 1:1 aspect ratio.

---

## 17 — Verified Review Card

**When to use:** Mimics real review platform UI. The verified badge and helpfulness count do the trust-building work.
**Reference:** `references/17-verified-review-card.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and typography style precisely. Create: a static ad on a solid [BRAND COLOR like periwinkle/lavender blue] background. Top: large bold white serif or semi-bold sans-serif pull-quote reading "[HEADLINE QUOTE like "I finally found something that works!"]" in quotation marks. Below the quote: five filled gold stars, large. Center-left: a white rounded-rectangle review card with subtle shadow containing: gray circular avatar icon, bold name "[REVIEWER NAME like Dawn K.]" with [FLAG EMOJI like 🇺🇸], blue checkmark and "[VERIFIED BADGE TEXT like Verified Reviewer]" in [BRAND COLOR] text, then 3-4 sentences of review body text in regular weight dark text. At the bottom of the card: a blue "[READ MORE like ...Read more]" link and "[HELPFULNESS like Was this review helpful? 👍 150]". Right side, overlapping the card edge: [YOUR PRODUCT like cream jar with lid] photographed at a slight angle, soft studio lighting, casting a gentle shadow on the background. No brand logo. The review UI is the trust mechanic. 1:1 aspect ratio.

---

## 18 — Stat Surround / Callout Radial (Lifestyle Flatlay)

**When to use:** Same stat-radial format as #13, but lifestyle flatlay background and hand-held product sell the experience, not just the specs.
**Reference:** `references/18-stat-callout-radial-lifestyle.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and typography style precisely. Create: a static ad on a white background with a lifestyle flatlay arrangement. Top: bold [ACCENT COLOR like purple] filled banner bar spanning full width, white all-caps sans-serif reading "[HEADLINE like INCREDIBLY TASTY BREAKFAST IN 30 SECONDS]". Center: a [PERSON DETAIL like woman's hand] holding [YOUR PRODUCT like branded shaker cup] in mid-frame. Scattered around the edges: [FLATLAY PROPS like product sachets, pancakes on plates, blueberries, muffins, fruit] arranged organically to fill corners and edges, slightly out of focus. Four stat callouts with curved [ACCENT COLOR] arrows pointing toward the held product: "[STAT 1 like 20g] / [LABEL like PROTEIN]", "[STAT 2 like 900K] / [LABEL like HAPPY CUSTOMERS]", "[STAT 3 like 20+] / [LABEL like FLAVORS]", "[STAT 4 like 30K] / [LABEL like 5 STAR REVIEWS]" with five small gold stars. Stats in bold black, labels in all-caps regular weight. Bright, flat studio lighting. 1:1 aspect ratio.

---

## 19 — Highlighted / Annotated Testimonial

**When to use:** The highlighter pen does the work. Long-form review text with key phrases visually emphasized. Directs the eye to the money lines.
**Reference:** `references/19-highlighted-annotated-testimonial.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and typography style precisely. Create: a static ad on a clean white background. Top left: circular customer headshot photo of [PERSON DESCRIPTION like smiling woman, mid-60s, silver wavy hair, wearing blue top]. To the right of the photo: bold name "[REVIEWER NAME like Veronica B.]" with a [VERIFIED ICON like blue checkmark]. Below: a long-form customer quote in large regular-weight black sans-serif type spanning most of the frame: "[FULL QUOTE, 3-5 sentences]". Key phrases within the quote are highlighted with [HIGHLIGHT COLOR like bright lime green / neon yellow] rectangular background fills behind the text: "[HIGHLIGHTED PHRASE 1 like thyroid removed]", "[HIGHLIGHTED PHRASE 2 like This is the best product I have found.]" Bottom right: [YOUR PRODUCT like supplement jar] at a slight angle, partially cropped at the bottom edge. To the left of the product: a circular [TRUST BADGE like "100% MONEY BACK / 90 DAYS / 100% GUARANTEE"] seal in [BADGE COLOR like lime green with dark text]. [BRAND] logo bottom left corner, small. 1:1 aspect ratio.

---

## 20 — Advertorial / Editorial Content Card

**When to use:** Looks like a news post, not an ad. The editorial framing creates curiosity and authority. Designed to blend into feed.
**Reference:** `references/20-advertorial-editorial-card.png`
**Source template:**
> Use the attached images as brand reference for tone ONLY. Do NOT use polished ad layouts. This should look like organic editorial content. Create: a full-bleed moody portrait photo of [PERSON DESCRIPTION like young man in dark textured sweater holding an electric guitar], warm amber-toned lighting, shot on 50mm f/1.8, shallow depth of field, cinematic color grade with warm highlights and cool shadows. Lower 45% of the frame is a text overlay zone: a prominent white rounded-rectangle pill label reading "[CATEGORY TAG like HOT TOPIC]" in centered uppercase sans-serif, sized to span roughly one-third of the frame width. Below: very large, dominant, bold all-caps condensed sans-serif headline filling the width of the frame in white text with key words in [HIGHLIGHT COLOR]: "[HEADLINE like [BRAND] IS BLOWING UP ON TIKTOK — HERE'S WHY EVERYONE'S USING IT TO [ACTION]]". The headline should be oversized — at least 35% of the total frame height. Bottom center: "[@HANDLE like @waveform.watch]" in small white text. No product shot, no CTA button, no stars. 4:5 aspect ratio.

---

## 21 — Bold Statement / Reaction Headline

**When to use:** Pure brand energy. No stats, no reviews — just a provocative line, a hot gradient, and the product. The copy IS the ad.
**Reference:** `references/21-bold-statement-reaction-headline.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and visual tone precisely. Create: a static ad on a vibrant [GRADIENT like coral-pink to golden-yellow] gradient background, flowing diagonally from upper left to lower right. Upper left: oversized playful [FONT STYLE like rounded retro serif / Cooper Black style] white headline reading "[BOLD STATEMENT like This popcorn is so f*****g delicious.]" — text should feel loose, fun, and expressive, not rigid or corporate. Right side: [PERSON DETAIL like a hand reaching down from upper right] grabbing from [YOUR PRODUCT like the signature bright yellow popper bowl overflowing with fluffy popcorn]. Product sits center-right, slightly below midline. Bottom left: [BRAND] logo in [LOGO COLOR like black] with "[PRODUCT DESCRIPTOR like Flavor Wrapped Popcorn Kernels]" in smaller text below. No stats, no reviews, no badges. The gradient and the headline do all the work. 1:1 aspect ratio.

---

## 22 — Flavor Story / "Tastes Like"

**When to use:** Indulgent food hero in the background, product as the payoff. For food/drink brands selling flavor association.
**Reference:** `references/22-flavor-story-tastes-like.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, colors, and packaging precisely. Create: a flavor-visualization ad. Full background is a photorealistic close-up food scene of [INDULGENT FOOD like freshly baked raspberry donuts dusted with powdered sugar on a gray stone surface]. Shot at 50mm f/2.8, shallow depth of field, warm bakery lighting. Top third: large bold white sans-serif headline reading "[HEADLINE like A protein bar that tastes like freshly baked raspberry donuts]" with one key word in bold italic for emphasis. [YOUR PRODUCT] packaged unit placed bottom-right, angled 15° as if casually laid into the scene. Bottom: semi-transparent white bar spanning full width with three stat columns separated by thin vertical lines: "[STAT 1 like 15g PROTEIN]" | "[STAT 2 like 2g SUGAR]" | "[STAT 3 like 180 CALORIES]". Very bottom edge: smaller bold sans-serif "[CLEAN LABEL CLAIM like NO ARTIFICIAL SWEETENERS]". Food is the hero — product is the payoff. 1:1 aspect ratio.

---

## 23 — Long-Form Manifesto / Letter Ad

**When to use:** Copy-only manifesto. For brands where the writing IS the ad. High-conviction, premium-positioning play.
**Reference:** `references/23-long-form-manifesto-letter.png`
**Source template:**
> Use the attached images as brand reference. Match exact brand typography style and tone. Create: a copy-dominant manifesto ad on a clean white background. No background imagery — text is the entire creative. Top: oversized bold black serif or sans-serif headline reading "[PROVOCATIVE HEADLINE like They're not cheap.]" spanning the top 15%. Below: left-aligned body copy in smaller regular-weight black text, structured as short punchy sentences and line breaks (NOT paragraphs), building a persuasive argument about [CORE BRAND TENSION like why the price is justified]. The copy should flow through: acknowledging the objection, listing what you'd lose if they cut corners, reframing as a positive, closing with a confident brand statement. Approximately [12-18 LINES] of copy. Bottom 20%: [YOUR PRODUCT] centered or slightly right, product-only on white, clean studio shot angle. No icons, no badges, no CTA button. 1:1 aspect ratio.

---

## 24 — Product + Comment Callout (Faux Social Proof)

**When to use:** Clean product hero up top, FB-style comment card below. Social-proof play that still respects the product shot.
**Reference:** `references/24-product-comment-callout.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and packaging precisely. Create: a social proof ad. Top 55%: [YOUR PRODUCT] centered on a clean white background, studio-lit, shot at 85mm f/2.8 with soft shadow. Product cap/lid slightly open or [DETAIL showing use]. A few [LOOSE UNITS like gummies / capsules] spilling casually at the base. Bottom 45%: a realistic Facebook-style comment card. Left: small circular profile photo of [PERSON DESCRIPTION like a man in his 30s, friendly smile, casual]. Bold name "[FIRST NAME + LAST INITIAL like Alan R.]" above the comment. Comment text in regular weight: "[TESTIMONIAL 2-3 sentences touching on a specific problem and the product being a game-changer]". Below comment: "[TIMEFRAME like 4w]" · Like · Reply in gray. Bottom right of comment: Facebook-style reaction emojis (thumbs up + heart) with "[COUNT like 33]". Should look like an organic screenshot. 1:1 aspect ratio.

---

## 25 — Us vs Them Color Split

**When to use:** Cleaner, more graphic variant of #07. Color-blocked halves. Punchier when the comparison is the core argument.
**Reference:** `references/25-us-vs-them-color-split.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and colors precisely. Create: a side-by-side comparison ad divided vertically into two equal halves. Left half: [PRIMARY BRAND COLOR like vibrant teal] background. [YOUR PRODUCT] photographed with dynamic energy — [ACTION DETAIL like chocolate dripping / liquid pouring] to convey indulgence. Brand logo in bold white upper-left. Below product: vertical stack of 4 benefits, each with a green circle checkmark emoji: "[STRENGTH 1-4 like ≤2G SUGAR / ALL NATURAL INGREDIENTS / ≥6G FIBRE / 12G PROTEIN]" in bold white uppercase. Right half: [CONTRAST COLOR like pale cream/beige] background. Generic unbranded competitor product [DESCRIPTION like crumpled foil-wrapped chocolate bar]. Header: "[COMPETITOR CATEGORY like Other chocolate bars]" in dark text. Below: vertical stack of 4 weaknesses, each with a red circle X emoji: "[WEAKNESS 1-4 like 29G SUGAR / FULL OF FRUCTOSE CORN SYRUP / 1G FIBRE / 2G PROTEIN]" in bold dark uppercase. Center divider: a comic-style "VS" burst graphic in [ACCENT COLOR like coral/red]. 1:1 aspect ratio.

---

## 26 — Stat Callout (Data-Driven Lifestyle)

**When to use:** Lifestyle photo top, stat-driven headline bottom. For brands that lead with hard numbers and audience-result claims.
**Reference:** `references/26-stat-callout-data-driven-lifestyle.png`
**Source template:**
> Use the attached images as brand reference. Match exact brand colors and typography. Create: a statistic-led ad. Top 50%: lifestyle product photography — [SCENE like a woman's hands holding the product pad / person using the product in context] on a [MOOD like warm, skin-toned, soft-focus] background. Product packaging visible in frame. Middle: brand logo centered with thin horizontal rules on either side as a visual divider. Bottom 50%: dark gradient overlay (fading from transparent to [DARK COLOR like deep brown/black]). Large bold uppercase sans-serif text: "[STAT-DRIVEN HEADLINE with specific percentages like AFTER SWITCHING TO [BRAND], [X]% OF USERS [RESULT], WHILE [Y]% [SECOND RESULT]]." Key result phrases highlighted in [ACCENT COLOR like soft pink / brand secondary color]. 4:5 aspect ratio.

---

## 27 — Benefit Checklist Showcase (Split Product + Info)

**When to use:** Heavy-info ad. Product overhead on the left, full benefit/star/CTA stack on the right. Good for landing-page-style scrollers.
**Reference:** `references/27-benefit-checklist-split.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and brand colors precisely. Create: an information-dense benefit ad, split composition. Left 45%: overhead product shot — [PRODUCT DISPLAY DESCRIPTION like a white bowl filled with fresh dog food divided into labeled sections by thin white lines, each section labeled in curved white text: "[VARIETY 1-4 like CHICKEN & YAMS / BEEF N' RICE / SALMON N' RICE / TURKEY & YAMS]"]. Shot on 50mm f/4, clean white surface. Right 55%: white background. Top: [STAR RATING like five gold stars] with "[REVIEW COUNT like 10,000+ REVIEWS]" in [BRAND COLOR]. Brand logo. Below: [BRAND COLOR] serif or sans-serif headline: "[HEADLINE like Made for the pickiest dogs]". Then 3 checkmark benefit rows, each with a filled [BRAND COLOR] circle checkmark + bold text: "[BENEFIT 1-3]". Bottom right: large rounded [ACCENT COLOR] CTA button reading "[CTA like SHOP NOW]". 1:1 aspect ratio.

---

## 28 — Feature Arrow Callout / Product Annotation

**When to use:** Hand-held product, curved arrows pointing to four benefit labels. Annotated-product play — softer than #04's diagram look.
**Reference:** `references/28-feature-arrow-callout.png`
**Source template:**
> Use the attached images as brand reference. Match exact brand colors and typography style. Create: a product annotation ad on a [BACKGROUND like warm cream/light yellow textured] background. Top: italic serif headline "[BENEFIT STATEMENT like Barista grade coffee. Instant. Affordable.]" in [BRAND COLOR like dark navy]. Below in massive bold sans-serif: "[VALUE PROP like ALL IN ONE]". Center: [PERSON'S HAND] holding [YOUR PRODUCT] at a natural angle. Four curved arrows in [BRAND COLOR] pointing from the product outward to four benefit callout labels arranged around it in bold [BRAND COLOR] text: "[CALLOUT 1-4]". Arrows should feel hand-drawn or editorial, not rigid. Bottom: full-width [CONTRAST COLOR like deep navy] banner with [PROMO TEXT like HUGE SALE + FREE GIFTS] in bold [ACCENT COLOR like gold/white] with optional emoji accents. 1:1 aspect ratio.

---

## 29 — UGC + Viral Post Overlay

**When to use:** Casual selfie + overlaid Reddit/Twitter post. Reaction format — the opinion is the hook, no product shown.
**Reference:** `references/29-ugc-viral-post-overlay.png`
**Source template:**
> Use the attached images as brand reference for product color ONLY. Do NOT use ad layouts or polish. This should look completely native and organic. Create: a casual selfie of [PERSON like a man in mid-20s, beanie, crewneck sweatshirt] doing something mundane [ACTION like brushing teeth, making coffee, cooking]. iPhone front camera, slightly grainy, bathroom/kitchen lighting, no professional setup. Overlaid in the center of the frame: a realistic screenshot of a [PLATFORM like Reddit / Twitter / X] post. [POST DETAILS like subreddit name, username, timestamp, upvote count]. Post title in bold: "[PROVOCATIVE OPINION HEADLINE related to the product's problem/benefit space]". Post body in regular text: "[2-3 sentences expanding on the opinion]". The post should feel like the person is reacting to it or sharing it — NOT selling a product. No product visible. No brand logo. No CTA. The hook is the opinion. 9:16 aspect ratio.

---

## 30 — Hero Statement + Icon Benefit Bar

**When to use:** Short power statement, big lifestyle hero, three-icon benefit bar bottom. Clean direct-response sandwich.
**Reference:** `references/30-hero-statement-icon-benefit-bar.png`
**Source template:**
> Use the attached images as brand reference. Match exact brand colors, packaging, and typography. Create: a bold statement ad. Top 15%: white banner overlay with massive bold [BRAND COLOR like dark purple] uppercase sans-serif headline: "[2-3 WORD POWER STATEMENT like APPETITE KILLER.]" with a period for punch. Middle 55%: lifestyle product photo — [SCENE like woman's hand holding a capsule above an open supplement jar on a clean surface, soft natural light]. Product label and branding clearly visible. Bottom 25%: [SOFT BRAND COLOR like lavender/light purple] background. Three evenly spaced icon-and-text benefit columns: [ICON 1 + LABEL] | [ICON 2 + LABEL] | [ICON 3 + LABEL]. Icons are simple line-drawn in [BRAND COLOR] circles. Very bottom edge: scrolling ticker bar in [DARK BRAND COLOR] with repeating text: "[SOCIAL PROOF like OVER 300K+ LIVES CHANGED]". 1:1 aspect ratio.

---

## 31 — Comparison Grid / Table

**When to use:** Meme-format comparison table. Two products top, three rows of you-vs-them attribute lines. Goes viral on X/Reddit.
**Reference:** `references/31-comparison-grid-table.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product packaging precisely. Create: a structured comparison grid ad on white background. Top row divided 50/50: Left: [YOUR PRODUCT] packaging photographed clean on white with [DETAIL like chips spilling out]. Right: [COMPETITOR PRODUCT] packaging on white. Below: three horizontal rows spanning full width, each divided 50/50 by a thin black vertical line and separated by thin black horizontal lines. Each row compares one attribute: Row 1: "[YOUR ADVANTAGE]" vs "[COMPETITOR WEAKNESS]". Row 2: "[YOUR ADVANTAGE]" vs "[COMPETITOR WEAKNESS]". Row 3: "[YOUR ADVANTAGE]" vs "[COMPETITOR WEAKNESS]". All text in bold black serif or heavy sans-serif, centered in each cell. No icons, no colors, no checkmarks — the copy contrast does the work. 1:1 aspect ratio.

---

## 32 — UGC Story Callout / Text Bubble Explainer

**When to use:** Faked-IG-Story layout. Hand-held product + 5 highlighted text bubbles. Native education format.
**Reference:** `references/32-ugc-story-text-bubble.png`
**Source template:**
> Use the attached images as brand reference for product color and packaging ONLY. Do NOT use ad layouts or polish. This must look like a real person's Instagram Story. Create: a casual iPhone photo of [PERSON DESCRIPTION like a woman's hand with clean natural nails] holding [YOUR PRODUCT with key packaging details] at a slight angle over [SURFACE/SETTING like a clean white desk with lifestyle props]. Natural overhead daylight, slightly warm, iPhone 15 quality. Scattered across the frame: 5 text bubbles using Instagram Story's built-in highlighted text tool. Each bubble must have a highlighted background for readability, with varied highlight colors between bubbles. Bubble 1: "[TOPIC + EMOJI]". Bubble 2: "[EDUCATIONAL HOOK]". Bubble 3: "[WHY THIS PRODUCT]". Bubble 4: "[PERSONAL RESULT]". Bubble 5: "[BRAND ENDORSEMENT]". Should feel casual and hand-placed, not designed. 9:16 aspect ratio.

---

## 33 — Faux Press / News Article Screenshot

**When to use:** Looks like a Daily Mail / TODAY article screenshot with two casual UGC photos. Looks shared, not advertised.
**Reference:** `references/33-faux-press-news-article.png`
**Source template:**
> Use the attached images as brand reference. Create: a static ad designed to look like a real online news article screenshot. Top 25%: white background with a realistic major publication masthead/logo in large bold black serif text [PUBLICATION STYLE like "Daily Mail" or "TODAY" or "INSIDER"]. Below: thin gray horizontal rule. Small gray text "Latest Headlines" left-aligned. Then: bold black serif headline spanning full width: "[HEADLINE like 'It's my holy grail product': The $[PRICE] [PRODUCT CATEGORY] with over [NUMBER] five-star reviews]". Bottom 60%: two side-by-side casual UGC-style photos of [PEOPLE like two different young women, one brunette, one blonde] each holding [YOUR PRODUCT] in a casual selfie pose — one taken in natural daylight, one in warm indoor evening light. Photos should look like real customer submissions, not studio shots. No brand logo. No CTA. 4:5 aspect ratio.

---

## 34 — Faux iPhone Notes / App Screenshot

**When to use:** Disguised as iOS Notes. Headline + checklist + product breaking the frame. Stops the scroll with format mimicry.
**Reference:** `references/34-faux-iphone-notes.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and packaging precisely. Create: a static ad disguised as an iPhone Notes app screenshot. Top: realistic iOS status bar (time, signal bars, wifi icon, battery icon). Below: iOS Notes navigation — blue "< All iCloud" back arrow left, share icon and three-dot menu icon right. Below nav: small gray timestamp text. Main content area on white background: bold black serif headline "[HEADLINE]". Below: [3 BENEFIT ROWS], each with a [BRAND COLOR] filled circle checkmark + [EMOJI] + bold black text using food-equivalency format: "[BENEFIT 1]" / "[BENEFIT 2]" / "[BENEFIT 3]". Right side, overlapping the benefit text slightly: [YOUR PRODUCT] at a slight angle with [DETAILS like a few gummies/capsules spilling out at the base]. Product should feel casually placed into the note layout, breaking the frame slightly. 1:1 aspect ratio.

---

## 35 — Hero Product Showcase + Stat Bar

**When to use:** Superlative claim headline, hero product with exploded ingredient/crumb pattern, three-stat bar bottom. Strong general-purpose showcase.
**Reference:** `references/35-hero-product-stat-bar.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design, wrapper, and brand colors precisely. Create: a product showcase ad on a [BACKGROUND COLOR like warm sand/beige/cream] background. Top: large bold [BRAND COLOR like chocolate brown] uppercase sans-serif headline: "[SUPERLATIVE CLAIM like THE WORLD'S HEALTHIEST CHOCOLATE]". Below headline: white rounded-rectangle CTA button with [BRAND COLOR] uppercase text "[CTA like EXPLORE NOW]". Center: [YOUR PRODUCT] in full packaging, angled slightly, hero-lit with soft studio lighting. Surrounding the product: [SCATTERED ELEMENTS like broken chocolate pieces, cocoa powder dust, crumbs, ingredient pieces] arranged in an exploded/radial pattern. Bottom: a white or light rounded-pill stat bar spanning the width with three metrics separated by thin vertical lines: "[STAT 1]" | "[STAT 2]" | "[STAT 3]". Numbers should be large and dominant, labels smaller below. 1:1 aspect ratio.

---

## 36 — Whiteboard Before / After + Product Hold

**When to use:** Hand-drawn before/after on a whiteboard + handheld product. Casual educational play — sells the mechanism.
**Reference:** `references/36-whiteboard-before-after-product-hold.png`
**Source template:**
> Use the attached images as brand reference for product packaging ONLY. Do NOT use ad layouts or polish. This should look like a real person's photo. Create: a lifestyle photo set in [SETTING like a bright modern kitchen]. In the background: a small tabletop dry-erase whiteboard or flip-chart propped up on [SURFACE]. On the whiteboard: two simple hand-drawn black marker line illustrations side by side — left drawing labeled "[BEFORE LABEL]" showing [BEFORE STATE], an arrow pointing right to a second drawing labeled "[AFTER LABEL]" showing [AFTER STATE]. Below the drawings on the whiteboard: handwritten text in black marker "[HANDWRITTEN CTA]". In the foreground: [PERSON'S HAND] holding [YOUR PRODUCT] up next to the whiteboard, positioned in the lower-right of the frame. Product label clearly visible. Shot on iPhone, natural kitchen lighting, casual and educational. 4:5 aspect ratio.

---

## 37 — Hero Statement + Icon Bar + Offer Burst (Promo Variant)

**When to use:** Promo variant of #30. Moody dark background + neon starburst discount badge + offer ticker. For sales windows.
**Reference:** `references/37-hero-statement-promo-variant.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and brand colors precisely. Create: a promotional variant of a hero statement ad on a [BACKGROUND like dark charcoal/moody gray] background. Top 12%: white or light banner with massive bold [DARK COLOR] uppercase sans-serif headline: "[PROVOCATIVE 2-3 WORD STATEMENT]" with a period for punch. Upper-left of product area: a [BRIGHT ACCENT COLOR like neon green/lime] comic-style starburst badge rotated slightly, reading "GET UP TO [DISCOUNT like 40%] OFF" in bold black text. Center: [PERSON'S HAND] gripping [YOUR PRODUCT] from above, lifting it off its [PACKAGING like retail box] below. Product label and branding clearly visible. Moody, slightly dramatic lighting. Bottom 20%: three evenly spaced icon-and-text benefit columns on a semi-transparent dark strip: [ICON 1 + LABEL] | [ICON 2 + LABEL] | [ICON 3 + LABEL]. Very bottom: full-width [BRIGHT ACCENT COLOR] banner with bold [DARK] text: "[PROMO like BLACK FRIDAY SPECIAL]". 1:1 aspect ratio.

---

## 38 — UGC Lifestyle + Product + Review Card (Split)

**When to use:** Left = candid lifestyle photo, right = brand-color block with product hero + short review card. Hybrid UGC/clean format.
**Reference:** `references/38-ugc-lifestyle-review-split.png`
**Source template:**
> Use the attached images as brand reference. Match the exact product design and brand colors precisely. Create: a vertical split social proof ad. Left 55%: a casual UGC-style photo of [PERSON like a blonde woman in her early 30s, wearing a casual zip-up sweater] enjoying the product in context — [ACTION like sipping an iced drink through a gold metal straw in a bright café setting]. Natural lighting, warm and inviting, iPhone-quality casual feel. The person should look genuinely happy, not posed. Right 45%: solid [PRIMARY BRAND COLOR like deep indigo/purple] background. Top-right: small decorative sparkle/star accents in [ACCENT COLOR like gold/yellow]. Floating center-right: [YOUR PRODUCT] at a slight angle, studio-lit on the colored background. Below product: a white rounded-rectangle review card with: five filled [ACCENT COLOR] stars at top, then italic or casual serif text: "[SHORT REVIEW QUOTE]" in [BRAND COLOR]. Bottom center: [BRAND LOGO] in white on the colored background, with small decorative sparkle accents. 4:5 aspect ratio.

---

## 39 — Curiosity Gap + Scroll-Stopper Hook

**When to use:** Top = bold truncated "...more" headline, bottom = uncomfortable problem photo. No product. Pure curiosity-bait.
**Reference:** `references/39-curiosity-gap-scroll-stopper.png`
**Source template:**
> Use the attached images as brand reference for visual tone ONLY. Do NOT include any product, logo, or branding. Create: a scroll-stopping curiosity ad designed to look like a truncated social media post. Top 35%: clean white background with large bold black sans-serif text (heavy weight, tight leading): "[HOOK HEADLINE like Most [AUDIENCE] don't realize THIS is why [PROBLEM STATEMENT] but did you know...]". The last few words should be followed by "...more" in lighter gray text, mimicking a truncated Facebook/Instagram caption that requires clicking "more" to expand. Bottom 65%: a close-up, slightly uncomfortable or attention-grabbing photo of [PROBLEM VISUAL]. Photo should feel real and editorial, not stock. Slightly shallow depth of field. No text on the photo. No product. No logo. No CTA. The entire purpose is to provoke curiosity and earn the click. 1:1 aspect ratio.

---

## 40 — Native / Ugly Post-It Note Style (Product Hero)

**When to use:** Found-looking lifestyle product photo with a handwritten post-it stuck to it. Highest "looks organic" format in the catalog.
**Reference:** `references/40-native-post-it-note.png`
**Source template:**
> Use the attached images as brand reference. Match the exact [PRODUCT DESCRIPTION — shape, color, label details, key typography on packaging] precisely. Create: a lifestyle product photo set in [REAL-LIFE SETTING like warm kitchen floor / bathroom counter / living room coffee table] with [LIGHTING DESCRIPTION like soft natural daylight from a nearby window] and a naturally blurred background showing [BACKGROUND DETAILS]. Frame is very slightly off-center — product not perfectly centered, [LEFT / BOTTOM / RIGHT] edge of product very slightly cropped — feels found rather than composed. Slight natural sensor grain consistent with a phone camera in indoor daylight. Subtle natural vignette at frame corners. Center of frame, large and dominant: [FULL PRODUCT DESCRIPTION] sitting on [SURFACE], slightly angled toward the viewer. [SCATTERED SURFACE DETAIL] on the surface around the base of the product for casual realism. Stuck directly onto the front face of the product: a [POST-IT COLOR — yellow default] square post-it note, slightly crooked and not perfectly straight — slightly trapezoid from the angle it was pressed on. Realistic paper texture with a horizontal crease across the middle as if it was folded once. Subtle curl at bottom-right corner only. Held at the top by a small piece of [TAPE COLOR] tape, slightly wrinkled. One word in the handwriting is slightly heavier-inked or underlined from natural pen pressure. Handwritten in thick black marker-style lettering, imperfect and uneven, lowercase natural writing — not formatted, not centered, not evenly spaced: "[LINE 1 — short, setup or hook]" line break "[LINE 2 — continuation or turn]" line break "[LINE 3 — punchline]" line break "[LINE 4 — optional final beat or emoji]" No attribution line. No signature. Bottom center of frame, outside the photo on a plain white or off-white strip: small plain lowercase sans-serif caption text: "[brand url] — [3-5 word casual caption, sounds typed not written]". No logo overlay anywhere in the frame. Brand identity carried entirely by the product packaging visible in the photo. No border. [MOOD — 3 adjectives]. 4:5 aspect ratio.
