# Aurora POC v0.1 — Unruly Labs LP company website

Codename **Aurora**. Single-file, clickable, static-HTML preview of the Unruly Labs LP company website. Built by Potter subagent on 2026-05-18 against Ryan ts 1779224106 ("use your best judgement; just want to see the poc").

## How to view

```bash
open /Users/ddinaram/Potter/aurora-poc/index.html
```

That's it. No build step. Tailwind is pulled from CDN. Opens in any browser.

## What's in the POC

Single-page scroll, six sections:

1. **Hero** — bi-tonal headline ("We operate businesses we built. From AI infrastructure to games that pop."), forge subhead, CTA pair (See our work / Get an audit).
2. **Master-brand statement** — the holding-company narrative paragraph (the structural-claim anchor).
3. **Portfolio** — four cards: Agent-Fleet IP, 30-site enterprise monitoring (ECS anonymized), Forge website-audit (in pilot), clog-busters (vibrant lane). **destiny is omitted** per Atlas red-pen Fix 1.
4. **Services** — three productized lines (Agent-fleet operations · Vision-LLM monitoring · Website-audit / Forge).
5. **About** — joint principal narrative + Ryan bio + Darsan bio. `[OPERATOR-PENDING TITLE]` placeholders and `STALENESS FLAG` markers preserved as visible chips.
6. **Contact** — `mailto:ops@unrulylabslp.com` plus a placeholder card for the eventual Tally embed.

## Design choices

- **Color palette.** Deep navy (`#0a2540`) as the institutional base; warm orange (`#ff7a00`) as the vibrant-lane accent. Bi-tonal voice corner expressed visually: institutional cards use navy gradients; the clog-busters card uses the orange gradient.
- **Typography.** Inter via Tailwind defaults. Large display headlines with negative letter-spacing for institutional gravity; generous body line-height for readability.
- **Layout.** Single column, `max-w-5xl`, mobile-first. Sticky top nav with same-page anchors.
- **No images.** Card art uses colored gradient blocks with text labels (POC class).

## What's locked vs. placeholder

| Element | State |
|---|---|
| Hero headline + subhead | Locked from Phase 3 v1.1 (Candidate 1 hero) |
| Master-brand paragraph | Locked from Phase 3 v1.1 |
| Portfolio cards 1, 2, 3, 4 | Locked copy from Phase 3 v1.1 (destiny omitted per Atlas red-pen) |
| Services 01, 02, 03 | Locked from Phase 3 Part 1 #3 (top-3) |
| Ryan bio | Locked from Phase 4 v1.3 (resume-bounded) |
| Darsan bio | Locked from Phase 4 v1.3 (resume-bounded, STALENESS FLAG visible) |
| Title placeholders | `[OPERATOR-PENDING TITLE]` — operator picks from: Operating Principal / Managing Partner / Principal / Controller / Head of Product / custom |
| Controller-role | Tentatively Ryan-only (Potter MEMORY anchor) — governance clarification pending |
| Tally intake form | Placeholder card — replace at v0.2 |
| Imagery | Colored gradient placeholders — replace at v0.2 once Part 8 asset pipeline lands |
| Final tagline | Darsan-pick pending across 13 candidates in Phase 3 v1.1 Section F |

## Anti-patterns honored

- **No invented copy.** Every body paragraph traces to Phase 3 v1.1 or Phase 4 v1.3 verbatim.
- **No destiny card.** Atlas red-pen Fix 1 applied.
- **No outside-attorney references.** Forge conditions cascade applied.
- **No "Co-Founder" / "co-CEO" titles.** Ryan-locked operator-direct (LP ts 1779201364). Operator picks from the title-pair options list.
- **Operator-facing simplicity.** Single file. Single command to view.

## Next steps (operator-facing)

1. **Open it.** `open /Users/ddinaram/Potter/aurora-poc/index.html` — eyes-on the POC.
2. **Red-pen.** Anything that reads wrong — note it. Hero pick, tagline pick, color choice, copy edits.
3. **Decide v0.2 scope.** Options range from "ship this as the live site (Tally form + asset pipeline + domain wire-up)" to "rebuild in Webflow per Phase 5 build playbook." POC class to launch class is operator-gated.

## Stack

- Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com"></script>`)
- No build step, no Node, no dependencies
- Single `index.html` file, self-contained
- ~14 KB HTML + inline CSS

End of Aurora POC v0.1 operator note.
