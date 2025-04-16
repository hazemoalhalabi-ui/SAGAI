
# IMAGE-BASED SCORING FRAMEWORK WITH LLaVA

**Version**: 1.0.0  
**Project Github**: [https://github.com/perezjoan/SAGAI](https://github.com/perezjoan/SAGAI)

## 1. Description

This script enables batch scoring of street-level images using the LLaVA (Large Language and Vision Assistant) model within a Google Colab environment. It combines two core steps: first, the LLaVA source code is cloned from GitHub to configure the model structure; second, pretrained vision-language weights—such as llava-v1.6-mistral-7b—are downloaded from Hugging Face to enable inference. Unlike standard captioning or reasoning workflows, this script is designed to perform fast, lightweight numeric scoring of images based on a custom prompt. Three predefined AI behaviors are included: Categorization (e.g., rural vs. urban), Counting (e.g., number of visible shops), and Measuring (e.g., estimated sidewalk width in meters). Only one behavior is active at a time, determined by setting a selected_task variable (T1, T2, or T3). Users are encouraged to write their own scoring logic and prompts by modifying the prompt templates provided in the script. The results are saved into a spreadsheet-style .csv. A resuming mechanism ensures that previously processed images are skipped automatically in case the session is interrupted.

## 2. Key Features

### Google Drive Integration
- Automatically mounts Google Drive to access your image folder and save outputs.
- Works with `.jpg`, `.jpeg`, and `.png` files.
- Skips any image ending in `_NA.jpg` (e.g., from STREET VIEW BATCH DOWNLOADER).

### LLaVA Model Setup
- Uses Hugging Face's `snapshot_download` to load pretrained weights (default: `llava-v1.6-mistral-7b`).
- Runs with 4-bit quantization for memory-efficient inference in Colab GPU environments.
- Supports easy model switching by editing the `repo_id` and `model_path`.

### Numerical Scoring Pipeline
- Ideal for tasks that require simple scalar outputs per image (e.g., 0 = no shop, 1 = one shop, 2 = multiple shops).
- You define the scoring rules in natural language. Prompts are editable and fully customizable for different use cases.
- Output is written as a 2-column CSV: image name and score.

### Pre-coded AI Scoring Behaviors
- **Categorization (T1)**: Classifies each image as rural (0) or urban (1).
- **Counting (T2)**: Detects presence of storefronts — 0 (none), 1 (one), or 2 (multiple).
- **Measuring (T3)**: Estimates visible sidewalk width in meters, rounded to the nearest 0.5 (e.g., 2.0, 2.5).
- Selection is made via the `selected_task` parameter. Prompts are editable and fully customizable.

### Resumable Execution
- If the output CSV already exists and is not empty, the script resumes from the last processed image.
- Images already present in the CSV will be skipped.
- If the output CSV does not exist or is empty, the script starts fresh.

### Colab-Ready and Modular
- No local setup needed—the entire pipeline runs from Google Colab.
- Outputs can feed directly into geospatial or statistical analysis scripts.

## 3. How to Use

Open the notebook in Google Colab and ensure you’ve selected a GPU runtime (`Runtime → Change runtime type → Hardware accelerator → GPU`). Mount your Drive using `drive.mount('/content/drive')`, then define:  

- `case_study`: a short name for the project (e.g., `"vienna"` or `"nice"`).  
- `selected_task`: choose from `"T1"`, `"T2"`, or `"T3"` to select the desired AI behavior.  

Output paths and folders will be created automatically based on the case name. In the AI Settings section, one of three pre-coded prompts will be activated based on the `selected_task` value. You can customize any of them or insert your own logic for different visual assessments. The model will loop over all valid images, apply the scoring rubric, and write results to the `.csv` file.

## 4. Additional Information

The script loads a vision-language model from Hugging Face using `snapshot_download`. You can change the model by editing the `repo_id` (e.g., to `llava-hf/llava-1.5-7b-hf`) and updating `model_path` accordingly. If you're using a private model, make sure to log in via `huggingface_hub.login(token=...)` before downloading. Public models require no authentication.

The model is loaded with 4-bit quantization via `BitsAndBytesConfig`, keeping memory usage under 6 GB on most GPUs. You can switch to 8-bit loading by replacing `load_in_4bit=True` with `load_in_8bit=True`, and removing the `bnb_4bit_*` arguments.

If your images were generated using the companion script STREET VIEW BATCH DOWNLOADER, the script will automatically ignore any images with `_NA` in the filename—these indicate locations where Google Street View returned no visual data.

This scoring pipeline is designed as part of a larger visual analysis framework. It generates compact structured outputs that can be used in **Script 4: Geospatial Scoring Aggregation and Mapping**, allowing fully automated visual-to-map workflows.
