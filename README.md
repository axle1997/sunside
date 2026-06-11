# Sunside ☀️

**Find a terrace in the sun — right now, anywhere.**

Sunside shows you every bar, café, pub and restaurant with outdoor seating near you, and calculates which ones are actually in the sun at this moment by ray-tracing the sun's position against real building geometry. Scrub the time slider to watch shadows sweep across the streets and plan where to be at golden hour.

**Try it live: https://axle1997.github.io/sunside/**

<!-- Screenshot: map view with sunny terraces highlighted -->
![Sunside showing sunny terraces on a map](screenshots/map.png)

<!-- Screenshot: time slider at golden hour with long shadows -->
![Scrubbing the time slider to golden hour](screenshots/golden-hour.png)

## Why

"Let's sit outside" is easy. "Let's sit outside *in the sun*" means walking past four shaded terraces hoping the fifth one works. In dense cities, whether a terrace gets sun depends entirely on the buildings around it and the time of day — information that exists, but not in any app. So this is that app.

## How it works

- **Venues** come live from [OpenStreetMap](https://www.openstreetmap.org) via the Overpass API: anything tagged with `outdoor_seating` (bars, pubs, cafés, restaurants, beer gardens) within your chosen radius.
- **Sun position** (altitude and azimuth) is computed astronomically for your exact location and the selected time — no API needed.
- **Shadows** are real: every building footprint in the area is fetched with its tagged height (or floor count × 3 m, defaulting to ~12 m when untagged). For each terrace, Sunside casts rays toward the sun and checks whether any building is tall enough to block it, sampling three points per terrace to distinguish full sun, partial sun, and shade.
- **The shadow overlay** you see on the map is the same geometry, rendered in a single canvas pass so it reads like an aerial photo rather than a pile of polygons.

Everything runs client-side in one HTML file. There is no backend, no tracking, no account, no build step.

## Using it

1. Open the page and allow location access (or pan the map anywhere and tap **Search this area**).
2. Amber dots are in the sun right now. Faint gray specks are in shade. The list panel sorts the sunny ones to the top.
3. Drag the time slider to plan ahead — sunrise and sunset are marked on the track. Tap **Now** to return to live mode.
4. Tap any terrace for details and walking directions.

## Run or host your own

It's a single file. Clone the repo and open `index.html` in a browser, or fork this repo and enable GitHub Pages (Settings → Pages → deploy from main). No dependencies to install — Leaflet and fonts load from public CDNs.

## Honest limitations

- **Coverage depends on OSM tagging.** Cities with active mapping communities (Paris is excellent) show hundreds of terraces; elsewhere it may be sparse. The fix is upstream: [tag terraces in OpenStreetMap](https://wiki.openstreetmap.org/wiki/Key:outdoor_seating) and every app using the data improves.
- **Untagged building heights are estimated** (~12 m), and trees, awnings, parasols and balconies aren't modelled. Treat "partial sun" as a maybe.
- **Terrace positions are approximate** — OSM places the venue, not the exact tables.
- The public Overpass API is shared infrastructure and occasionally rate-limits; if a search fails, wait a minute and retry.

## Data & credits

Venue and building data © OpenStreetMap contributors, queried via [Overpass API](https://overpass-api.de). Map tiles by [CARTO](https://carto.com), rendered with [Leaflet](https://leafletjs.com). Sun position math follows the standard astronomical approximations popularised by [SunCalc](https://github.com/mourner/suncalc).

## License

MIT — do whatever you like, attribution appreciated. Note that OpenStreetMap data carries its own [ODbL terms](https://www.openstreetmap.org/copyright).
