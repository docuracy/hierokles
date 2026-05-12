# Hierokles Synekdemos — Interactive Map

A static single-page web app that maps the dioceses, provinces, and cities recorded in the 6th-century *Synekdemos* of Hierokles, built with [MapLibre GL JS](https://maplibre.org/).

## Data sources

The three datasets come from the [Ancient World Mapping Center (AWMC), UNC Chapel Hill](https://awmc.unc.edu/). The legacy Hierokles application that hosted them (`awmc.unc.edu/awmc/applications/bam/modules/hierokles/`) is now offline, so the files are retrieved from the Wayback Machine:

- `data/hierokles_dioceses.geojson` — 6 dioceses (MultiPolygon), from the [2022-10-31 snapshot](https://web.archive.org/web/20221031132640/http://awmc.unc.edu/awmc/applications/bam/modules/hierokles/hierokles_dioceses.geojson)
- `data/hierokles_provinces.geojson` — 64 provinces (MultiPolygon), from the [2022-10-31 snapshot](https://web.archive.org/web/20221031132640/http://awmc.unc.edu/awmc/applications/bam/modules/hierokles/hierokles_provinces.geojson)
- `data/hierokles_places.geojson` — 586 cities, converted from AWMC's nodes/edges JSON (the [2022-10-22 snapshot](https://web.archive.org/web/20221022161051/http://awmc.unc.edu/awmc/applications/bam/modules/hierokles/hierokles_places.json)) to GeoJSON Points

The original `hierokles_places.json` is kept alongside in `data/` for reference.

AWMC also publishes their current canonical version of this material as a hosted [ArcGIS Experience](https://experience.arcgis.com/experience/46837fd10a9844ecac09cd5aff6a7541/) (a Hierokles feature service with 918 places). Their general data repository at [github.com/AWMC/geodata](https://github.com/AWMC/geodata) is the right place to look for other AWMC layers; the Hierokles dataset is not currently mirrored there (its `political_shading/provinces post_diocletian` shapefile is the closest match but is unlabelled).

## Running locally

The page is fully static. From the repository root:

```
python3 -m http.server 8000
```

then open <http://localhost:8000/>.

## Deploying to GitHub Pages

1. Push the repository to GitHub.
2. Settings → Pages → Source: *Deploy from a branch* → `main` / `/` (root).
3. The `.nojekyll` file at the root keeps GitHub Pages from interfering with file paths.

## Layout

- `index.html` — the SPA (basemap, layers, search, popups, styling)
- `data/` — GeoJSON datasets
- `.nojekyll` — disables Jekyll on GitHub Pages

Basemap raster tiles come from the [Consortium of Ancient World Mappers](https://cawm.lib.uiowa.edu/) — a shaded-relief basemap of the ancient Mediterranean, the same one used by the AWMC's own ArcGIS Experience for this dataset. No API key is required.

The province and diocese polygons in `data/` have been simplified with Shapely (`simplify(0.01, preserve_topology=True)`) to bring the first-load payload from ~52 MB down to ~1 MB. The displayed boundaries are still well within the precision the AWMC data warrants at this scale; if you want the originals, re-fetch them from the URLs in `data/`.
