# i-Tree Canopy Reliability Framework

Reproducible analysis package accompanying the manuscript:

> Yakar, F. *Reliability of i-Tree Canopy in Data-Scarce Regions: A Proxy-Sensitivity and Sampling-Precision Framework.* iForest – Biogeosciences and Forestry (under review, ms25/5061).

This repository contains three complementary analyses supporting the methodological reliability of i-Tree Canopy applications in data-scarce regions:

1. **Quantitative climate-similarity framework** for proxy-region selection (addresses qualitative proxy selection in published applications).
2. **Independent Sentinel-2 remote-sensing validation** of the i-Tree canopy cover estimate, including point-level validation against the original 20,255 i-Tree sampling locations.
3. **Point-level structural correlation analysis** among ecosystem-service proxies (n = 20,023), addressing the limited statistical depth of mean-based correlation analyses.

## Repository structure

```
.
├── climate_similarity.py            Climate similarity main analysis
├── plot_pca.py                      PCA visualization of bioclimatic space
├── plot_ranking.py                  Bar chart of similarity ranking
├── requirements.txt                 Python dependencies
├── outputs/                         Climate similarity CSV results
│   ├── bioclim_raw.csv
│   ├── bioclim_zscore.csv
│   └── similarity_ranking.csv
├── figures/                         All manuscript figures
│   ├── fig2_pca_climate_similarity.png/.pdf
│   ├── fig3_similarity_ranking.png
│   ├── fig4_sentinel2_validation.png/.pdf
│   ├── fig5_service_correlations.png/.pdf
│   └── fig6_correlation_heatmap.png/.pdf
├── validation_scripts/              Sentinel-2 validation analysis (R1)
│   ├── sentinel2_validation.js      GEE: validation against i-Tree
│   ├── point_level_proxies.js       GEE: extract NDVI/FCover/SAVI/BSI/NDWI at points
│   ├── point_level_evi.js           GEE: extract EVI at points
│   ├── parse_kml.py                 Extract sampling points from i-Tree KML
│   ├── derive_polygon.py            Reconstruct study boundary via alpha shape
│   ├── fix_polygon.py               Refine polygon to exclude lake interior
│   └── make_validation_figure.py    Validation panel figure
├── validation_data/                 Spatial data
│   ├── itree_points.csv             20,255 i-Tree sampling locations
│   ├── eymir_itree_polygon.geojson
│   └── itree_polygon_with_lake_hole.geojson
└── correlation_analysis/            Point-level correlation analysis (R3)
    ├── correlation_final.py         Main analysis with bootstrap CIs
    ├── Eymir_point_level_proxies.csv  GEE-extracted multi-proxy data
    ├── Eymir_point_EVI.csv            GEE-extracted EVI data
    └── service_correlation_bootstrap.csv  Final correlation results
```

## Part 1: Climate similarity framework

Quantitative proxy-region selection based on five bioclimatic indicators (mean annual temperature, annual precipitation, temperature seasonality, precipitation seasonality, De Martonne aridity index), z-score standardized across nine candidate cities, ranked by Euclidean distance from Ankara.

```bash
pip install -r requirements.txt
python climate_similarity.py
python plot_pca.py
python plot_ranking.py
```

## Part 2: Sentinel-2 remote-sensing validation

Independent cross-validation of the i-Tree Tree/Shrub canopy estimate using Sentinel-2 spring composite (April-June, 2023-2025), with three complementary methods:

- Polygon-level binary NDVI thresholding
- Sub-pixel fractional cover (FCover) by NDVI rescaling
- Point-level NDVI extraction at the original 20,034 sampled i-Tree locations

Cross-checks against ESA WorldCover v200 (2021) and Dynamic World (2023-2025).

### Reproduce
1. `validation_scripts/parse_kml.py` extracts sampling points from the i-Tree KML export.
2. `validation_scripts/derive_polygon.py` reconstructs the study boundary via alpha shape.
3. `validation_scripts/fix_polygon.py` refines the boundary to exclude lake interior.
4. Upload `validation_data/itree_points.csv` to Google Earth Engine as a FeatureCollection asset.
5. Run `validation_scripts/sentinel2_validation.js` in the Earth Engine Code Editor (update POINTS_ASSET_PATH).
6. Export NDVI, FCover, and RGB rasters to Google Drive.
7. Run `validation_scripts/make_validation_figure.py` locally to generate the panel figure.

## Part 3: Point-level structural correlation analysis

Pairwise correlations among Sentinel-2-derived ecosystem-service proxies at the 20,023 i-Tree sampling locations, with bootstrap 95% confidence intervals.

Service proxies:
- **Carbon biomass / sequestration**: Enhanced Vegetation Index (EVI)
- **Air-pollution interception**: sub-pixel canopy fraction (FCover)
- **Runoff buffering capacity**: 1 - Bare Soil Index (1 - BSI)

EVI is methodologically independent from NDVI/FCover, avoiding circular reasoning.

### Reproduce
1. Run `validation_scripts/point_level_proxies.js` in Google Earth Engine.
2. Run `validation_scripts/point_level_evi.js` for the EVI proxy.
3. Download both CSVs from Google Drive.
4. Run `correlation_analysis/correlation_final.py` locally.

## Data sources

- WorldClim v2.1 (Fick & Hijmans 2017): monthly climate normals (1991-2020)
- Sentinel-2 Surface Reflectance (Copernicus / ESA): COPERNICUS/S2_SR_HARMONIZED on Google Earth Engine
- ESA WorldCover v200 (Zanaga et al. 2022): 10 m global land cover (2021)
- Dynamic World (Brown et al. 2022): near-real-time global 10 m land cover
- i-Tree Canopy v8.0: random point sampling tool (USDA Forest Service)

## Citation

If you use this framework or data, please cite the manuscript above and this repository.

## License

MIT License. See LICENSE.

## Contact

Funda Yakar
Earth System Science, Middle East Technical University, Ankara, Turkiye
fundayakar@gmail.com
ORCID: 0000-0002-7082-3956
