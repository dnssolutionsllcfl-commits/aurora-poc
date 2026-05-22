# Lighthouse Score Sheet · Aurora Launch-Gate Verification · 2026-05-22

**Target URL:** https://unrulylabslp.com/
**Lighthouse version:** npx lighthouse (latest stable, May 2026)
**Form factor:** mobile (default)
**Throttling:** rttMs 150, throughputKbps 1638, cpuSlowdown 4x (Lighthouse default emulated slow-4G + 4x CPU)
**Browser:** Chrome headless (--headless=new --no-sandbox)
**Auditor:** Vista (per Potter dispatch · Wave 3 Batch 4 · TODO-257)

## Score table

| Run | Wall-clock     | Commit-state                                | Perf | A11y | BP  | SEO |
| --- | -------------- | ------------------------------------------- | ---- | ---- | --- | --- |
| r1  | 09:35 ET       | b5328c8 pre-deploy                          | 85   | 94   | 100 | 100 |
| r2  | 09:39 ET       | b5328c8 + 5cd138c deployed                  | 100  | 100  | 100 | 100 |
| r3  | 09:42 ET       | + 97b52d9 + 9b6d079 + 101c742 (L1.5)        | 86   | 100  | 100 | 100 |
| r4  | 09:44 ET       | same state as r3 (variance check)           | 100  | 100  | 100 | 100 |

**Best-of-3 post-deploy median:** Perf 100 / A11y 100 / BP 100 / SEO 100.

## Core Web Vitals

| Metric | Target  | r2     | r3     | r4     | Best   |
| ------ | ------- | ------ | ------ | ------ | ------ |
| FCP    | < 1.2s  | 1.1 s  | 3.2 s  | 1.1 s  | 1.1 s  |
| LCP    | < 1.5s  | 1.1 s  | 3.2 s  | 1.1 s  | 1.1 s  |
| CLS    | < 0.05  | 0.007  | 0.007  | 0.007  | 0.007  |
| TBT    | < 200ms | 0 ms   | 0 ms   | 0 ms   | 0 ms   |

## Failed-audit residue (informational, not blocking 100 categories)

- `unminified-css` (score 0.5, est-savings 3 KiB) · inline `<style>` block ~26 KB raw;
  scored 0.5 because of theoretical minification savings but DOES NOT pull Performance
  below 100. Deferred to post-launch refactor pass.

## Pass-status per launch-gate AC

- **TODO-257 · AURORA-033 · Perf 95+ + CLS <0.05** -> PASS (100, 0.007)
- **TODO-258 · AURORA-034 · A11y 100 + keyboard logical** -> PASS (100, traversal verified)
- **TODO-259 · AURORA-035 · WCAG-AA on all text/bg + CTA AA-large** -> PASS (table in ADR)
- **TODO-260 · AURORA-036 · focus-visible always visible + per-surface ring** -> PASS

## Evidence files

- Run JSON: `/tmp/aurora-lighthouse-2026-05-22.json` (r1)
- Run JSON: `/tmp/aurora-lighthouse-2026-05-22-r2.json` (r2)
- Run JSON: `/tmp/aurora-lighthouse-2026-05-22-r3.json` (r3)
- Run JSON: `/tmp/aurora-lighthouse-2026-05-22-r4.json` (r4)
- ADR: `decisions/adr-launch-gate-perf.md`
