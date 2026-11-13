# Filament Inventory CSV format

Stable header (do not rename columns; new columns may be added at the end in a future version):

```
id,material,brand,product_line,color_name,color_hex,finish,diameter_mm,spools_sealed,spools_opened,grams_remaining,spool_capacity_g,location,date_opened,date_dried,notes,presets_k1c,presets_k2,low_stock_threshold,seeded,barcode,date_weighed,date_added
```

| Column | Meaning |
|--------|---------|
| id | Stable row id. Import updates a row with the same id. |
| material | PLA, PLA+, PLA-Silk, PETG, TPU, ABS, ASA, PET-CF, Other, or a custom value. |
| brand | e.g. Generic, Creality, Prusa Polymers. |
| product_line | e.g. CR-PLA, Hyper PLA, Generic PETG. |
| color_name | Display name (White, Gold, Teal, …). |
| color_hex | `#RRGGBB` or blank if unknown (e.g. Clear PETG). |
| finish | `basic`, `matte`, or `silk`. |
| diameter_mm | Default `1.75`. |
| spools_sealed | Unopened count. **Blank means unknown, not zero.** |
| spools_opened | Opened count. **Blank means unknown, not zero.** |
| grams_remaining | Net filament weight in grams, after taring an empty spool on the scale. **Blank means unknown, not zero.** |
| spool_capacity_g | Default `1000`. |
| location | Storage location. |
| date_opened | `YYYY-MM-DD`. |
| date_dried | `YYYY-MM-DD`. |
| notes | Free text. Seeded rows start with `from print history, please count`. |
| presets_k1c | `yes` if K1C presets suit this filament. |
| presets_k2 | `yes` if K2 Pro presets suit this filament. |
| low_stock_threshold | Per-row override (total spools). Blank uses the app default. |
| seeded | `yes` for rows that came from print history. |
| barcode | The raw text of a scanned or typed 1D/2D barcode (QR, Data Matrix, UPC, EAN, Code 128, …). Blank on older rows. |
| date_weighed | `YYYY-MM-DD` when the current net weight was entered. Blank if the weight is unknown or was never weighed. |
| date_added | When the entry was created (`YYYY-MM-DDTHH:MM` local). Blank on older or seeded rows. Appended at the end so older CSVs still import. |

The file is UTF-8, comma-separated, RFC 4180-style quoting, CRLF line endings. A leading UTF-8 BOM is accepted on import. Older files without the trailing `barcode`, `date_weighed`, or `date_added` columns still import; missing values are treated as blank.
