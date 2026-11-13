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

- **List**, **Grid**, and **Inventory** views. **Inventory** lists every spool (color, material, brand, weight and percent, date weighed, sealed/opened counts, barcode, location), with totals at the top. It sorts by recently added first by default, or by material, color, or weight, and filters by material.
- A green banner confirms every save, for example `Saved: PETG · Generic · White · 642 g`. After you add a spool, Inventory opens with that row highlighted.
- **List** and **Grid** still support filters by material, search, and **Sort: most on hand** (for “color doesn’t matter, use what I have plenty of”).
- On each card: **+ / −** sealed spools, **Used a spool**, **Weigh**, **Edit**. Tap **Weigh** and type the current net grams from the scale (tare an empty spool first), then Enter or **Save**. Cards show that weight as a share of the spool capacity, for example `642 g (64%)`.
- **+ Add** for a single filament. **Add spools** is rapid entry: pick a material (it stays), then for each spool take a photo, tap the filament, type the exact current grams (or use the 250 / 500 / 750 / 1000 shortcuts), **Save & next**. A running “N spools added this session” count and a session list (swatch, material, brand, color, weight, time, Undo / Edit) stay visible below the form.
- **Scan** (on the main list, and on Add / Edit next to the barcode field): opens the rear camera and continuously reads 1D and 2D codes (QR, Data Matrix, Aztec, PDF417, UPC, EAN, Code 128, Code 39, and similar). A short beep plays when a code is read (and vibration, if the iPad supports it). If the code already belongs to a spool, that entry opens with the current-weight field focused so you can type the scale reading. Otherwise a new entry is started with the barcode filled in, using the last material / brand from rapid entry, and any brand, material, color, or hex the code itself contains. If the camera is denied, type the code or use a photo.
- **Camera** (on Add, Edit, and Add spools): take or pick a photo, tap or drag a reticle, and the app averages a 15×15 pixel patch while dropping the brightest 10% (glare). An optional **white balance** step lets you tap a white or gray card first. Photos are downscaled to 1,200 px before sampling so older iPads stay stable. The hex is always editable; a nearby color name is suggested.
- **Low stock** uses a configurable threshold (Settings). Entries with unknown counts are not treated as zero and are not flagged low.
- Light and dark appearance (system, or force one in Settings).

## CSV backup (please use this)

Counts live only on the iPad. iPadOS may clear Home Screen app storage if the app sits unused for a long time, so save a CSV to OneDrive now and then (CSV → **Save CSV** uses the iOS share sheet; otherwise Download or Copy CSV). That file can also be used for print planning.

### Stable CSV header

```
id,material,brand,product_line,color_name,color_hex,finish,diameter_mm,spools_sealed,spools_opened,grams_remaining,spool_capacity_g,location,date_opened,date_dried,notes,presets_k1c,presets_k2,low_stock_threshold,seeded,barcode,date_weighed,date_added
```

- `spools_sealed`, `spools_opened`, and `grams_remaining`: **blank means unknown, not zero**.
- `grams_remaining` is the net filament weight in grams (weigh the spool after taring an empty spool).
- `presets_k1c` / `presets_k2`: `yes` if that printer’s presets suit the filament.
- `finish`: `basic`, `matte`, or `silk`.
- `diameter_mm` defaults to `1.75`; `spool_capacity_g` defaults to `1000`.
- Dates are `YYYY-MM-DD`.
- `seeded` is `yes` for entries that came from print history.
- `barcode` is the raw text of a scanned or typed code. It was appended so older CSVs without this column still import.
- `date_weighed` is the `YYYY-MM-DD` when the current net weight was entered.
- `date_added` is when the entry was created (`YYYY-MM-DDTHH:MM` local). It is appended at the end so older CSVs without this column still import. Blank on older or seeded rows.
- Import matches rows by `id` (updates existing, adds new).

## Seed list

The starting list is every distinct material + color + brand found in print records (print log, queue, K2 CFS slot snapshots). Quantities are left blank with the note “from print history, please count.” No colors or brands were invented.


## What a scan fills in

The raw code is always stored on the entry. The app also fills fields when the payload clearly contains them — it never guesses missing brand, material, or color.

- **OpenSpool JSON** (`"protocol":"openspool"`) and the compact `OS1.0|TYPE|COLOR_HEX|BRAND|MIN_TEMP|MAX_TEMP` QR form.
- **OpenPrintTag-style JSON** and similar objects with `brand_name`, `material_type`, `material_name`, `primary_color`, or a nested `data.material` object (a JSON QR stand-in; official OpenPrintTag NFC tags are CBOR and are not barcodes).
- Generic JSON, `key=value` text, or a URL with query parameters for brand, material, color, hex, diameter, or weight.
- Retail **UPC / EAN / Code 128** values are stored as-is. There is no public barcode layout for Creality, Sunlu, or Elegoo spools that encodes color or material, and Creality CFS RFID is not a barcode.

## Privacy and hosting

No login, no tracking, no external CDNs. Your counts never go to GitHub. The public repo only has the app and the seed list (materials and colors already in the print records).

## Limitations (iPadOS 15)

- This app is written for Safari 15: no `:has()`, container queries, CSS nesting, `dvh`/`svh`, `color-mix()`, `oklch`, `structuredClone`, `Array.at` / `findLast`, top-level await, `<dialog>`, or the Popover API.
- `navigator.share` with a file opens the iOS share sheet on iPadOS 15. If share is unavailable, the app falls back to a download link and a Copy CSV box.
- Safari 15 auto-orients JPEGs when they are drawn from an `<img>` onto a canvas; sampling uses that path.
- `input type=color` is present, with a hex text field as the fallback.
- There is no `BarcodeDetector` API on iPadOS 15, so scanning uses a vendored copy of `@zxing/library` 0.23.0 (no CDN). Live camera access needs a tap, HTTPS, and the `playsinline` video attribute. `navigator.vibrate` is often unavailable on iPad; the success beep uses the Web Audio API. Creality CFS / Bambu factory spool chips are NFC/RFID, not barcodes, and are out of scope.
- Service worker caching is simple (app shell only, including the decoder). Data is in `localStorage`, with a JSON backup key.

## Local files

- `index.html` — the whole app (CSS and JS inline)
- `vendor/zxing.min.js` — offline barcode decoder (`@zxing/library` 0.23.0)
- `manifest.json`, `sw.js`
- `icons/icon-192.png`, `icons/icon-512.png`, `icons/apple-touch-icon.png` (iOS 15 uses the apple-touch-icon, not the manifest icons, for the Home Screen)
- `icons/favicon.png`
