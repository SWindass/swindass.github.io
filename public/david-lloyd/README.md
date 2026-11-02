# David Lloyd / parkrun / Premier Inn Finder

Interactive UK map that overlays three datasets on one view, letting you plan a parkrun-plus-swim-plus-hotel weekend in a single glance.

Live at **https://www.ntroniq.com/david-lloyd/** (served as `public/david-lloyd/index.html`).

## What's on the map

| Layer        | Count | Icon      |
| ------------ | ----- | --------- |
| David Lloyd  | 117   | Red dot   |
| parkruns     | 903   | Green dot |
| Premier Inns | 844   | Purple dot |

All three layers can be toggled independently from the top-right legend.

## Features

- **Search** — type a town, postcode, or place name and the map flies to the geocoded bbox (Nominatim / OpenStreetMap).
- **"Mine" selections** — tap any pin, click the star in the popup, and that venue is remembered in `localStorage`. When at least one venue is starred in a layer, that layer is filtered to only the starred ones.
- **Labels at close zoom** — zooming past level 12 shows the venue name tagged next to each pin, prefixed with `Premier Inn`, `Parkrun`, or `David Lloyd` for disambiguation.
- **Parkrun course polylines** — zooming past level 13 overlays the actual course route (where known) decoded from the parkrun's Google My Maps page.
- **Direct booking** — Premier Inn popups link to the exact hotel's booking page (matched against the official sitemap, so no 500s).
- **Parkrun pages** — parkrun popups link to the course's `parkrun.org.uk/<slug>/` page.
- **Dark / light theme** — follows the OS `prefers-color-scheme`.
- **Mobile layout** — compressed top bar with a drawer; pin hit radii are enlarged on touch devices.
- **Add to Home Screen** — single standalone HTML file, works offline after first load (bar the tiles).

## Data provenance

- **David Lloyd clubs** — scraped from the David Lloyd Leisure club finder.
- **parkruns** — scraped from `parkrun.org.uk/countries/UK/events/`.
- **parkrun course routes** — each parkrun's Google My Maps `mid` was looked up on its event page, then the KML (new-format mids) or embed HTML (old-format mids with dots) parsed into polylines, Haversine-thinned to 10 m and delta-encoded.
- **Premier Inn hotels** — parsed from the public `sitemap-hotels-en.xml`, giving both coordinates and the per-hotel booking URL.

## How it's built

- **Single file.** The entire app — code, styles, 117 + 903 + 844 markers, all route polylines, all booking URLs — is one ~1 MB HTML file loaded directly. No build step, no bundler, no backend.
- **Leaflet 1.9.4** with Esri tile layers (World Street Map in light mode, Canvas in dark mode).
- **No tracking**, no analytics, no cookies, no external calls beyond tile loads and (on search) a Nominatim lookup.

## Deploy

The site is deployed by the GitHub Actions workflow in `.github/workflows/deploy.yml` on every push to `main`. GitHub Pages serves it under the custom domain in `CNAME` (`www.ntroniq.com`), so updates to this file go live within a minute or two of a push.

## Updating the standalone HTML

The source of truth lives in `C:\Users\HP ZBook\Downloads\david-lloyd-map-standalone.html` on the author's workstation. To deploy an update, copy that file over `public/david-lloyd/index.html` and push.

Helper scripts (parkrun GPX extraction, Premier Inn sitemap scrape, route injection) live in the session scratchpad and are not committed, as the generated data is embedded in the HTML.
