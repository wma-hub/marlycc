# Marly Creative Concepts — Agency-Level Rules

## Studio Identity
Marly Creative Concepts is a productized ad production studio. All output must be production-ready, client-deliverable, and QA-verified before handoff.

---

## Output Formats

- **HTML5 Static Ads** — Single-frame, no animation. Pure HTML/CSS.
- **HTML5 Animated Ads** — CSS or GSAP animations. No video embeds unless explicitly scoped.

All ads are self-contained HTML files. No external CDN dependencies in final delivery unless explicitly approved by client.

---

## Standard Ad Sizes

| Label | Dimensions     | Use Case                        |
|-------|----------------|---------------------------------|
| 1x1   | 1080 × 1080 px | Social square (Instagram, Meta) |
| 4x5   | 1080 × 1350 px | Social portrait (Instagram)     |
| 9x16  | 1080 × 1920 px | Stories / Reels / TikTok        |
| 16x9  | 1920 × 1080 px | YouTube / Landscape              |
| 300×250 | 300 × 250 px | Display — Medium Rectangle      |
| 728×90  | 728 × 90 px  | Display — Leaderboard           |

Each size gets its own folder and its own PNG screenshot at 1x pixel density.

---

## Typography

- **Default fallback stack:** `system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`
- Client-specific fonts must be specified in the client-level `brand.md` before production begins.
- Fonts must be embedded via `@font-face` with local file references — no Google Fonts CDN in final delivery.
- Always specify `font-display: swap` for web fonts.

---

## Asset Rules

- All asset paths must be **absolute** within the delivery folder structure, or relative and verified.
- **No `localhost` references** in any final delivery file.
- No hotlinked images. All images must be local to the delivery folder.
- Compress images before delivery: JPEG ≤ 150KB, PNG ≤ 300KB per asset unless otherwise scoped.

---

## Delivery Format

Per project, deliver:
1. One **zipped HTML folder per ad size** (e.g., `client-campaign-1080x1080.zip`)
   - Contains: `index.html`, `/assets/` folder (images, fonts)
2. One **PNG screenshot per size** at actual pixel dimensions
3. Optional: a single **master ZIP** containing all sizes + screenshots

Naming convention: `[client]-[campaign]-[WxH].zip` — e.g., `acme-summer24-1080x1080.zip`

---

## Brief Intake Format

All briefs must be submitted using this field order:

```
[Format]        Static or Animated
[Size]          e.g., 1x1, 9x16, 300x250
[Background]    Color hex, gradient spec, or image reference
[Layout]        Description or reference image
[Content]       Headline, subhead, body copy, CTA
[Speakers]      (If applicable) Name, title, headshot file
[Remove/Exclude] Elements, logos, or copy to omit
```

Incomplete briefs are returned to the client before production starts.

---

## QA Standard

Every ad must pass the QA checklist in `../_shared/qa-checklist.md` before delivery. No exceptions.

---

## Animation Defaults (Animated Format)

- Default duration: 15s loop unless otherwise specified
- Easing: `ease-in-out` unless brand specifies
- No autoplay audio
- Respect `prefers-reduced-motion` with a static fallback

---

## Review Portal Rules

**The review portal is a separate repo and Vercel project from this one.**
- Repo: `wma-hub/marly-reviews` at `/Users/wma/Desktop/marly/reviews/`
- Deploys to: `review.marly.cc`
- Canonical template: `/Users/wma/Desktop/marly/reviews/_example-client/sample-campaign/index.html`
- Do NOT create or edit review pages in this repo (`marlycc` / `marly.cc`).

- Never use `<iframe>` in review pages.
- All ad units must be inlined with namespaced class prefixes.
- Review pages must be single self-contained HTML files with no relative file dependencies.
- Every review page `<head>` must include the Google Analytics snippet (G-NM1YHB24BE) immediately after the viewport meta tag. Copy from the canonical template above.

---

## Embedding Ads in index.html — Mandatory CSS Safety Rules

Ad HTML files are written as standalone documents. Before embedding any ad's `<style>` block into `index.html`, you MUST strip the following — they apply globally to the entire page and will break the layout:

1. **`body { ... }` rules** — ALWAYS remove. Standalone ad files use `body` to set dimensions and overflow; on the portfolio page these override the entire document body. Any `body { width, height, overflow, cursor }` rule embedded in a `<style>` block inside a `<div>` will affect every element on the page.

2. **Bare `* { ... }` or `*,*::before,*::after { ... }` resets** — ALWAYS remove. The page already declares a global `* { box-sizing: border-box; margin: 0; padding: 0; }` reset at the top of its main `<style>` block. Duplicate resets inside ad `<style>` blocks are redundant and can conflict.

3. **`@media (prefers-reduced-motion) { *, *::before, *::after { ... } }`** — Remove or scope to the ad's container class. A bare `*` selector inside this media query will disable ALL animations on the page for reduced-motion users, not just the ad's.

**Rule:** If you see `body`, `* {`, or `*,*::before,*::after {` in an ad's `<style>` block, strip it before adding the ad to `index.html`. No exceptions.

---

## Versioning

- Use suffixes for revisions: `v1`, `v2`, `v2b`
- Never overwrite a delivered version — archive it in `/archive/`

---

## Deployment Rules
- NEVER use `vercel deploy` or the Vercel CLI directly. This project auto-deploys from GitHub on push to main.
- To deploy, just `git push origin main` and stop. Vercel handles the rest.
- If a deploy seems stuck or failed, tell me — do NOT retry, loop, or attempt workarounds.
- NEVER make additional commits beyond what was explicitly requested. Ask before making changes I didn't ask for.
