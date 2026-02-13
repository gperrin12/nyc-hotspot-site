# NYC Taxi Hotspot Analysis: Saturday 2AM vs Monday 8AM

An interactive geospatial visualization comparing normalized taxi pickup density across New York City at two contrasting moments — late-night Saturday and Monday morning rush hour.

**[🔗 Live Demo →](https://gperrin12.github.io/nyc-hotspot-site/)**

![Overlay View](screenshots/overlay.png)

---

## Overview

New York City's taxi demand tells two very different stories depending on when you look. This project visualizes the spatial fingerprint of those stories using 2015 NYC Taxi & Limousine Commission trip data, comparing:

- **Saturday 2:00 AM** — peak nightlife hours
- **Monday 8:00 AM** — morning rush hour

Each time window is independently normalized so the visualization reveals *where* demand concentrates, not just where volume is highest.

## Key Findings

| Metric | Saturday 2 AM | Monday 8 AM |
|--------|--------------|-------------|
| **Hotspot center** | Lower East Side / East Village | Midtown / Penn Station |
| **Character** | Tight nightlife cluster below 14th St | Dispersed commuter pattern with transit hubs |
| **Top areas** | LES, East Village, West Village, Chelsea/Meatpacking | Penn Station, Times Square, Grand Central, UES, LGA, JFK |
| **Spatial dispersion** | ~1.2 km (concentrated) | ~2.8 km (spread across boroughs) |

The weighted center of gravity shifts approximately **2.5 km north** between Saturday night and Monday morning — a clean signature of the nightlife-to-commute transition.

![Saturday 2AM](screenshots/saturday.png)
*Saturday 2AM: Nightlife corridors dominate downtown Manhattan*

![Monday 8AM](screenshots/monday.png)
*Monday 8AM: Demand disperses to Midtown transit hubs and airports*

![Difference View](screenshots/difference.png)
*Difference mode: Pink = Saturday-dominant cells, Blue = Monday-dominant cells*

## Features

- **Overlay mode** — both time windows on one map to see spatial overlap
- **Individual views** — isolate Saturday or Monday patterns
- **Difference mode** — highlights cells unique to each time window (pink = Saturday-dominant, blue = Monday-dominant)
- **Heatmap toggle** — broader glow rendering for density visualization
- **Interactive basemap** — pan/zoom on CartoDB Dark Matter tiles via Leaflet
- **Summary statistics** — center of gravity, spatial dispersion, top neighborhoods with bar charts

## Methodology

### Data Source

NYC Taxi & Limousine Commission pre-2016 archive (`par` table), containing raw GPS pickup coordinates (latitude/longitude) for all yellow and green taxi trips. Analysis covers the full 2015 calendar year.

### Spatial Binning

Raw GPS coordinates are rounded to 0.001° resolution, creating a grid of approximately **110-meter cells**. This balances spatial precision with sufficient aggregation for clear pattern recognition.

### Temporal Filtering

- **Saturday 2 AM**: `day_of_week = 6` (Saturday) AND `hour = 2` across all weeks in 2015
- **Monday 8 AM**: `day_of_week = 1` (Monday) AND `hour = 8` across all weeks in 2015

### Normalization

Each dataset is **independently min-max normalized** to a [0, 1] scale. This is critical because the two time windows have different absolute trip volumes — normalization ensures the visualization compares spatial *patterns* rather than raw counts.

Dot **size** and **opacity** are linearly mapped from the normalized value:
- `radius = base + normalized × scale`
- `opacity = 0.15 + normalized × 0.78`

### Metrics

- **Center of Gravity**: Trip-count-weighted mean of lat/lon coordinates
- **Spatial Dispersion**: Weighted standard deviation of point distances from the center of gravity, converted to kilometers (× 111 km/degree)
- **Neighborhood Classification**: Bounding-box lookup against manually defined Manhattan neighborhood boundaries

### Limitations

- Only the **top 80 grid cells** per time window are visualized (by pickup count)
- Neighborhood bounding boxes are approximate and may misclassify edge cases
- Pre-2016 data only captures yellow and green taxis — no rideshare (Uber/Lyft) data
- GPS accuracy varies; some coordinates may be imprecise

## Tech Stack

- **Leaflet** with CartoDB Dark Matter tiles for the dark basemap
- **Canvas overlay** for high-performance point rendering with glow effects
- **Vanilla JavaScript** — no build step, no dependencies beyond CDN-hosted libraries
- Single `index.html` file — just open it or deploy anywhere

## Screenshots

> **Note:** Replace the screenshot paths above with actual images. To capture them:
> 1. Open `index.html` in your browser
> 2. Set each view mode (Overlay, Sat, Mon, Difference)
> 3. Take screenshots and save to a `screenshots/` folder in the repo

## Local Development

No build process required. Just open the file:

```bash
# Clone the repo
git clone https://github.com/yourusername/your-repo-name.git

# Open in browser
open index.html
```

Or serve locally:

```bash
python -m http.server 8000
# Visit http://localhost:8000
```

## Deploy

The simplest deployment options:

- **GitHub Pages**: Push to repo → Settings → Pages → Deploy from `main` branch
- **Netlify**: Drag the folder to [app.netlify.com/drop](https://app.netlify.com/drop)
- **Vercel**: `npx vercel` in the project directory

## Data Attribution

Trip data provided by the [NYC Taxi & Limousine Commission](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page). Basemap tiles by [CARTO](https://carto.com/) and [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors.

## License

MIT
