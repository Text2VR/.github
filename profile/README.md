# Text2VR Organization

From a single text prompt to an interactive VR scene.

Text2VR is a research-driven system that turns natural language descriptions into VR-ready 3D environments and assets, combining panorama generation, asset extraction, 3D reconstruction, and VR deployment in a single pipeline.

---

## Overview

The organization currently maintains a **single main repository**:

- **Repository**: `Text2VR/Text2VR`  
  – LangGraph-based backend pipeline (`app/`, microservice clients, workflows)  
  – React + TypeScript frontend (`src/`)  
  – Service directories for panorama, segmentation, inpainting, and 3D generation  
  – Docker Compose configuration and pretrained checkpoint folders

High-level system architecture (see the main repo for full details):

![Text2VR System Architecture](https://raw.githubusercontent.com/Text2VR/Text2VR/main/docs/architecture.png)

---

## Research Context

Text2VR is developed in the context of HCI and game/VR research:

<!--- **HCI Korea 2026 (submitted)**  
  *“Text2VR: A Modular Pipeline for Interactive VR Scenes from a Single Text Prompt”*  
  – Focuses on a prompt-to-VR authoring workflow and a preliminary user study on perceived ease of use, immersion, time acceptability, and visual quality.
!-->
- Submitted to *Journal of Korea Computer Game Society (JKCGS)*
- **Text2VR: A Modular Pipeline for Interactive VR Scenes from a Single Text Prompt** <br/>
  ***CM. Lee**\*, S. Myoung\*, M. An\*, Y. Jin\*, EC. Lee†.* <br/>
  **Note:** (*: Co-first Author, †: Corresponding Author) <br/>
  - Extended journal version emphasizing VR content authoring workflows, automatic asset/background generation, and interactive VR scene construction using Text2VR.

The code in this organization corresponds to these works and is intended to be reproducible and extensible for further research.

---

## What Text2VR Does

At a high level, the Text2VR pipeline:

1. **Rewrites user prompts** into optimized generation queries (LLM-based query rewrite via LangGraph).
2. **Generates a 360° panorama** from the prompt using DreamScene360 (Stable Diffusion + Stitch Diffusion), with optional self-refinement.
3. **Segments interactive objects** using GPT-4o + GroundingDINO + SAM and crops them as RGBA assets.
4. **Converts 2D crops into 3D assets** with TRELLIS, producing GLB models.
5. **Inpaints the background** after object removal using Stable Diffusion 2 Inpaint (wrap-aware).
6. **Trains a 3D Gaussian Splatting background** from the inpainted panorama (PLY) for immersive VR rendering.
7. **Packages everything for VR**, so scenes can be explored in a VR engine (e.g., Unity with a Gaussian Splatting plugin and colliders/interaction scripts).

A more detailed pipeline overview is available in the `Text2VR` repository:

![Pipeline Overview](https://raw.githubusercontent.com/Text2VR/Text2VR/main/docs/pipeline_overview.png)

---

## Tech Highlights

- **LangGraph Orchestration**  
  Explicit workflow graph managing query rewrite, panorama generation, segmentation, inpainting, 3D asset generation, and Gaussian Splatting.

- **Microservice Architecture (via Docker Compose)**  
  Separate containers for:
  - Panorama generation (`DREAMSCENE360/` → `panorama-api`)
  - Segmentation (`ASSET_SEG/` → `segmentation-api`)
  - Inpainting (`BG_INPAINT/` → `inpainting-api`)
  - 3D asset generation (TRELLIS → `trellis-api`)

- **Frontend Integration**  
  React + TypeScript frontend (`src/`) for:
  - Prompt input and scene naming  
  - Real-time pipeline status and intermediate previews  
  - Result browsing and download (panorama, assets, PLY, Unity export)

- **VR-focused Outputs**  
  - 3D Gaussian Splatting background (PLY)  
  - GLB assets ready for import into VR engines  
  - Unity-ready export endpoints (`unity_assets` API) for building interactive VR scenes

---

## Monorepo Structure (High-Level)

For full details, see the `Text2VR` repository README. At a glance:

- `app/` – FastAPI backend and LangGraph workflow
- `src/` – React frontend
- `DREAMSCENE360/` – Panorama generation service
- `ASSET_SEG/` – Segmentation service
- `BG_INPAINT/` – Background inpainting service
- `docker-compose.yml` – Microservice orchestration
- `docs/` – Architecture and pipeline diagrams
- `pre_checkpoints/` – Pretrained model weights

---

## Team: GARASANI

Text2VR is developed by **Team GARASANI** (Capstone Design · Graduation 2025).

<table>
  <tr>
    <td align="center" valign="top" width="160">
      <a href="https://github.com/LeeChangmin0310">
        <img src="https://github.com/LeeChangmin0310.png?size=120" width="96" height="96" alt="LeeChangmin0310 avatar"/><br/>
        <sub><b>Chang-Min Lee</b></sub><br/>
        <sub>@LeeChangmin0310</sub><br/>
        <sub>Project Lead & Maintainer</sub>
      </a>
    </td>
    <td align="center" valign="top" width="160">
      <a href="https://github.com/suyeonmyeong">
        <img src="https://github.com/suyeonmyeong.png?size=120" width="96" height="96" alt="suyeonmyeong avatar"/><br/>
        <sub><b>Soo-Youn Myoung</b></sub><br/>
        <sub>@suyeonmyeong</sub><br/>
        <sub>Core Contributor</sub>
      </a>
    </td>
    <td align="center" valign="top" width="160">
      <a href="https://github.com/dalsik">
        <img src="https://github.com/dalsik.png?size=120" width="96" height="96" alt="dalsik avatar"/><br/>
        <sub><b>Moon-Sik An</b></sub><br/>
        <sub>@dalsik</sub><br/>
        <sub>Core Contributor</sub>
      </a>
    </td>
    <td align="center" valign="top" width="160">
      <a href="https://github.com/0in11">
        <img src="https://github.com/0in11.png?size=120" width="96" height="96" alt="0in11 avatar"/><br/>
        <sub><b>Young-In Jin</b></sub><br/>
        <sub>@0in11</sub><br/>
        <sub>Core Contributor</sub>
      </a>
    </td>
  </tr>
</table>

> “Garasani (가라사니): the spark of perception, the thread of insight.”  
> Text2VR explores how unstructured prompts can be transformed into structured, interactive 3D worlds.

---

## For Reviewers and Collaborators

- The **HCI conference paper** and the **computer game journal version** both reference this organization as the implementation source.  
- To reproduce the system:
  - Start from `Text2VR/Text2VR`
  - Follow the repository README for environment setup, model downloads, and pipeline execution
- Issues and feature requests can be opened on the main repository.
