# Georgia Tick Ecology & Distribution Study

[![DOI](https://img.shields.io/badge/DOI-pending-blue.svg)](https://github.com/IanPsheaSmith/GeorgiaTickDist)

## Table of Contents
- [Overview](#overview)
- [Species Studied](#species-studied)
- [Repository Structure](#repository-structure)
- [Data Files](#data-files)
- [Analyses](#analyses)
- [Usage](#usage)
- [System Requirements](#system-requirements)
- [Citation](#citation)
- [License](#license)
- [Contact](#contact)

## Overview

This repository contains all data, code, and associated files for a community ecology and spatial distribution study of ticks in the country of Georgia (Caucasus). The study draws on ten years of surveillance data (2013–2023) collected across 580 unique sites to describe tick species diversity, biogeography, co-occurrence patterns, and habitat suitability using boosted regression tree (BRT) models.

**Key Features:**
- Tick occurrence dataset spanning 216 unique collection dates across 10 years
- Complete R Markdown code for fully reproducible analysis
- Biodiversity metrics, bioregionalization, and species co-occurrence network analyses
- BRT-based species distribution models for seven medically-important tick species
- Covariate raster stacks covering environmental, climatic, and soil variables across Georgia

## Species Studied

### All Identified Species (21 total across 5 genera)

| Genus | Species |
|-------|---------|
| *Dermacentor* | *D. reticulatus*, *D. marginatus* |
| *Haemaphysalis* | *Ha. parva*, *Ha. punctata*, *Ha. inermis*, and others |
| *Hyalomma* | Multiple species |
| *Ixodes* | *I. ricinus*, and others |
| *Rhipicephalus* | *Rh. sanguineus*, and others |

### Distribution-Modeled Species (≥30 presence records)

Seven species met the minimum occurrence threshold for boosted regression tree modeling:
*Dermacentor reticulatus*, *D. marginatus*, *Haemaphysalis parva*, *Ha. punctata*, *Ha. inermis*, *Ixodes ricinus*, and one additional *Rhipicephalus* species.

## Repository Structure

```
GeorgiaTickDist/
├── Georgia_Ticks_Biodiv.Rmd        # Complete analysis R Markdown document
├── TicksEurope_Clean.xlsx          # Tick occurrence dataset
├── BRT_PseudoAbs_Datasets/         # Species datasets with pseudo-absences for BRT modeling
├── BRT_RealAbs_Datasets/           # Species datasets with real absences for BRT modeling
├── CovariateRasters/               # Environmental and climatic raster stacks
└── soilData/
    └── Cropped_Soil/               # Soil variable rasters cropped to Georgia extent
```

## Data Files

### Tick Occurrence Dataset
**File:** `TicksEurope_Clean.xlsx`

The primary occurrence dataset containing tick collection records from Georgia (2013–2023):
- `Date`: Collection date (YYYY-MM-DD)
- `Latitude` / `Longitude`: Collection site coordinates (WGS84)
- `Species`: Tick species identification
- `Count`: Number of individuals collected
- `Site`: Collection site identifier
- Collection method metadata (flagging, dragging, dry ice traps)

### BRT Modeling Datasets
**Location:** `BRT_PseudoAbs_Datasets/` and `BRT_RealAbs_Datasets/`

Species-specific presence/absence datasets prepared for boosted regression tree modeling. Two versions are provided:
- **Pseudo-absence datasets** (`BRT_PseudoAbs_Datasets/`): Background pseudo-absence points randomly sampled proportional to sampling effort, following Barbet-Massin et al. (2012)
- **Real-absence datasets** (`BRT_RealAbs_Datasets/`): True absence records derived from collection events where a given species was not detected

Both dataset types include matched environmental covariates extracted at each occurrence and absence point.

### Covariate Rasters
**Location:** `CovariateRasters/`

Multi-band GeoTIFF raster stacks covering the national extent of Georgia with all environmental predictors used in the distribution models. Covariates include:

- Temperature variables (mean annual, seasonality; WorldClim v2)
- Precipitation variables (mean annual, seasonality; WorldClim v2)
- NDVI (Normalized Difference Vegetation Index; MODIS MOD13A3)
- Elevation (digital elevation model)
- Land cover classes

**Raster Specifications:**
- Coordinate system: WGS84
- Spatial resolution: ~1 km
- Extent: Georgia national boundaries

### Soil Data
**Location:** `soilData/Cropped_Soil/`

Soil property rasters sourced from SoilGrids 2.0 (Poggio et al., 2021), cropped to the Georgia study extent. Soil variables include texture, organic carbon, pH, and bulk density, which were incorporated as predictors given the importance of soil microhabitat for tick survival and questing behavior.

## Analyses

All analyses are implemented in `Georgia_Ticks_Biodiv.Rmd` and are fully reproducible. The workflow includes four primary analytical components:

**1. Biodiversity Metrics**
Species richness and community diversity were quantified using Shannon's Diversity Index, Gini-Simpson Diversity, Pielou's Evenness, Simpson's Dominance, and Berger-Parker Dominance. Whittaker rank-abundance curves were generated alongside rarefaction analyses using the Chao1 and first-order Jackknife estimators (via the `iNEXT` package) to assess sampling completeness.

**2. Bioregionalization**
Occurrence points were aggregated into 25 cluster-defined sites using k-means clustering (elbow plot optimized). Spatially constrained hierarchical clustering was then implemented using the `ClustGeo` package (Chavent et al., 2021), integrating both Jaccard dissimilarity and haversine-based spatial distance matrices to define discrete tick bioregions. Bioregion differences were evaluated using ANOVA across environmental covariates, and indicator species were identified using IndVal analysis (Dufrêne & Legendre, 1997; 9,999 permutations).

**3. Species Co-occurrence Networks**
Two co-occurrence network types were constructed from the site-species matrix: a probabilistic co-occurrence network (Veech, 2013; `cooccur` package) and a simple count-based co-occurrence network. Niche overlap between modeled species was quantified using Schoener's D and Warren's I statistics.

**4. Species Distribution Models**
Boosted regression trees (BRTs) were fit for all seven species with ≥30 presence records using the `dismo` and `gbm` packages. Model parameters: 5,000 trees, interaction depth = 10, learning rate = 0.001, with 10-fold cross-validation. Each species dataset was partitioned into training (75%) and testing (25%) sets. Model performance was evaluated using RMSE and AUC. Partial dependence plots were generated for all covariates to characterize species-environment relationships, and spatial predictions of habitat suitability were projected across Georgia.

## Usage

### Cloning the Repository

```bash
git clone https://github.com/IanPsheaSmith/GeorgiaTickDist.git
cd GeorgiaTickDist
```

### Reproducing the Analysis

1. Open `Georgia_Ticks_Biodiv.Rmd` in RStudio
2. Install required R packages (listed in the Rmd header)
3. Ensure all data files are in the expected relative paths (no changes needed if cloned from GitHub)
4. Knit the document to reproduce all analyses, figures, and model outputs

## System Requirements

- **R version:** 4.5.1 or later (R Core Team, 2023)
- **RStudio:** Recommended for Rmd execution
- **OS:** Platform-independent (tested on Windows and Linux)
- **Memory:** ≥8 GB RAM recommended for raster processing and BRT model fitting
- **Disk space:** ~2 GB for full raster stack storage

## Citation

If you use this data or code in your research, please cite (under review, not final citation):

Pshea-Smith, I.A., Kotorashvili, A., Kotaria, N., Golubiani, G., Kirkitadze, G., Chunashvili, T., Shubashishvili, A., Di Paola, N., Kugelman, J., & Hulseberg, C. (in review). An ecological analysis of ticks in the Country of Georgia using ten years of surveillance data. *[Journal pending]*. DOI: pending

**Data Usage:** All data are freely available for use with proper attribution.

## Contact

Ian Pshea-Smith
University of Florida — Department of Geography, Spatial Ecology and Epidemiology Research Laboratory
Email: [ianpsheasmith@ufl.edu](mailto:ianpsheasmith@ufl.edu)

**Issues and Questions:**
Please use the [GitHub Issues](https://github.com/IanPsheaSmith/GeorgiaTickDist/issues) page for data questions, code issues, or requests for clarification.

## Acknowledgments

### Funding
This study was financially supported by the Armed Forces Health Surveillance Division's Global Emerging Infections Surveillance (AFHSD GEIS) awards P0020_23_GA, P0032_24_GA, and P0056_25.

### Disclaimer
The views expressed here are those of the authors and do not reflect the official policy of the Department of the Army, Department of Defense, or the US Government. This work was prepared in part by US government employees; as such, it may not be subject to copyright under 17 U.S.C. § 105. Material has been reviewed by the authors' respective institutions with no objection to its presentation and/or publication.

---

**Last Updated:** February 2026
**Repository Status:** Active Development
**Manuscript Status:** In Review
