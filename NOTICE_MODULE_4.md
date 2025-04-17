# GEOSPATIAL SCORING AGGREGATION AND MAPPING
**Version:** 1.0   
**Project Github:** https://github.com/perezjoan/SAGAI

---

## 1. Description

For each sampling location, the script groups the four directional image scores (e.g., from 0°, 90°, 180°, and 270°) and computes four statistical metrics: mean, sum, median, and variance. You only need to define a case study name (e.g., nice, vienna) and recall the selected task of Module 3 (T1, T2, or T3). All paths will be generated automatically. All four summary statistics are retained in the output GeoPackage for both points and streets. The user also has to choose between two aggregation modes for the automated maps: 'average' or 'sum'.

---

## 2. Key Features

### Google Drive Integration
- Mounts Google Drive inside Google Colab to load GeoPackages and scores (.csv).

### Point- and Street-Level Merging
- Reads point and street layers from a GeoPackage (osm.gpkg).
- Reads a single CSV containing all image-based scores.
- Extracts base point IDs (e.g., point_12) and averages scores over headings.
- Each point gets an `overall_score` from its valid images (1 to 4 images max).
- All points linked to a given `street_id` are averaged to compute a `street_average_score`.
- Four statistical metrics are calculated: mean, sum, median, and variance.

### Handling of Missing
- If an image has no score (e.g., marked as NA in the .csv file), it is skipped in the averaging.
- If all views are NA for a point, the result will be left empty (NaN) and excluded from further aggregation.
- Points and streets with no valid score remain NaN and are displayed in light grey in the maps.

### Mapping and Visualization
- Two maps are automatically generated:  
  - 'average' mode: point average and street average  
  - 'sum' mode: point totals and street totals

### Export to GeoPackage
- The processed point and street layers are written back to a new GeoPackage `{case}_osm_joined_{task}.gpkg`, preserving the structure and CRS of the original data.

---

## 3. How to Use

Open the script in Google Colab and mount your Google Drive.  
Set your case study name and task version at the top of the script—for example:  
```python
case_study_name = "vienna"
task_version = "T3"
```

This will automatically generate all input and output paths.  
Ensure that your input CSV (e.g., `Score_Analysis_LLaVA_Vienna_T3.csv`) and the base GeoPackage (e.g., `vienna_osm.gpkg`) exist in the expected locations.  
Then set the desired aggregation mode:  
```python
aggregation_mode = "average"  # or "sum"
```

The script will process all valid scores, perform spatial joins, calculate the chosen statistics at point and street level, visualize the results, and save the updated GeoPackage under a new name (e.g., `vienna_osm_joined_T3.gpkg`).

---

## 4. Additional Information

This script is designed to work with the output of the companion **IMAGE-BASED SCORING FRAMEWORK WITH LLaVA**, which produces a .csv file containing scores for every image analyzed, and with **OSM POINT GENERATOR**, which downloads, cleans, and generates spatial points along OSM street networks.

Each score must be a numeric value or `"NA"` (case-insensitive).  
The script extracts the base point ID from each image filename (e.g., `point_12_90.jpg` → `point_12`), then computes the sum and the average of all valid (non-NA) scores for that point.

- The sum and the average are assigned to the corresponding point geometry.
- At the street level, the script calculates the sum and the average of all point scores associated with each `street_id`.

If a point has only one valid score, that score is used directly; if it has no valid scores, it is excluded from the computation entirely.

While designed to integrate seamlessly with the other modules in this framework, the script can also be used independently—provided that the input CSV and GeoPackage follow the expected naming and schema conventions.
