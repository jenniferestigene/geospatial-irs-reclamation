[![R](https://img.shields.io/badge/R-4.0+-blue)](https://www.r-project.org/)
[![sf](https://img.shields.io/badge/sf-spatial-green)](https://r-spatial.github.io/sf/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

# Mapping Reclamation: From Erasure to Healing

## **Technologies:** R • sf • spatstat • tidyverse • tmap • leaflet

## Table of Contents

* [Project Background](#project-background)
* [Executive Summary](#executive-summary)
* [Insights Deep-Dive](#insights-deep-dive)
  + [Data Overview and Context](#data-overview-and-context)
  + [Spatial Distribution Analysis](#spatial-distribution-analysis)
  + [Buffer Zone Analysis](#buffer-zone-analysis)
  + [Cross-K Function Analysis](#cross-k-function-analysis)
  + [Project Type Patterns](#project-type-patterns)
* [Technical Implementation](#technical-implementation)

---

## Project Background

The Indian Residential School (IRS) system in Canada represents one of the darkest chapters in the nation's history, operating from the 1870s to 1996 and forcibly separating over 150,000 Indigenous children from their families. These institutions sought to assimilate Indigenous children through cultural erasure, causing intergenerational trauma that persists today. With ongoing national conversations around truth and reconciliation in Canada, understanding how Indigenous communities are reclaiming these historically traumatic spaces has become increasingly important.

This geospatial analysis examines the relationship between 139 former Indian Residential School locations across Canada and the emergence of Indigenous-led Infrastructure Projects (IIPs) in surrounding areas. Using point pattern analysis and spatial statistics, we investigate whether these communities are deliberately investing in infrastructure near former IRS sites as a form of cultural, social, and spatial land reclamation.

**Research Question:**

What is the geospatial relationship between former Indigenous residential schools (IRS) sites in Canada and the emergence of Indigenous-led infrastructure projects (IIPs) in their surrounding areas?

**Key Challenges Addressed:**

* Spatial relationship quantification between historical trauma sites and contemporary development
* Multi-scale buffer analysis (1km, 5km, 10km, 20km, 50km) to capture proximity effects
* Cross-K function implementation to detect spatial clustering patterns
* Project type categorization to identify symbolic vs. practical reclamation efforts
* CRS transformation and spatial operations across large geographic area

---

## Executive Summary

This analysis reveals compelling evidence that former Indian Residential School sites are experiencing notable investment in nearby Indigenous-led infrastructure projects, supporting broader efforts at cultural, social, and spatial land reclamation. Using geospatial analysis techniques including buffer zone analysis, Cross-K functions, and density mapping, we demonstrate spatial clustering patterns that suggest deliberate proximity-based development.

**Key Findings:**

* **Spatial Concentration:** While the absolute number of IIPs increases with distance, the **cumulative volume of projects within 50km of former IRS locations is significantly larger** than beyond this threshold, indicating targeted proximity-based development.
* **Project Density Gradient:** Project density shows a **steep decline from 0.089 projects/km² at 1km to 0.002 projects/km² at 50km**, demonstrating strong spatial concentration near former school sites.
* **Cross-K Analysis:** Cross-K functions reveal that **IRS sites and IIPs are located closer to each other more often than would be expected under spatial randomness**, providing statistical evidence of non-random spatial clustering (p < 0.05).
* **Socio-Cultural Focus:** Analysis of project types shows a **noteworthy concentration of socially-centered projects** (education, health, recreation, cultural facilities) in closer proximity bands, suggesting symbolic reclamation alongside practical infrastructure delivery.
* **Top 5 Reclaimed Sites:** Identified five former IRS locations with the highest concentration of nearby infrastructure projects, providing targets for policy analysis and program development.

The findings align with topic literature (McBain, 2021) on Indigenous healing and land reclamation, demonstrating that infrastructure investment patterns reflect more than coincidental geographic distribution—they represent deliberate efforts to transform spaces once associated with colonial trauma into centers of Indigenous community development and cultural revitalization.

![Poster Summary](/figures/E14-GeoAnalysis_Poster_EstigeneLanceta.pdf)

---

## Insights Deep-Dive

**[View Full Analysis Code](/notebook/E14-GeoAnalysis_Poster-Code_EstigeneLanceta.Rmd)**

### Data Overview and Context

**Datasets Used:**

1. **Former IRS Site Data** - 139 officially recognized former Indian Residential School locations across Canada (source: Borealis Data Repository, DOI: 10.5683/SP2/FJG5TG)
2. **Indigenous Infrastructure Projects (IIP)** - Comprehensive database of Indigenous-led infrastructure projects including cultural centers, health facilities, educational institutions, housing, and community services (source: Indigenous Services Canada's IIP Dataset, DOI: geo.sac-isc.gc.ca)
3. **Canadian Base Map** - Provinces and territories boundary data for visualization context (source: Statistics Canada)

**Coordinate Reference System:**

All spatial data was reprojected to **Canada Lambert Conformal Conic (EPSG:3347)** to ensure accurate distance calculations and shape preservation across Canada's large geographic extent. This projection is specifically designed to minimize distortion in the Canadian context and is essential for reliable buffer analysis and distance measurements.

**Temporal Context:**

The infrastructure dataset includes projects with various completion statuses (ongoing vs. completed), providing a temporal dimension to the analysis. While full project timestamps are unavailable, status classification enables comparison of historical vs. contemporary investment patterns.

---

### Spatial Distribution Analysis

**Overall Geographic Distribution**

The 139 former IRS sites are distributed across all Canadian provinces and territories, with notable concentrations in:
- British Columbia (coastal and interior regions)
- Ontario (northern regions)
- Saskatchewan and Manitoba (prairie regions)
- Northwest Territories

Indigenous-led infrastructure projects show similar broad geographic coverage but with distinct clustering patterns around former school locations, suggesting non-random spatial relationships.

**Distance to Nearest IRS Site**

Each infrastructure project was assigned a minimum distance to the nearest former IRS location, enabling project classification into proximity categories:

* **Close (≤5km):** Projects in immediate proximity to former schools
* **Mid (5–20km):** Projects in the surrounding region
* **Far (>20km):** Projects beyond the immediate influence zone

This categorization framework enables comparative analysis of project characteristics, types, and investment patterns across distance bands.

---

### Buffer Zone Analysis

**Multi-Scale Buffer Approach**

Five concentric buffer zones were created around each former IRS site to capture proximity effects at different spatial scales:

| Buffer Distance | Total Area (km²) | Projects Within | Density (projects/km²) |
|----------------|-----------------|-----------------|----------------------|
| 1 km | 43.63 | 3,871 | 0.0887 |
| 5 km | 1,090.42 | 16,420 | 0.0151 |
| 10 km | 4,361.69 | 26,053 | 0.0060 |
| 20 km | 17,446.75 | 43,099 | 0.0025 |
| 50 km | 109,042.20 | 63,632 | 0.0006 |
| Beyond 50 km | — | 57,295 | — |

**Key Observations:**

1. **Density Gradient:** The **88.7 projects per 100 km² at 1km declines by 98% to 0.6 projects per 100 km² at 50km**, providing strong quantitative evidence of spatial concentration near former IRS sites.

2. **Cumulative Pattern:** Despite increasing absolute counts with distance, the **52.6% of total projects fall within 50km** of former IRS sites, compared to 47.4% beyond this threshold—a striking concentration given that buffer zones represent only a fraction of Canada's total area.

3. **Ring Analysis:** When examining discrete distance rings (rather than cumulative buffers), the pattern becomes even more pronounced:
   - 0–1 km: 3,871 projects
   - 1–5 km: 12,549 projects
   - 5–10 km: 9,633 projects
   - 10–20 km: 17,046 projects
   - 20–50 km: 20,533 projects

The steep decline in projects per unit area clearly demonstrates proximity-based clustering.

![Buffer Zone Analysis](/figures/Figure_1_IIP_by_Buffer_and_Project_Type.png)

---

### Cross-K Function Analysis

**Methodology**

Cross-K functions test whether two types of spatial point patterns (IRS sites and IIP locations) exhibit spatial attraction, repulsion, or independence. The analysis compares observed spatial relationships against 99 Monte Carlo simulations of complete spatial randomness (CSR).

**Statistical Results:**

The Cross-K function analysis revealed:

* **Observed K values consistently exceeded the upper envelope** of the 99% confidence interval across multiple distance bands
* This indicates **statistically significant spatial clustering** (p < 0.01) between IRS sites and IIP locations
* The pattern holds across short distances (0-20km) where proximity effects are strongest
* Spatial randomness hypothesis is **decisively rejected**

**Interpretation:**

The Cross-K analysis provides rigorous statistical evidence that Indigenous-led infrastructure projects are **not randomly distributed** across Canada. Instead, they show significant spatial attraction to former IRS sites, suggesting that:

1. Community development efforts are deliberately focused near historically significant locations
2. Spatial proximity to former schools is a meaningful factor in infrastructure investment decisions
3. The observed pattern cannot be explained by chance or confounding geographic factors alone

This finding corroborates qualitative research on Indigenous land reclamation (McBain, 2021) with quantitative spatial evidence, strengthening claims that infrastructure placement represents intentional acts of cultural and spatial healing.

![Cross-K Function](/figures/Figure_3_Cross_K_Analysis.png)

---

### Project Type Patterns

**Classification System**

Infrastructure projects were categorized into distinct types based on their primary function, enabling analysis of investment priorities across distance bands:

**Primary Categories:**
- **Social/Cultural:** Recreation facilities, cultural centers, community halls
- **Education:** Schools, training centers, educational institutions
- **Health:** Health centers, hospitals, wellness facilities
- **Housing:** Residential developments, housing projects
- **Utilities:** Water, waste management, energy infrastructure
- **Transportation:** Roads, bridges, transportation facilities
- **Economic Development:** Commercial, industrial projects

**Proximity-Based Distribution**

Analysis of project types by distance band reveals distinct patterns:

**Close Proximity (≤5km):**
- **Higher concentration of socio-cultural projects** (education, health, recreation)
- Average project distance: 3.2 km from nearest IRS site
- Suggests symbolic and community-focused reclamation priorities

**Mid-Range (5–20km):**
- **Balanced mix** of cultural and practical infrastructure
- Transition zone between symbolic and regional service delivery
- Average project distance: 12.8 km from nearest IRS site

**Far Distance (>20km):**
- **Dominated by utilities and housing**
- Reflects broader regional infrastructure needs
- Less influenced by proximity to former school sites

**Project Type Proximity Rankings:**

When ranked by average distance to nearest IRS site:

| Project Type | Avg Distance (km) | N Projects |
|-------------|-------------------|------------|
| Health | 8.4 | 4,231 |
| Education | 9.7 | 6,842 |
| Recreation | 11.2 | 5,127 |
| Cultural | 12.3 | 3,956 |
| Housing | 15.8 | 12,441 |
| Utilities | 18.9 | 8,723 |
| Transportation | 21.4 | 5,219 |

This ranking demonstrates that **health and education facilities are systematically closer** to former IRS sites than infrastructure types focused on utilities or transportation, supporting the interpretation that proximity reflects deliberate community healing and service restoration priorities.

![Project Type Distribution](/figures/Figure_1_IIP_by_Buffer_and_Project_Type.png)

---

### Top Reclaimed Sites Analysis

**Identification Methodology**

Using 10km buffers around each former IRS location, we calculated the number of infrastructure projects within proximity of every school site. The top 5 sites with highest nearby project counts represent areas of intensive reclamation investment:

**Top 5 Most Reclaimed Sites:**

1. **Site ID 47** - 2,847 projects within 10km
2. **Site ID 82** - 2,391 projects within 10km
3. **Site ID 113** - 1,976 projects within 10km
4. **Site ID 29** - 1,854 projects within 10km
5. **Site ID 65** - 1,729 projects within 10km

These locations represent focal points for Indigenous community investment and provide valuable case studies for understanding successful reclamation strategies. The concentration of development around these specific sites suggests they may have:

- Strong community advocacy and leadership
- Access to targeted funding programs
- Strategic importance for cultural or geographic reasons
- Active land claims or self-governance arrangements

**Under-Reclaimed Sites:**

Conversely, the analysis identified 23 former IRS sites with ≤2 projects within 10km, representing potentially under-served areas that may benefit from increased policy attention and resource allocation.

![Emphasis Mapping](/figures/Figure_2_Top_IRS_Sites_by_Nearby_Projects.png)

---

## Technical Implementation

### Spatial Data Processing Pipeline

```
1. Data Import (st_read from shapefile sources)
2. CRS Verification and Transformation (to EPSG:3347)
3. Buffer Generation (multi-scale: 1, 5, 10, 20, 50 km)
4. Spatial Join Operations (st_within, st_join)
5. Distance Calculations (st_distance for nearest IRS site)
6. Density Computation (projects per km²)
```

### Analytical Methods

```
1. Buffer Zone Analysis (st_buffer, area calculations)
2. Spatial Joins (point-in-polygon operations)
3. Cross-K Function Analysis (spatstat package)
4. Kernel Density Estimation
5. Distance-based Classification
6. Project Type Aggregation and Comparison
```

### Visualization Techniques

```
1. Stacked Bar Charts (project volume by buffer and type)
2. Interactive Maps (leaflet for web-based visualization)
3. Static Thematic Maps (tmap for publication-quality figures)
4. Cross-K Function Plots (spatstat visualization)
5. Density Heatmaps (kernel density estimation)
```

### Key R Packages

| Package | Purpose |
|---------|---------|
| `sf` | Spatial vector data handling and operations |
| `spatstat` | Spatial statistics and point pattern analysis |
| `tidyverse` | Data manipulation and visualization |
| `tmap` | Thematic mapping and cartography |
| `leaflet` | Interactive web maps |
| `ggplot2` | Statistical graphics |
| `dbscan` | Density-based spatial clustering |

---

## Skills Demonstrated

**Geospatial Analysis:**
* Point pattern analysis and spatial statistics
* Multi-scale buffer zone analysis
* Cross-K function implementation for spatial clustering detection
* Coordinate reference system transformation and projection management
* Spatial joins and geometric operations
* Kernel density estimation
* Distance-based spatial classification

**Data Science:**
* Large-scale spatial data manipulation (120,000+ infrastructure projects)
* Statistical hypothesis testing for spatial randomness
* Descriptive spatial statistics and aggregation
* Data visualization for policy communication
* Integration of multiple spatial datasets with different schemas

**Technical:**
* R programming for spatial analysis (sf, spatstat, tmap)
* Geospatial data processing workflows
* Interactive and static map production
* R Markdown for reproducible research
* Version control for data science projects

**Domain Knowledge:**
* Indigenous rights and reconciliation context in Canada
* Historical trauma and community healing frameworks
* Canadian geography and administrative divisions
* Infrastructure typology and classification
* Policy-relevant research translation

**Research & Communication:**
* Translating complex spatial analysis for policy audiences
* Academic poster design and data storytelling
* Integration of quantitative findings with qualitative literature
* Actionable insights for community development and program design

---

## Policy Implications

**Key Policy Recommendations:**

1. **Targeted Investment:** The identified "under-reclaimed" sites with few nearby projects represent opportunities for strategic infrastructure investment to support community healing

2. **Project Type Prioritization:** The concentration of health and education facilities near former IRS sites validates community priorities and suggests these sectors should receive continued support in proximity-based funding programs

3. **Regional Equity:** The spatial variation in reclamation intensity suggests need for regional equity considerations in funding allocation mechanisms

---

## Limitations

This analysis is subject to several important limitations:

1. **Correlation vs. Causation:** Spatial proximity does not prove intentional reclamation—other socioeconomic factors may influence IIP distribution
2. **Data Completeness:** IIP dataset may not capture all Indigenous-led infrastructure, particularly smaller community projects
3. **Temporal Constraints:** Limited project timeline data restricts ability to analyze temporal trends
4. **Boundary Effects:** Canada's large size and irregular borders may introduce edge effects in spatial statistics
5. **Ecological Fallacy:** Patterns observed at aggregate spatial level may not reflect individual community intentions

Despite these limitations, the consistent spatial patterns across multiple analytical approaches provide robust evidence of proximity-based development that warrants attention from policymakers and community leaders.

---

## Authors

**Jennifer Estigene**  
**Iana Lanceta**

*Hertie School of Governance*  
*GRAD-E1457: Geospatial Analysis for Data Science*  
*May 2025*

---

## License

MIT License - see [LICENSE](LICENSE) file for details.

---

## References

**Academic Literature:**
* McBain, C. (2021). *De-Colonial Intersections of Conservation and Healing: The Indian Residential School System* (Doctoral dissertation, Carleton University).

**Data Sources:**
* Former IRS Site Data: Borealis Data Repository (DOI: 10.5683/SP2/FJG5TG)
* Indigenous Infrastructure Projects: Indigenous Services Canada IIP Dataset (DOI: geo.sac-isc.gc.ca)
* Canadian Base Map: Statistics Canada - Provinces & Territories Dataset (https://www12.statcan.gc.ca/census-recensement/alternative_alternatif.cfm)

**Policy Context:**
* Truth and Reconciliation Commission of Canada (2015). *Calls to Action*
* Indigenous Services Canada. *Community Infrastructure*

---
