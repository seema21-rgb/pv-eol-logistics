# PV end-of-life waste logistics: a GIS-LCA decision support framework

Code for the master's thesis *City-Scale Photovoltaic End-of-Life Waste Logistics: A GIS-LCA Based Decision Support Framework Applied German Municipalities* (Seema Rehman, International Master of Environmental Sciences, University of Cologne, 2026).

The framework projects photovoltaic end-of-life waste at postal-code level from the German Marktstammdatenregister (MaStR), validates the postal-code centroid used for installations whose coordinates are withheld, and calculates Stage 2 transport work, transport-stage life cycle impacts and transport cost to certified recycling facilities. It is applied to Cologne and tested in Bonn and Euskirchen.

## Contents

| File | Thesis section | What it does |
|---|---|---|
| `notebooks/01_Cologne_pipeline_scripts_01-14.ipynb` | 4.1 to 4.9.1, 5.2 to 5.6.1 | Full Cologne pipeline: extraction, exclusion criteria, mass model, retirement windows, postal-code aggregation, centroid validation, collection-point assignment, facility distances, transport work, LCA, lifetime sweep |
| `notebooks/02_Bonn_pipeline_scripts_01-14.ipynb` | 5.7 | Same pipeline for Bonn |
| `notebooks/03_Euskirchen_pipeline_scripts_01-14.ipynb` | 5.7 | Same pipeline for Euskirchen |
| `notebooks/15_monte_carlo.ipynb` | 4.9.2, 5.6.2 | Monte Carlo analysis (10,000 runs): mean lifetime, Weibull shape factor and detour factor |
| `notebooks/16_prospective_lca_and_cost.ipynb` | 4.8.2, 4.8.3, 5.5.2, 5.6.3 | Prospective emission factors (indices only) and transport cost |
| `notebooks/17_facility_scenarios.ipynb` | 4.7, 5.5.3 | Facility scenarios FS-A, FS-B and FS-C with hypothetical sites |
| `notebooks/18_centroid_flip_test.ipynb` | 5.2 | Does the centroid change the choice of collection point or facility? |
| `checks/check_30kw_boundary.ipynb` | 4.3 | Large records without coordinates at the 30 kW boundary |
| `checks/20_excluded_mass_share.py` | 4.3 | Share of modelled mass in large records without coordinates |
| `checks/check_extraction_completeness.py` | 4.1 | Records with a study postal code but a different municipality name |

Every notebook keeps the printed output of the run reported in the thesis.

## Requirements

- ArcGIS Pro 3.4.0 with its Python environment (`arcpy`). The spatial steps need a licensed ArcGIS installation.
- Python 3 with `numpy` and `matplotlib` (see `requirements.txt`).
- The prospective indices in notebooks 15 and 16 were derived with premise 2.4.4, ecoinvent 3.12 (cut-off) and REMIND (SSP2-NPi, SSP2-PkBudg650) in Brightway 2. ecoinvent is licensed, so only the indices relative to 2025 are published here, never absolute ecoinvent values.
- The static factors per tonne-kilometre come from the ELCD 3.2 lorry process (openLCA format by GreenDelta, v2.18) with ReCiPe 2016 midpoint (H) in openLCA 2.6.1.

## Input data

The input data are not part of this repository. To re-run the notebooks, prepare these tables in one file geodatabase and set the `GDB` path at the top of each script:

| Table | Content | Source |
|---|---|---|
| `mastr_pv_for_gis` | National MaStR export of photovoltaic units (downloaded October 2025, 5,730,136 units) | Bundesnetzagentur, Data Licence Germany – Attribution – Version 2.0 |
| `plz_cologne`, `plz_cologne_centroids` | Cologne postal-code polygons and their centroids | Offene Daten Köln |
| `OSM_PLZ.shp` | Postal-code polygons for Bonn and Euskirchen | Esri Deutschland, PLZ_Gebiete (OpenStreetMap contributors) |
| `facilities_utm` | Reiling recycling sites in Münster, Torgau and Marienfeld (EPSG:25832) | Company addresses |
| `awb_collection_points` | Cologne collection centres Gremberghoven and Ossendorf | AWB Köln |

The collection points of Bonn (Wertstoffhof Südstraße) and Euskirchen (AWZ Mechernich) are set in the Bonn and Euskirchen notebooks.

## Order of execution

1. Run the city notebooks 01, 02 and 03 from top to bottom. Later cells use tables written by earlier cells.
2. Run notebooks 15 to 18. Each one first reproduces the published transport work and reports any deviation.
3. The scripts in `checks/` are read-only and can be run at any time after the city notebooks.

## Main settings

- Size classes: small below 30 kW gross capacity, large at 30 kW or more.
- Mass model: mass-to-power ratio = 2.72 × 10²⁵ · exp(−year / 37.11) t/MW (Nieto-Morone et al., 2023).
- Retirement: fixed 30-year lifetime; windows 2026–2035, 2036–2045 and 2046–2055 (horizons 2030, 2040, 2050); sweep from 20 to 40 years.
- Road distance: straight-line distance in ETRS89 / UTM 32N multiplied by 1.3 (Heinold & Makowski, 2026).
- Transport cost: €0.0717 per tonne-kilometre (CNR, 2021; 2021 prices, undiscounted).

## Licence

The code is released under the MIT licence. Data sources keep their own licences.

## Citation

Rehman, S. (2026). *City-Scale Photovoltaic End-of-Life Waste Logistics: A GIS-LCA Based Decision Support Framework Applied German Municipalities*. Master's thesis, University of Cologne.
