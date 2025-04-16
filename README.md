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

SAGAI is organized into four Python modules:
![SAGAI Diagram](https://github.com/perezjoan/SAGAI/blob/images/sagai%20diagram.png)

| Module | Function |
|--------|----------|
| **Module 1** | Generate points along the OSM street network in a bounding box |
| **Module 2** | Download Google Street View images at each point |
| **Module 3** | Use a lightweight LLaVA model to analyze the images via prompt |
| **Module 4** | Aggregate and map the scores at both point and street levels |

---

## 🚀 Quick Start

SAGAI runs in **Google Colab**, with no installation required. Each module is a standalone Python script.

1. **Clone the repo** to your Google Drive:
   ```bash
   git clone https://github.com/perezjoan/SAGAI.git
   ```

2. **Open the Colab notebooks** for each module:

   - [Module 1: OSM Point Generator](https://colab.research.google.com/drive/your-notebook-id-1)  
     Input: 📍 bounding box in WGS84 (latitude/longitude)

   - [Module 2: Street View Batch Downloader](https://colab.research.google.com/drive/your-notebook-id-2)  
     Input: 🔑 Google Maps API key & 🌍 geographic data from Module 1

   - [Module 3: Image-Based Analysis with LLaVA](https://colab.research.google.com/drive/your-notebook-id-3)  
     Input:  🧠 select a scoring task (`T1`, `T2`, `T3`) or write your own prompt & 🖼️ images downloaded from Module 2

   - [Module 4: Aggregation & Mapping](https://colab.research.google.com/drive/your-notebook-id-4)
     Input: 🌍 geographic data from Module 1 and 📃 scores from Module 3
3. **Run the full pipeline** to generate visual scores and thematic maps for your city or neighborhood.

Each module is independent and can be customized or reused for different spatial tasks.


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

A pilot case study is included in the modules:
- **Nice, France** – A linear urban corridor following the Paillon Valley, characterized by mixed land uses and dense social housing developments in the northern sector.

SAGAI outputs:
- `.gpkg` files for analysis
- Color-coded maps for each scoring task

---

## 📚 Citation

If you use SAGAI in your research, please cite:

> Perez, J. and Fusco, G. (2025). *Streetscape Analysis with Generative Artificial Intelligence (SAGAI): A Modular Workflow for Vision-Language Scoring and Mapping of Street-Level Scenes.* [Preprint link or DOI]

---

## 🪪 License and Attribution

SAGAI is released under the [Apache License 2.0](LICENSE). This allows for use, modification, and redistribution in academic, commercial, and open-source contexts.

Please note the following third-party components:

- **LLaVA v1.6**, **CLIP**, and **Transformers** are used under their respective open-source licenses (MIT / Apache 2.0).
- **OpenStreetMap** data is included under the **ODbL 1.0 license**.
- **Google Street View** imagery is accessed via API and remains subject to Google’s **Terms of Service**.

If you build upon this codebase, please retain the included [NOTICE](NOTICE) file.

---

## ✨ Acknowledgments

This project was supported in part by the [EMC2 project](https://emc2-dut.org/) under the **Driving Urban Transition Partnership**, co-funded by **ANR (France)**, **FFG (Austria)**, **MUR (Italy)**, and **Vinnova (Sweden)**. SAGAI is designed as an open and extensible tool to support future research initiatives and collaborations in urban analytics and AI-based spatial analysis.

---

## 📫 Feedback and Contributions

Feel free to open an issue or pull request. Contributions and forks are welcome!

🔗 [GitHub Discussions](https://github.com/perezjoan/SAGAI/discussions) – Share use cases, ideas, and extensions.

