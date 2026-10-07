# DMV Housing Affordability & Commute Time: Census ETL Pipeline & Geospatial Analysis

🔗 [Live interactive site](https://ahmadzaihabib.github.io/dmv_housing_affordability/) · 📓 [Pipeline notebook](dmv_etl_pipeline.ipynb)

An end-to-end Python ETL pipeline that pulls raw federal data from the U.S. Census Bureau, cleans and validates it, and turns it into analysis-ready, tract-level datasets and maps showing where housing costs and commute burdens concentrate across the Washington, DC metro area.

**Stack:** Python · pandas · GeoPandas · Census API (`censusdata`) · Matplotlib · mapclassify · contextily · Folium

## 1. Overview

This project analyzes the spatial distribution of housing affordability and commuting time across **997 census tracts in 5 DC-area jurisdictions**: the District of Columbia, Montgomery County (MD), Prince George's County (MD), Fairfax County (VA), and Arlington County (VA). Housing cost and commute time are major elements of daily accessibility and quality of life, and their distribution can reveal significant inequalities between neighborhoods.

The project uses both static and interactive maps to visualize how median gross rent and mean commute time differ across census tracts, while flagging regions with missing or uncertain estimates.

## 2. Data Pipeline

```
Census API (ACS 2022 5-yr) ─┐
TIGER/Line tract geometries ─┼─► clean & validate ─► GEOID join ─► derive metrics ─► static + interactive maps
LODES 2020 workplace jobs ──┘
```

1. **Extract**
   - Pulls 2022 ACS 5-year estimates via the Census API (`censusdata`) for each of the 5 counties: median gross rent, median household income (with margins of error), and commute-time distributions.
   - Downloads TIGER/Line 2023 tract and county geometries for DC, Maryland, and Virginia, then filters them to the 997 study-area tracts.
   - Loads 80K+ LEHD LODES 2020 Workplace Area Characteristics records (block-level job counts for DC, MD, and VA).
2. **Transform**
   - Replaces Census suppression codes (`-666666666`, `-333333333`, `-222222222`) with nulls and coerces all fields to numeric.
   - Computes coefficients of variation, CV = (MOE / 1.645) / estimate × 100, and flags low-confidence estimates (CV ≥ 40%).
   - Derives a weighted mean commute time per tract from the ACS commute-time bins using bin midpoints.
   - Aggregates LODES block-level jobs to tracts (first 11 digits of the block geocode).
   - Joins ACS attributes, job counts, and geometries on GEOID, then reprojects for mapping.
3. **Load & publish**
   - Static choropleth maps (Matplotlib, quantile classification, CV hatching).
   - Interactive Folium web map with layer toggles and tract-level tooltips, hosted on GitHub Pages.

## 3. Static Maps

### Static Map 1: Median Gross Rent by Census Tract (ACS 2022)

Hatched tracts indicate low-confidence estimates where the coefficient of variation (CV) is 40% or higher. Missing data is shown in gray.

![Median Gross Rent by Census Tract](https://ahmadzaihabib.github.io/dmv_housing_affordability/median_rent_cv_map.png)

### Static Map 2: Mean Commute Time by Census Tract (ACS 2022)

Mean one-way commute time in minutes. Missing data shown in gray.

![Mean Commute Time by Census Tract](https://ahmadzaihabib.github.io/dmv_housing_affordability/mean_commute_map.png)

## 4. Side-by-Side Comparison

### Housing Cost vs. Commute Burden (ACS 2022)

Left: Median Gross Rent by census tract. Right: Mean Commute Time by census tract.

![Side-by-side comparison of housing cost and commute time](https://ahmadzaihabib.github.io/dmv_housing_affordability/Housing%20Cost%20vs.%20Commute%20Burden%20(ACS%202022).png)

## 5. Interactive Webmap

The interactive map lets users investigate the geographic distribution of housing prices and commute burden in the DMV area. Users can switch between median gross rent, median household value, and mean commute time layers, and hover over individual census tracts to view details including tract ID, median rent, median household income, and mean commute time. County lines are shown in thick white lines and labeled for geographic context.

🔗 [View the interactive map](https://ahmadzaihabib.github.io/dmv_housing_affordability/dmv_interactive.html)

## 6. Data Sources

- **American Community Survey (ACS) 2022 5-year estimates:** Median gross rent, median household income, and commute-time distributions, via the Census API (U.S. Census Bureau).
- **LEHD LODES 2020 (Workplace Area Characteristics):** 80K+ block-level workplace job records for DC, Maryland, and Virginia (U.S. Census Bureau).
- **TIGER/Line 2023 tract and county geometries:** Used to spatially join, map, and label ACS variables.

## 7. Data Quality Notes

- Of 997 tracts, 56 have no published median rent and 111 lack the margin of error needed to compute a rent CV; these are shown in gray or excluded from CV flagging.
- Low-confidence estimates (CV ≥ 40%) are hatched on the rent map rather than dropped.
- Web map geometries were simplified to improve performance.

## Running it

Open [`dmv_etl_pipeline.ipynb`](dmv_etl_pipeline.ipynb) in Jupyter or Google Colab and run all cells. All data is downloaded directly from Census Bureau sources at runtime.

---

**Author:** Habib Gul Ahmadzai · Originally developed for Command-Line GIS (Bloustein School of Planning and Public Policy, Rutgers University, Fall 2025)
