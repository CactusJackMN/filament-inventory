# Filament Inventory

**Repo:** https://github.com/CactusJackMN/filament-inventory

**Live app (GitHub Pages, once enabled):** https://cactusjackmn.github.io/filament-inventory/

A Progressive Web App for inventorying 3D printer filaments (material, brand, color, and how many spools you have). Built for Louis’s iPad (iPadOS 15.8 / Safari 15). It works offline and stores data only on the device.

## Install on the iPad

1. Open the live page in **Safari** (not Chrome).
2. Tap the **Share** button, then **Add to Home Screen**.
3. Open it from the Home Screen. It runs full-screen, like a regular app.

The first visit must be over HTTPS (GitHub Pages provides that). After it is on the Home Screen, it works offline.

## Daily use

- **List** and **Grid** views, filters by material, search, and **Sort: most on hand** (for “color doesn’t matter, use what I have plenty of”).
- On each card: **+ / −** sealed spools, **Used a spool**, **Edit**.
- **+ Add** for a single filament. **Add spools** is rapid entry: pick a material (it stays), then for each spool take a photo, tap the filament, enter grams (or 250 / 500 / 750 / 1000), **Save & next**. Undo is on the session list.
- **Camera** (on Add, Edit, and Add spools): take or pick a photo, tap or drag a reticle, and the app averages a 15×15 pixel patch while dropping the brightest 10% (glare). An optional **white balance** step lets you tap a white or gray card first. Photos are downscaled to 1,200 px before sampling so older iPads stay stable. The hex is always editable; a nearby color name is suggested.
- **Low stock** uses a configurable threshold (Settings). Entries with unknown counts are not treated as zero and are not flagged low.
- Light and dark appearance (system, or force one in Settings).

## CSV backup (please use this)

Counts live only on the iPad. iPadOS may clear Home Screen app storage if the app sits unused for a long time, so save a CSV to OneDrive now and then (CSV → **Save CSV** uses the iOS share sheet; otherwise Download or Copy CSV). That file can also be used for print planning.

### Stable CSV header

```
id,material,brand,product_line,color_name,color_hex,finish,diameter_mm,spools_sealed,spools_opened,grams_remaining,spool_capacity_g,location,date_opened,date_dried,notes,presets_k1c,presets_k2,low_stock_threshold,seeded
```

- `spools_sealed`, `spools_opened`, and `grams_remaining`: **blank means unknown, not zero**.
- `presets_k1c` / `presets_k2`: `yes` if that printer’s presets suit the filament.
- `finish`: `basic`, `matte`, or `silk`.
- `diameter_mm` defaults to `1.75`; `spool_capacity_g` defaults to `1000`.
- Dates are `YYYY-MM-DD`.
- `seeded` is `yes` for entries that came from print history.
- Import matches rows by `id` (updates existing, adds new).

## Seed list

The starting list is every distinct material + color + brand found in print records (print log, queue, K2 CFS slot snapshots). Quantities are left blank with the note “from print history, please count.” No colors or brands were invented.

## Privacy and hosting

No login, no tracking, no external CDNs. Your counts never go to GitHub. The public repo only has the app and the seed list (materials and colors already in the print records).

## Limitations (iPadOS 15)

- This app is written for Safari 15: no `:has()`, container queries, CSS nesting, `dvh`/`svh`, `color-mix()`, `oklch`, `structuredClone`, `Array.at` / `findLast`, top-level await, `<dialog>`, or the Popover API.
- `navigator.share` with a file opens the iOS share sheet on iPadOS 15. If share is unavailable, the app falls back to a download link and a Copy CSV box.
- Safari 15 auto-orients JPEGs when they are drawn from an `<img>` onto a canvas; sampling uses that path.
- `input type=color` is present, with a hex text field as the fallback.
- Service worker caching is simple (app shell only). Data is in `localStorage`, with a JSON backup key.

## Local files

- `index.html` — the whole app (CSS and JS inline)
- `manifest.json`, `sw.js`
- `icons/icon-192.png`, `icons/icon-512.png`, `icons/apple-touch-icon.png` (iOS 15 uses the apple-touch-icon, not the manifest icons, for the Home Screen)
- `icons/favicon.png`
