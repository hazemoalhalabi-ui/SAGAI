# OSM POINT GENERATOR  
**Version**: 1.0.0  
**Project GitHub**: [https://github.com/perezjoan/SAGAI](https://github.com/perezjoan/SAGAI)

---

## 1. Description

This script automates the process of downloading and processing OpenStreetMap (OSM) street data within a user-defined geographic bounding box. It begins by fetching street geometries from OSM and proceeds to clean and filter the data to ensure consistency and remove duplicates. Once the streets are processed, the script generates sample points along each street segment. If a segment is shorter than the specified offset, no point is placed. For segments between the offset and offset plus the spacing distance, one point is placed at the center.

Each street and point is assigned a unique identifier to maintain traceability. Additionally, point coordinates are converted to latitude and longitude using the WGS84 coordinate system for compatibility with global mapping tools. The final results—including both the cleaned streets and the generated points—are saved as separate layers within a GeoPackage file, allowing for easy reuse in GIS workflows. The user can fully control the spacing and offset parameters, making the script adaptable to various urban analysis or mapping tasks.

---

## 2. Key Features

### Google Drive Integration
- Mounts Google Drive directly in the Colab environment.
- Enables reading and writing files persistently, including saving the final GeoPackage output.

### Bounding Box Selection
- User defines a geographic area of interest using coordinates (`xMin`, `xMax`, `yMin`, `yMax`).
- Street data within this area is automatically fetched using the `osmnx` library.

### Data Cleaning and Processing
- Converts list-type attributes into strings to prevent export errors.
- Removes duplicate street segments based on a combination of `osmid` and length.
- Retains only essential attributes.
- Assigns a unique street ID (e.g., `street_1`, `street_2`, etc.) for traceability.

### Point Generation Along Streets
- User specifies spacing (`point_distance`) and offset (`offset_distance`) for point placement.
- Points are placed:
  - Not at all if segment < offset
  - At center if offset ≤ segment < offset + spacing
  - At regular intervals if segment ≥ offset + spacing
- Each point receives a unique `point_id` and is linked to its corresponding `street_id`.

### Coordinate System Conversion
- Reprojects geometries to WGS84 (EPSG:4326) for global latitude/longitude compatibility.
- Adds `Latitude` and `Longitude` fields for use in web maps and geolocation APIs.

### Output Saving
- Saves streets and points into a single GeoPackage (`.gpkg`) with two layers:
  - One for cleaned streets
  - One for sampled points

### Visualization
- Plots the final street network, bounding box, and generated points.
- Displays statistics (e.g., total street length, number of points).
- Enables visual verification before saving.

### Error Handling
- Detects and manages common file writing issues.
- Automatically creates missing directories and removes corrupted files to avoid crashes.

---

## 3. How to Use

1. Run the script in **Google Colab**.
2. Install required library:
   ```python
   !pip install osmnx
   ```
3. Mount Google Drive:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
4. Specify:
   - **Case study name** (e.g. `"nice"`)
   - **Bounding box** (`xMin`, `xMax`, `yMin`, `yMax`)
   - **Projected CRS** for distance calculations
   - **Spacing** and **offset** for point generation
5. Script saves output to:
   ```
   /MyDrive/SAGAI/StreetSamples/{case_study}_osm.gpkg
   ```

---

## 4. Additional Information

The script begins with the download of street data from OpenStreetMap (OSM) using the `osmnx` library. A rectangular polygon is created from the bounding box coordinates using `shapely` and passed to `ox.graph_from_polygon()`. The resulting graph is converted into GeoDataFrames (`nodes`, `edges`), and only `edges` are retained.

The raw data is cleaned:
- List attributes are converted to strings.
- Geometries are reprojected from WGS84 to a projected CRS (e.g., EPSG:2154).
- Street lengths are computed and stored in `length_m`.
- Duplicates are removed based on `osmid` and rounded length.

Each street is assigned a unique `street_id`. A custom function places points based on segment length and offset logic. Each point is given a `point_id` and linked to its street.

Points are reprojected to WGS84, and `Latitude`/`Longitude` columns are added. The final output is saved as a GeoPackage with two layers, compatible with major GIS platforms.

---
