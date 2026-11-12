# Vendored decoder

`zxing.min.js` is the UMD build of [@zxing/library](https://github.com/zxing-js/library) **0.23.0**, Apache License 2.0 (see `zxing.LICENSE`).

It is copied from the npm package file `umd/index.min.js` so the app can decode barcodes offline on iPadOS 15 Safari, which has no `BarcodeDetector`. Do not load this from a CDN at runtime. The service worker caches this file with the app shell.

To refresh the vendor copy:

```
curl -L -o vendor/zxing.min.js https://cdn.jsdelivr.net/npm/@zxing/library@0.23.0/umd/index.min.js
```
