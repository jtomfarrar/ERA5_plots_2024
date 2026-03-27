# ERA5_plots_2024

Extract ERA5 reanalysis data from the Copernicus Climate Data Store (CDS) API and produce plots and statistics for oceanographic research campaigns. Supports two workflows: long single-site timeseries (with climatology and histograms) and regional gridded maps (with spatial plots and animations).

---

## Setup

**1. Create and activate the conda environment:**
```bash
conda env create -f environment.yml
conda activate NORSE_ASTRAL
```

**2. Configure the CDS API key.**
A `~/.cdsapirc` file is required before any data can be downloaded. See:
https://cds.climate.copernicus.eu/api-how-to

---

## Repository structure

```
ERA5_plots_2024/
├── src/                  Python scripts (extraction and plotting)
├── data/
│   └── processed/
│       └── timeseries/   Processed single-site NetCDF files
├── img/                  Output plots and animations
└── environment.yml
```

---

## Workflow — Tier 1: Single-site timeseries

Use this workflow to extract a long hourly record at a point location, compute monthly climatology, and generate standard plots.

**Step 1.** Add or select a site in `src/ERA5_timeseries_sites_config.py`.

**Step 2.** Run `src/ERA5_timeseries_extraction.py` to download and process data.
- Downloads hourly ERA5 surface and wave variables from CDS
- Derives relative humidity, wind speed, and converts units
- Saves processed data to `data/processed/timeseries/ERA5_surface_[SITE]_site_timeseries.nc`

**Step 3.** Run `src/ERA5_timeseries_plots_stats.py` to generate all products.
- Monthly climatology NetCDF
- Locator map, 5-panel timeseries summary, supplemental timeseries, histograms, 3×3 climatology grid
- Text file of extreme wave events (Hs > 10 m)

### Configured sites

| Site | Longitude | Latitude | Date range |
|---|---|---|---|
| RAMA_12N | 88.5°E | 12.0°N | 2000–2026 |
| ASTRAL_2025_Ida | 87.68°E | 13.21°N | 2000–2026 |
| ASTRAL_2025_Kelvin | 87.54°E | 13.21°N | 2000–2026 |
| ASTRAL_2025_Planck | 87.61°E | 13.26°N | 2000–2026 |
| ASTRAL_2025_WHOI43 | 87.61°E | 13.16°N | 2000–2026 |
| Endurance_RCA | 130.2°W | 44.98°N | 2000–2026 |
| SAFARI | 161.0°W | 35.0°N | 2000–2026 |

---

## Workflow — Tier 2: Regional gridded maps

Use this workflow to extract ERA5 data over a spatial domain for a campaign period and produce map plots or animations.

**Step 1.** Run the campaign extraction script to download gridded ERA5 data.

**Step 2.** Run the matching plot script to create maps and (optionally) animation frames.

### Configured campaigns

| Campaign | Region | Period | Extraction script | Plot script |
|---|---|---|---|---|
| NORSE 2023 | Norwegian/Icelandic waters | Nov 2023 | `ERA5_NORSE_map_extraction.py` | `ERA5_NORSE_plots.py` |
| ASTRAL 2024 | Bay of Bengal | May–Jul 2024 | `ERA5_ASTRAL_map_extraction.py` | `ERA5_ASTRAL_plots.py` |
| SAFARI | South Pacific | 2000–2026 | — | `ERA5_SAFARI_plots.py` |
| S-MODE 2022 | U.S. West Coast | 2022 | `ERA5_SMODE_IOP1_map_extraction.py` | `ERA5_SMODE_plots.py` |

---

## Key scripts

| Script | Role |
|---|---|
| `ERA5_extraction_tool.py` | Core CDS download functions; used by all extraction scripts |
| `ERA5_timeseries_sites_config.py` | Site coordinates, date ranges, and map extents |
| `ERA5_timeseries_extraction.py` | Download and process single-site hourly timeseries |
| `ERA5_timeseries_plots_stats.py` | Timeseries plots, climatology, histograms, extreme events |
| `ERA5_[CAMPAIGN]_map_extraction.py` | Download regional gridded ERA5 for a campaign |
| `ERA5_[CAMPAIGN]_plots.py` | Spatial map plots and animation frames |

---

## Output file naming

| File | Description |
|---|---|
| `ERA5_surface_[SITE]_site_timeseries.nc` | Processed hourly timeseries with derived variables |
| `ERA5_surface_[SITE]_site_monthly_climatology.nc` | Monthly mean climatology |
| `ERA5_surface_[SITE]_wave_height_gt10m.txt` | Extreme wave event list (CSV) |
| `ERA5_surface_[CAMPAIGN]_[YEAR].nc` | Regional gridded surface variables |
| `ERA5_surface_[CAMPAIGN]_waves_[YEAR].nc` | Regional gridded wave variables |
| `img/[SITE]_*.png` | Plots: locator map, timeseries, climatology, histograms |

---

## Links

- CDS account and API key: https://cds.climate.copernicus.eu/user
- ERA5 single-levels dataset: https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels
- CDS request queue: https://cds.climate.copernicus.eu/live/queue
