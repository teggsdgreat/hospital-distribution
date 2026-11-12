# Hospital Hub-and-Spoke Network Map

An interactive map of hospitals showing each **hub** and lines to all of its **spoke** sites, with the straight-line distance on every line. It helps you find hospitals quickly, see how far each spoke is from its hub, and spot spokes that would fit better with another hub.

Everything runs in the visitor's browser (Leaflet + Turf.js), so there is no server or database to maintain.

**Live map:** https://teggsdgreat.github.io/hospital-distribution/

Link straight to one hub's network by adding `#hub=<code>` to the link, e.g. `https://teggsdgreat.github.io/hospital-distribution/#hub=AN3`.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app: map, tools, styling and logic |
| `hospitals.csv` | The hospital list the map loads on start |
| `README.md` | This guide |

## How hubs and spokes are read from the CSV

- `Hub` column: the hub code a hospital belongs to (e.g. `AN3`).
- `Spoke` column: the hospital's own code (e.g. `AN3-12`).
- The **hub hospital** of each group is the one whose Spoke code is the hub code + `-1` (e.g. `AN3-1`, St Charles Borromeo Hospital). If you'd rather mark hubs explicitly, add a `Role` column and write `Hub` for the hub hospitals.
- Every other hospital with that Hub code is a spoke and gets a line to the hub.

## What's on the page

- **Hubs tab** (opens first)
  - Totals: hubs, spokes, average and longest hub–spoke distance, links over a chosen distance, and spokes that are closer to a different hub.
  - Each hub with its number of spokes, average and farthest distance. Click a hub to zoom to it, fade the others, label every line with its km, and list its spokes from nearest to farthest.
  - **Assigned hub / Nearest hub** switch: draw lines to the hub in the CSV, or to whichever hub is closest. Comparing the two shows a distance-based regrouping.
  - **Spokes closer to another hub**: candidates for reassignment, with a download.
  - Downloads: a hub–spoke distance table (CSV) for all hospitals shown.
  - Links longer than the chosen limit (default 30 km) are drawn dashed with an orange label.
- **Find**: search by name, town, LGA or state; colour markers by Hub, State, Denomination, etc.
- **Nearest**: nearest hospitals to your location, a point, or any hospital.
- **Distance**: measure between any two hospitals; export pairs, nearest neighbours or a full distance matrix.
- **Groups**: automatic proximity groups (max distance, neighbour chain, or a fixed number of groups) — useful for testing alternative clusters.
- **Data**: coordinate checks and loading another file. Filters (State, Hub, Region, LGA, Denomination, Location precision) are at the top.

## Updating the hospital list

Replace `hospitals.csv` in the repo (**Add file → Upload files**, same name) and commit. The site updates within a minute or two. Keep the column names `Latitude`, `Longitude`, `Hospital Name`, `Hub` and `Spoke`.

Visitors can also load their own CSV, Excel or GeoJSON file from the **Data** tab (or by dragging it onto the map). That only affects their own browser.

## Road distance and driving time (optional)

All distances are straight-line ("as the crow flies"); road distance is longer. For road distance:
1. Create a free account at https://openrouteservice.org/dev/#/signup and copy your API key.
2. Paste it in the **Data** tab (saved only in that visitor's browser). A **Road distance & drive time** button then appears for each hub in the Hubs tab, or
3. set it for everyone by editing `index.html`: find `orsKey: ''` near the top and put the key between the quotes. Anyone can read a key placed in the page, and the free plan's daily limits are shared by all visitors.

## Customising

All settings are in the `CFG` block at the top of the `<script>` in `index.html`: title, data file name, filter columns, default colour field, the hub/spoke column names, the hub code suffix (`-1`) and the long-link distance.

## Background maps

The default background is OpenStreetMap, with Esri Streets, Esri Light grey and Esri Satellite also on offer. OpenStreetMap's servers refuse requests from a page opened straight from your computer (`file://`), so the page then starts on Esri Streets instead.

## Privacy

A free GitHub Pages site is public: anyone with the link can see the hospital list. If the list must stay private, use a private host instead (for example Netlify with password protection, or your organisation's server).

## Data notes

According to its `Location precision` column, many rows are "Approximate / probable", and a few hospitals share identical coordinates (shown as 0 km apart). Treat distances to those as rough, and verify driving routes before operational use.
