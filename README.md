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
