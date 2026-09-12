# DMV Housing Affordability & Commute Time

🔗 [Live interactive site](https://ahmadzaihabib.github.io/dmv_housing_affordability/)

**Author:** Habib Gul Ahmadzai | **Course:** Command-Line GIS (Bloustein School of Planning and Public Policy, Rutgers University)

## 1. Overview

This project analyzes the spatial distribution of housing affordability and commuting time in the Washington, DC-Maryland-Virginia (DMV) area, using census tract-level American Community Survey (ACS) 2022 data. Housing cost and commute time are major elements of daily accessibility and quality of life, and their distribution can reveal significant inequalities between neighborhoods.

The project uses both static and interactive maps to visualize how median gross rent and mean commute time differ across census tracts, while flagging regions with missing or uncertain estimates. The interactive web map allows dynamic comparison between layers and tract-level detail via hover tooltips.

**Project Components**

- Static Map 1: Median Gross Rent by census tract (ACS 2022)
- Static Map 2: Mean Commute Time by census tract (ACS 2022)
- Side-by-side comparison of housing cost and commute burden
- Interactive web map with layer toggles & tract-level hover tooltips

## 2. Static Maps

### Static Map 1: Median Gross Rent by Census Tract (ACS 2022)

Hatched tracts indicate low-confidence estimates where the coefficient of variation (CV) is 40% or higher. Missing data is shown in gray.

![Median Gross Rent by Census Tract](https://ahmadzaihabib.github.io/dmv_housing_affordability/median_rent_cv_map.png)

### Static Map 2: Mean Commute Time by Census Tract (ACS 2022)

Mean one-way commute time in minutes. Missing data shown in gray.

![Mean Commute Time by Census Tract](https://ahmadzaihabib.github.io/dmv_housing_affordability/mean_commute_map.png)

## 3. Side-by-Side Comparison

### Housing Cost vs. Commute Burden (ACS 2022)

Left: Median Gross Rent by census tract. Right: Mean Commute Time by census tract.

![Side-by-side comparison of housing cost and commute time](https://ahmadzaihabib.github.io/dmv_housing_affordability/Housing%20Cost%20vs.%20Commute%20Burden%20(ACS%202022).png)

## 4. Interactive Webmap

The interactive map lets users investigate the geographic distribution of housing prices and commute burden in the DMV area. Users can switch between median gross rent, median household value, and mean commute time layers, and hover over individual census tracts to view details including tract ID, median rent, median household income, and mean commute time. County lines are shown in thick white lines and labeled for geographic context.

🔗 [View the interactive map](https://ahmadzaihabib.github.io/dmv_housing_affordability/dmv_interactive.html)

## 5. Data Description

### Datasets Used

- **American Community Survey (ACS) 2022:** Median gross rent, median household income, and mean commute time (U.S. Census Bureau).
- **Census Tract Geometries:** Used to spatially join and map ACS variables.
- **County Boundaries:** Used for reference outlines and labeling.

### Processing & Methods

- ACS attributes joined to census tract geometries using GEOID.
- Missing values and low-confidence estimates (CV ≥ 40%) flagged and handled.
- Geometries reprojected and simplified for static and interactive mapping.

### Data Quality Notes

- Some tracts contain missing or low-confidence ACS estimates.
- Web map geometries were simplified to improve performance.
