# Fonts

Install precompiled `.cpfont` font families from a catalog into
`/.fonts/<Familia>/` on the SD card, preview each family, choose which point
sizes to install (6–18), and pick the active family. Built to consume a
[cp-fonts](https://github.com/josmerod/cp-fonts) catalog — CrossGlyph builds of
curated [WP-Fonts](https://github.com/Chairzard/WP-Fonts) families.

The previews are page renders from the reader's own renderer (geometry `x4`),
so what you see is what the device draws.

## Install the plugin

Copy this folder to `/.crosspoint/plugins/fonts/` on the SD card, or install it
from your Plugin Store if this repo is your store's catalog. Then edit
`CATALOG_URL` at the top of `plugin.js` if your cp-fonts fork lives somewhere
else.

## Use

- **From the web UI:** open Settings → Fonts. Filter the catalog, look at the
  previews, uncheck any point sizes you don't want, and press *Install* — the
  device streams the files straight to `/.fonts/<Familia>/` (they never pass
  through your browser). *Remove* deletes a whole family.
- **On the reader:** Settings → Font lists every installed family with the
  sizes it carries — switching there applies immediately, no reboot needed.
- **Active font:** the *Set active* button writes `sdFontFamilyName` into
  `/.crosspoint/settings.json` (applies after a restart). The on-device picker
  is the live path.

The reader holds at most 128 families; the plugin warns when you're there.

## How it works

The catalog (`catalog/fonts.json` + preview PNGs + license files) is served
from raw.githubusercontent.com by the cp-fonts repo, which rebuilds it on a
schedule. The `.cpfont` binaries live as rolling-release assets; because
`/api/fetch` does not follow redirects, the plugin resolves the release
redirect via `relay` HEAD hops before streaming each file to SD (the same
pattern as the *dictionaries* plugin). Installed state comes from the
device's own `GET /api/fonts`.
