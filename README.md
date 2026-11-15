# HELIX — prop firm & challenge database

A static, bilingual (KO/EN) comparison site for prop-firm challenge programmes and brokerage accounts.
Every number on the page is either recorded with a source or shown as *unrecorded* — nothing is estimated.

- **Live:** https://pataboo.github.io/propfirm-helix/
- **Current snapshot:** 1,971 products · 71 brands · grade A 1,194 (60.6%) / B 208 / C 569
- **Files in this repo:** `index.html` (generated page) and `data.js` (generated dataset). Both are build output — edit the source, not these files.

## What the site does

| Route | Page |
|---|---|
| `#home` | Entry overview with live counts |
| `#challenges` | Main table: filters, presets, sorting, virtual scrolling, CSV export, favourites, column settings (URL state is shareable) |
| `#discover` | Rankings: rating, value, split, shortest payout cycle, payout cap, free and pay-later programmes |
| `#firms` / `#firm/<brand>` | Brand index and brand profile split into **rules · EA/platform · payouts** |
| `#challenge/<rowId>` | Single product: KPIs, fee breakdown, rule flags, payout terms, source quote |
| `#compare` / `#compare/<id>,<id>` | Side-by-side comparison of up to 5 products, with "differences only" |
| `#payouts`, `#spreads`, `#offers`, `#tools`, `#rules` | Payout terms, spread tables, promo codes, trading tools, per-firm rule changes |
| `#method` | Methodology and field dictionary: what each field means, how many rows have it, how grades work |

## Data policy

1. **A product row is a product.** One row = one challenge programme at one account size; brands are aggregated from rows.
2. **No invented values.** If a firm does not publish a number it stays empty (`—`), and *"no limit"* is stored as its own value so it is never confused with *unrecorded*.
3. **Grades are about evidence, not quality.**
   - **A** — read directly from the firm's own page (quote and URL stored on the row).
   - **B** — secondary/indirect evidence (archived page, reseller, structured third-party data).
   - **C** — lead only, not confirmed.
4. **Dates are fixed per row** (`asOf`). Rows older than 30 days (14 for grade C) carry a ⚠ badge with the age in days.
5. **Source quotes are kept.** The stored sentence is what the value was read from, so a later reader can re-check it.
6. **Out of scope:** anything requiring an account, a purchase or a contract with a firm. This repo contains only public page reads.

Current source mix: `official` 1,279 · `pfm-local` 532 · `store` 88 · `pfm` 37 · `wayback` 35.

## Building

The build script lives next to the data workbench, not in this repo:

```
/home/gooswan/IO_Linux/2_Workbench/Iris/Helix_Deploy_20261114/build_site3.py
```

It takes a base HTML from a historical commit of this repo, injects the dataset and writes:

```
repo/index.html   ← page (single file, inline CSS + JS)
repo/data.js      ← window.HELIX_DB (rows, firms, payouts, rule changes, EA/platform data)
```

Inputs: `data/*.json` (PFM exports, overrides, verification batches, `field_overrides_*.json`,
`ea_platforms.json`, `payout_methods.json`) and the PFM rule text. `build_site.py` and
`build_site2.py` in the same folder are superseded by `build_site3.py`.

Run:

```bash
cd /home/gooswan/IO_Linux/2_Workbench/Iris/Helix_Deploy_20261114
python3 build_site3.py
```

## Verifying before a deploy

```bash
# 1. build + JS syntax
python3 build_site3.py
python3 - <<'PY'
import re; h=open('repo/index.html',encoding='utf-8').read()
open('/tmp/inline.js','w').write(re.findall(r'<script>(.*?)</script>',h,re.S)[-1])
PY
node --check /tmp/inline.js
```

2. Open the built page and walk every route (`#home`, `#challenges`, `#compare`, `#discover`,
   `#docs`, `#method`, `#firms`, `#firm/<brand>`, `#challenge/<rowId>`, `#offers`, `#payouts`,
   `#spreads`, `#tools`, `#rules`) with the console open — zero errors expected.
3. Check the table renders a scroll window (DOM rows stay under ~30 at 1,971 rows), sorting and
   filters still apply, and keyboard operation (Tab → sort header, Enter → sort) still works.

## Deploying

```bash
cd repo
git add -A && git commit -m "<what changed and why>"
git push origin main
gh api -X POST repos/pataboo/propfirm-helix/pages/builds
# then reload https://pataboo.github.io/propfirm-helix/?v=$(date +%s)#challenges
```

GitHub Pages serves `main` directly; the `?v=` parameter only defeats browser cache.

## Limits

- Data is a **snapshot**, not a live feed. Prices and promos move; the per-row `asOf` is the truth.
- Mobile layouts are checked at 390 px; a horizontal overflow remains on very narrow desktop
  windows (~560–780 px) because the table keeps its minimum column widths.
- 24.2% of rows still have no recorded daily-loss limit and 17.9% no max-loss limit; those are
  research gaps, not zeroes.
- Nothing here is financial advice, and the project is not affiliated with any firm listed.
