# STREET VIEW BATCH DOWNLOADER

**Version:** 1.0  
**Project GitHub:** [https://github.com/perezjoan/SAGAI](https://github.com/perezjoan/SAGAI)

## 1. Description
This script automates the batch download of Google Street View images based on a set of geographic points stored in a GeoPackage file. Each point is expected to contain latitude and longitude coordinates and a unique identifier. The script connects to the Google Street View Static API, queries it for each point using four cardinal directions (0°, 90°, 180°, 270°), and saves the resulting images. It also checks whether imagery is available at each location. If not, it appends a `_NA` suffix to the image filename to indicate missing content. Users can customize camera parameters such as pitch and field of view to adjust the perspective of the images. All images are saved into an automatically generated folder structure in Google Drive. The user only needs to define a case study name (e.g., "nice"), and the script will create a structured folder `/My Drive/SAGAI/StreetViewBatchDownload_<CaseName>` for image output. This makes the workflow compatible with Google Colab and suitable for large-scale urban observation, dataset creation, or automated visual surveys.

## 2. Key Features

### Google Drive Integration
- Mounts Google Drive within the Colab environment.
- Enables persistent storage of downloaded images directly into the user’s Drive.

### Batch Image Download
- Reads coordinates from a GeoPackage file and downloads Street View images for each point.
- Images are retrieved in four directions (0°, 90°, 180°, 270°) per point.
- Each image is named with the point ID and heading (e.g., `point_10_90.jpg`).

### Missing Imagery Detection
- Images with no available Google Street View imagery are detected using a color dominance heuristic.
- These are saved with a `_NA` suffix (e.g., `point_10_90_NA.jpg`) for easier post-processing.

### Customizable Camera Parameters
- Users can define the pitch (vertical angle), field of view (FOV), and headings.
- Flexible control over the angle and zoom of the Street View camera.

### Error Handling
- Checks for required columns (`Latitude`, `Longitude`) in the GeoPackage file.
- Prints status updates for each successful or failed download.

### Flexible Output Management
- Automatically creates and saves all images to `/My Drive/SAGAI/StreetViewBatchDownload_<CaseName>`.
- Organizes outputs with consistent filenames for easy indexing and reuse in ML or GIS workflows.

## 3. How to Use
1. Open a Google Colab notebook and copy-paste the code.  
2. Mount your Google Drive using `drive.mount('/content/drive')`.  
3. Set the required input parameters: your Google API key and the case study name (e.g., `"nice"`).  
4. The script builds paths automatically to read points from `SAGAI/StreetSamples/<case>_osm.gpkg` and saves images to `SAGAI/StreetViewBatchDownload_<case>`.
5. Optionally, adjust pitch and FOV to control how images are captured.
6. Run the script — it will download images, detect missing ones, and organize all files in your Drive.

## 4. Additional Information
You must have an active **Google API key** with access to the Street View Static API.

### To obtain a key:
- Go to Google Cloud Console → Create/select a project.
- Go to APIs & Services → Library → Enable “Street View Static API”.
- Go to APIs & Services → Credentials → “+ CREATE CREDENTIALS” → API key.
- Paste the key into the `API_KEY` variable in the script.

For security:
- Restrict the key to specific referrers or IPs.
- Limit it to only the Street View API.
- Enable billing (required even for free-tier use).

### How it works:
- Reads the `points` layer from the specified GeoPackage.
- Each point must have `Latitude`, `Longitude`, and `point_id`.
- For each point, sends four API requests for cardinal directions.
- Detects blank imagery by checking pixel color dominance (>90% identical).
- Saves valid images and marks blanks with `_NA` in filenames.

### Checking usage & quota:
- Go to Google Cloud Console → APIs & Services → Dashboard → Street View Static API.
- Click “Quotas” to view usage (default is 25,000 requests/day).
- Enable alerts under Monitoring → Alerting.
