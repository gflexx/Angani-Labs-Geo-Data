# Angani Geo Data

Curated, open-source geospatial datasets for Kenya and East Africa, built for the [Angani Labs](https://github.com/gflexx/angani-labs) geospatial intelligence platform.

## 🌍 Overview
This repository hosts lightweight, optimized GeoJSON files representing critical infrastructure, environmental, and administrative features in Kenya. The data is generated programmatically via the OpenStreetMap (OSM) Overpass API, cleaned, simplified, and exported for direct consumption by frontend mapping libraries (e.g., Leaflet, Mapbox GL JS).

## 📂 Available Datasets

| Layer | File | Geometry | Description | Theme Color |
| :--- | :--- | :--- | :--- | :--- |
| **Counties** | `counties.geojson` | Polygon | Administrative boundaries for Kenya's 47 counties. | Emerald (`#10b981`) [TODO] |
| **Protected Areas** | `protected_areas.geojson` | Polygon | National parks, nature reserves, and forest boundaries. | Dark Emerald (`#059669`) |
| **Water Points** | `water_points.geojson` | Point | Boreholes, wells, springs, and dams (critical for drought monitoring). | Amber (`#fbbf24`) |
| **Health Facilities** | `health_facilities.geojson` | Point | Hospitals and clinics (critical for humanitarian response). | Coral (`#ef4444`) |
| **Major Roads** | `roads_major.geojson` | LineString | Trunk and primary highways (for flood isolation risk mapping). | Zinc (`#71717a`) |
| **Railways** | `railways.geojson` | LineString | National railway network (e.g., SGR). | Dark Zinc (`#52525b`) |
| **Airports** | `airports.geojson` | Point | Aerodromes and airports. | Light Zinc (`#a1a1aa`) |
| **Schools** | `schools.geojson` | Point | Educational institutions (potential evacuation centers). | Amber (`#fbbf24`) |

*(More layers, including rivers, settlements, and power infrastructure, are generated and available in this repository).*

## 🚀 Usage

These files are served directly via GitHub's raw content delivery network. You can fetch them directly in your frontend application without an API key.

**Example (JavaScript / Leaflet):**
```javascript
const GEOJSON_URL = "https://raw.githubusercontent.com/gflexx/angani-geo-data/main/water_points.geojson";

fetch(GEOJSON_URL)
  .then(res => res.json())
  .then(data => {
    L.geoJSON(data, {
      pointToLayer: (feature, latlng) => L.circleMarker(latlng, {
        radius: 4,
        fillColor: "#fbbf24",
        color: "#fbbf24",
        weight: 1,
        fillOpacity: 0.8
      })
    }).addTo(map);
  });
```
---

## ⚙️ Data Pipeline Architecture

The spatial datasets powering Angani Labs are not manually curated. They are generated and maintained via an automated Python pipeline running on Kaggle:

* **Ingestion:** Queries the [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API) using targeted bounding boxes and precise [OpenStreetMap (OSM)](https://www.openstreetmap.org/) feature tags across redundant public mirrors for high availability:
  * `https://overpass-api.de/api/interpreter`
  * `https://overpass.private.coffee/api/interpreter`
  * `https://overpass.kumi.systems/api/interpreter`
* **Conversion:** Transforms raw OSM XML/JSON payloads into standardized GeoJSON via [`osm2geojson`](https://github.com/aspectumapp/osm2geojson).
* **Optimization:** Simplifies complex geometries using [**Shapely**](https://shapely.readthedocs.io/) to minimize bundle size, optimize network payloads, and guarantee high-performance client-side rendering.
* **Cleaning:** Automatically deduplicates features and validates schemas to export clean, production-ready vector layers.

> *To inspect, modify, or regenerate layers, explore the `pipeline/` directory or reference the associated Kaggle Notebook.*

---

## 🌐 Primary Data Sources

| Domain | Source | Endpoint / Portal |
| :--- | :--- | :--- |
| **Vector Infrastructure & Boundaries** | OpenStreetMap (via Overpass) | [openstreetmap.org](https://www.openstreetmap.org/) |
| **Weather & Forecasts** | Open-Meteo | [open-meteo.com](https://open-meteo.com/) |
| **Satellite Imagery & Rasters** | NASA GIBS (Earthdata) | [gibs.earthdata.nasa.gov](https://gibs.earthdata.nasa.gov/) |
| **Seismic Activity** | USGS Earthquake Hazards | [earthquake.usgs.gov](https://earthquake.usgs.gov/fdsnws/event/1/) |
| **Tropical Cyclones & Climate Indices** | NOAA / IBTrACS | [ncei.noaa.gov/products/international-best-track-archive](https://www.ncei.noaa.gov/products/international-best-track-archive) |

---

## ⚖️ License & Attribution

All vector spatial features derived within this repository are sourced from [OpenStreetMap](https://www.openstreetmap.org/), licensed under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/).

By consuming or displaying this data, you agree to uphold proper attribution standards:

> **© [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors**

### Usage Scope
This repository is maintained by **Angani Labs** for non-commercial, humanitarian, and climate-tech applications across East Africa. For commercial licensing inquiries regarding derived analytical layers, please review the official [ODbL terms](https://opendatacommons.org/licenses/odbl/).

---

## 🤝 Contributing & Maintenance

Spotted an unmapped water point, a missing health facility, or an outdated regional boundary?

The canonical source of truth is OpenStreetMap. The most effective way to contribute is to [**edit OpenStreetMap directly**](https://www.openstreetmap.org/edit). Subsequent executions of the automated pipeline will automatically ingest, process, and reflect your updates.
