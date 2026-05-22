# ADR-002: Color palette lock

**Status:** Accepted
**Date:** 2026-05-21
**Owner:** Vista (per Potter dispatch · Wave 3 Batch 1 · STORY-AURORA-013)
**Story:** TODO-237 · STORY-AURORA-013

## Context

Several stale planning documents reference an Aurora palette of `--ink: #0a0e1a` plus a yellow-amber accent `--amber: #f59e0b`. Those values were drafted before the v0.3 single-shot build and never made it into production. The live CSS at `index.html` (lines 172-183) has been carrying a different, intentionally-chosen palette since commit `8332987` (v0.3) and was refined through v0.3.1 (commit `cad6893`) and the Hero V6 updates.

Per HARD RULE `feedback_potter_existing_asset_check_before_directing_build_2026_05_21`: design against the live source, not against stale specs. This ADR locks the LIVE palette as canonical and supersedes any earlier planning-doc values.

## Decision

The Unruly Labs LP brand palette is locked to the following `:root` CSS custom-property values. These match the LIVE values in `index.html` as of commit `8bce6a2`.

```css
:root {
  /* Primary brand · dark surfaces · navigation · CTA backgrounds */
  --navy:        #0a2540;  /* primary brand / theme-color / hero base / footer */
  --navy-deep:   #061a2f;  /* deeper navy for prove-strip / footer floor */
  --navy-soft:   #13314f;  /* lifted navy for gradient stops + avatar gradient */

  /* Foreground / text on light surfaces */
  --ink:         #0d1b2a;  /* body text on paper */
  --ash:         #5a6b7b;  /* secondary text / labels / muted */

  /* Background / light surfaces */
  --paper:       #f7f8fa;  /* default background */
  --line:        #e4e8ee;  /* borders / dividers / tile outlines */

  /* Accent · CTA · highlight */
  --accent:      #ff7a00;  /* primary CTA / kicker / hero-eyebrow pulse */
  --accent-hot:  #ff5a1f;  /* hover state for primary CTA */
  --accent-soft: #ffe9d6;  /* light accent backgrounds (low-emphasis surfaces) */
}
```

The hero gradient text uses a 3-stop accent ramp `#ff7a00 → #ff9c3d → #ffd58a` (declared inline on `.hero-h1 .num`). These three values are part of the accent ramp; they are NOT new tokens.

### Theme-color meta tag

`<meta name="theme-color" content="#0a2540">` — locked to `--navy`. Mobile Safari + Chrome use this for the address-bar tint on the dark hero.

### Stale values explicitly superseded

The following stale planning-doc values are NOT part of the locked palette and MUST NOT be introduced into any new build or refactor without a successor ADR:

- `#0a0e1a` (stale ink) → use `#0d1b2a` (LIVE `--ink`)
- `#f59e0b` (stale amber accent) → use `#ff7a00` (LIVE `--accent`)

## Rationale

- **Already live.** The palette has been on `unrulylabslp.com` since v0.3 and through v0.3.1 + Hero V6 + service-tiles + brand-mark Candidate B. Changing it now creates churn without a documented user-value reason.
- **Accent semantics.** `#ff7a00` (warm orange) reads as energy + execution + "operator-grade" and pairs cleanly with the deep navy without the yellow-flag amber feel. Operator buyer-class responds to confident accent, not safe-yellow.
- **Contrast.** `--navy` (`#0a2540`) on `--paper` (`#f7f8fa`) and inverse meet WCAG-AA at all in-use type sizes. `--accent` (`#ff7a00`) on white passes 4.5:1 for the bold weights we use it at (≥600).
- **Brand-mark coherence.** Candidate B brand-mark (commit `8bce6a2`) uses `#0a2540` rect + `#ff7a00` chevron stroke. Locking the palette to those exact hex values keeps the brand-mark consistent across nav, footer, favicon, og-image.

## Consequences

**Positive:**
- One canonical palette source — the `:root` block in `index.html` AND this ADR. Stale planning docs are explicitly superseded.
- Brand-mark, hero gradient, CTA, hover states, and theme-color all reference the same token family.
- Any future design refactor has a clear "where the tokens live" answer.

**Negative:**
- Stale planning documents still in the brain-repo may continue to reference the deprecated values. Future authors must check this ADR before designing.

**Mitigations:**
- Add a `<!-- ADR-002 -->` comment marker above the `:root` block in `index.html` so anyone editing CSS sees the pointer immediately. (Implementation note: optional · current ADR pointer in this file is sufficient.)
- All future "introduce a new color" requests trigger a new ADR before commit.

## Triggers for re-evaluation

- Brand-system refresh requested by Ryan or Darsan as operators.
- New line-of-business needs a distinct accent and a designer recommends a 2nd-accent token.
- Accessibility audit identifies a contrast failure that requires a palette shift.

## Related decisions

- **ADR-001:** Single-page IA.
- **ADR-003:** Typography pairing lock.
- **STORY-AURORA-023:** Brand-mark Candidate B (uses `--navy` + `--accent`).
