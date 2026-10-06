Built for the **SESAC workshop at RisCon26, Karlstad University,10/2026**. 

# Flood Scenarios & Critical Service Accessibility in Karlstad

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/amaletaa/SESAC_RisCon26_Workshop/blob/main/RisCon26_SESAC_workshop.ipynb)

A hands-on geospatial workshop notebook that asks one practical question: **if Karlstad floods, who can still reach a hospital, a fire station or a school in time, and who can't?** It takes official flood-depth scenarios, lays them over the real road network, and measures how emergency accessibility shrinks as the water rises.

Built for the **SESAC workshop at RisCon26**. <!-- TODO: add venue / date, e.g. "Karlstad University, [month] 2026" -->

## The story

Karlstad sits on the delta where the Klarälven river meets Lake Vänern, which makes flooding a planning question here rather than a hypothetical one. Sweden's Civil Contingencies Agency (MSB) maps this risk as **flood-depth scenarios**: how deep the water would be, and where, for floods of different magnitudes.

Unlike an event-based analysis ("what happened in February 2020?"), this notebook works with **scenarios** ("what would happen if…?"). Participants compare three of them side by side:

| Scenario | File | Meaning |
|---|---|---|
| 50-year flood | `Karlstad_50_djup.tif` | A flood with a ~2% chance of occurring in any given year |
| 100-year flood | `Karlstad_100_djup.tif` | ~1% chance in any given year |
| 200-year flood | `Karlstad_200_djup.tif` | ~0.5% chance in any given year |

*(djup = depth, in metres)*

## What the notebook does

1. **Flood scenarios** — loads the three MSB depth rasters and shows how the flooded area grows from one scenario to the next.
2. **Roads & critical services** — loads the road network and three categories of critical facilities (hospitals, fire stations, schools), prepared from OpenStreetMap and stored in this repository.
3. **Depth-dependent road disruption** — classifies every road segment by the water depth on it, following the depth–disruption relationship of Pregnolato et al. (2017):
   - **≥ 0.30 m** → impassable
   - **0.10 – 0.30 m** → passable at reduced speed
   - **< 0.10 m** → normal
4. **Accessibility analysis** — computes 5 / 10 / 15-minute drive-time isochrones around each facility, under normal conditions and under each flood scenario, so the loss of coverage is visible on the map.
5. **Who is affected** — overlays the results with an SCB population grid and NMD2018 land cover to estimate how many people, and what kind of land, fall outside emergency reach.

No programming experience is needed: all the code is pre-written, and participants run the cells top to bottom and interpret what they see.

## How to run it

Click the **Open in Colab** badge above. Everything runs in the browser, and the input data is downloaded automatically from this repository, so nothing needs to be installed locally.

## Data in this repository

| File | Content | Source |
|---|---|---|
| `Karlstad_50_djup.tif` | Flood depth (m), 50-year scenario | MSB |
| `Karlstad_100_djup.tif` | Flood depth (m), 100-year scenario | MSB |
| `Karlstad_200_djup.tif` | Flood depth (m), 200-year scenario | MSB |
| `Karlstad_nmd2018.tif` | National Land Cover Data (NMD2018), clipped to Karlstad | Naturvårdsverket |
| `SCB_population_Karlstad_5km.gpkg` | Population grid, clipped to the study area | SCB (Statistics Sweden) |
| `karlstad_drive.graphml` | Drivable road network as a routing graph (used for the isochrones) | OpenStreetMap, via OSMnx |
| `karlstad_osm.gpkg` | Road network geometries for mapping and flood intersection | OpenStreetMap |
| `karlstad_pois.gpkg` | Critical facilities: hospitals, fire stations, schools | OpenStreetMap |

The OpenStreetMap layers are **pre-fetched snapshots**, so the workshop runs the same way every time and doesn't depend on live OSM servers during the session. Because OSM is constantly edited, a fresh download today may differ slightly from these files.

## What this method can (and can't) tell us

Because the flood input is **depth** rather than a simple wet/dry mask, the notebook can distinguish between a road that is merely slowed down and one that is completely cut off, which is much closer to how emergency vehicles actually experience a flood.

It is still a simplified picture. The scenarios are modelled hazard maps, not observations of a real event. The depth thresholds are general values from the literature, not specific to Swedish emergency vehicles. Travel times assume free-flowing traffic, and the analysis does not account for temporary measures such as barriers, detours or evacuation routes. Treat the results as a way to see *where* accessibility is most fragile, not as an operational forecast.

## Companion notebook

For an event-based version of the same workflow (detecting a real flood from Sentinel-1 radar imagery instead of using modelled scenarios), see [Beginner EO Analysis Series: From flood mapping to critical infrastructure accessibility](https://github.com/amaletaa/SESAC-Beginner_EO_Analysi_Series), built on the February 2020 floods in the Viskan valley (Borås–Skene, Copernicus EMS activation EMSR427). Because it starts from satellite observations of an actual event, it shows where water *really* appeared, day or night and through cloud, although it can only tell flooded from not flooded, without depth.

## Credits & data licenses

- **Flood-depth scenarios** — Myndigheten för samhällsskydd och beredskap (MSB), översvämningskartering. <!-- TODO: check and add MSB's terms of use -->
- **Roads & critical facilities** — © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors, ODbL.
- **Population grid** — SCB (Statistics Sweden).
- **Land cover (NMD2018)** — Naturvårdsverket (Swedish Environmental Protection Agency), CC0.
- **Depth–disruption function** — Pregnolato, M., Ford, A., Wilkinson, S. M., & Dawson, R. J. (2017). The impact of flooding on road transport: A depth-disruption function. *Transportation Research Part D*, 55, 67–81.
- Workshop developed for **SESAC** (Swedish Competence Centre for Satellite-Enabled Social Science Analytics), with support from Rymdstyrelsen (Swedish National Space Agency).

Code in this repository is shared under the MIT License; the datasets above retain their own original licenses as listed.

## Author

Chantziara Amalia Nikoleta — SESAC, Project Assistant.

---

*If you use or adapt this notebook, a link back to this repository is appreciated but not required.*
