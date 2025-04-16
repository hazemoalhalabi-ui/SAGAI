# Streetscape Analysis with Generative AI (SAGAI)

**SAGAI** is an open-source, modular workflow that uses generative vision-language models to automatically score and map street-level urban environments. It enables scalable, prompt-driven interpretation of Google Street View imagery using only a bounding box as input—requiring no pre-labeled data, specialized hardware, or deep learning expertise.

💡 **Zero-shot. Lightweight. Fully deployable.**

---

## 🌍 What does SAGAI do?

SAGAI links together:
- **OpenStreetMap** for automatic street geometry extraction  
- **Google Street View** for street-level imagery  
- **LLaVA (Mistral 7B)** in quantized format for visual scoring via customizable prompts  
- **GeoPandas / Matplotlib** for spatial aggregation and thematic mapping  

It can:
- Classify urban vs rural street scenes
- Count visible storefronts
- Estimate sidewalk width
- Be easily adapted to **any visual scoring task** by changing the natural language prompts

---

## 🧭 Workflow Overview

SAGAI is organized into four fully independent Python modules:

| Module | Function |
|--------|----------|
| **Module 1** | Generate points along the OSM street network in a bounding box |
| **Module 2** | Download Google Street View images at each point |
| **Module 3** | Use a lightweight LLaVA model to analyze the images via prompt |
| **Module 4** | Aggregate and map the scores at both point and street levels |

🖼️ ![Workflow Diagram](A_flowchart-style_digital_illustration_diagram_tit.png)

---

## 🚀 Quick Start

SAGAI runs in **Google Colab** with no installation required. Each module is a standalone Python script.

1. Clone the repo to your Google Drive:
   ```bash
   git clone https://github.com/your-org/sagai.git

2. Open the corresponding Colab notebooks for each module:
   - [Module 1: OSM Point Generator](link-to-colab-notebook-1)
   - [Module 2: Street View Batch Downloader](link-to-colab-notebook-2)
   - [Module 3: Image-Based Analysis with LLaVA](link-to-colab-notebook-3)
   - [Module 4: Aggregation & Mapping](link-to-colab-notebook-4)

3. Provide your Google Maps API key (for Module 2 only)

4. Select your task (`T1`, `T2`, or `T3`) or define a new prompt (for Module 3 only)

5. Run the full pipeline and generate thematic maps for your city/neighborhood

---

## 📦 Predefined Scoring Tasks

| Task ID | Type         | Description                                   |
|---------|--------------|-----------------------------------------------|
| `T1`    | Classification | Urban vs Rural Scene (0 = rural, 1 = urban)   |
| `T2`    | Counting      | Number of visible storefronts (0–1-2+)         |
| `T3`    | Measurement   | Sidewalk width in meters (e.g. 0, 1.5, 2.0)    |

All prompts are defined in `module_3_llava_inference.py` and can be fully customized.

---

## 📊 Example Outputs

Two pilot case studies are included:
- **Nice, France** – linear mixed-use corridor with dense housing
- **Vienna, Austria** – low-density peri-urban hills and gardens

SAGAI outputs:
- `.gpkg` files for analysis
- Color-coded maps for each scoring task

---

## 📚 Citation

If you use SAGAI in your research, please cite:

> Perez, J. and Fusco, G. (2025). *Streetscape Analysis with Generative Artificial Intelligence (SAGAI): A Modular Workflow for Vision-Language Scoring and Mapping of Street-Level Scenes.* [Preprint link or DOI]

---

## 🪪 License

SAGAI is released under the **Apache 2.0 License**, ensuring compatibility with its core dependencies, including LLaVA, OpenStreetMap, and Google Street View.

---

## ✨ Acknowledgments

Developed as part of the EMC2 project under the **Driving Urban Transition Partnership**, co-funded by ANR (France), FFG (Austria), MUR (Italy), and Vinnova (Sweden).

---

## 📫 Feedback and Contributions

Feel free to open an issue or pull request. Contributions and forks are welcome!

🔗 [GitHub Discussions](https://github.com/perezjoan/SAGAI/discussions) – Share use cases, ideas, and extensions.

