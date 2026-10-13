# Human-Centered Multimodal Verification Agent on LINE
### Submodule: AI-Generated & Manipulated Media Detection Under Platform Degradation

This repository contains the visual media verification engine for a senior project developing an explainable fact-checking and synthetic media detection agent on the LINE messaging platform, targeting older adults in Thailand.

---

## 📌 Problem Overview

When images and screenshots are shared through private messaging platforms like LINE, they undergo lossy transformations:
* Re-compression (JPEG artifacts down to Q50–Q70)
* Resizing / downsampling (clamping dimensions to max 1080px)
* Multi-generational forwarding and screenshot crops

Standard convolutional and transformer-based AI detectors rely on delicate high-frequency noise and pixel-level artifacts. Transmission through LINE frequently strips these artifacts, causing severe detector performance degradation. This project investigates detector robustness under realistic LINE compression and implements a hybrid verification pipeline combining provenance metadata, forensic models, and generalized semantic classifiers.

---

## 🏗️ Verification Pipeline

```text
Incoming LINE Media
        │
        ▼
[Layer 1: Provenance Check] ─── Valid C2PA / SynthID ───► Verifiable AI / Human Origin
        │ (Absent / Stripped)
        ▼
[Layer 2: Dual Detector Ensemble]
   ├─ Semantic & Artifact: CO-SPY (CVPR 2025)
   └─ Generalized Feature: UniversalFakeDetect (UnivFD)
        │
        ▼
[Layer 3: Forensic Manipulation Check]
   └─ Localization: TruFor (Splicing / Inpainting Detection)
        │
        ▼
[Evidence Fusion & Uncertainty Modeling]
        │
        ▼
Explainable Output (3-Level Progressive Disclosure for Thai Seniors)
---

## 🔬 Experimental Roadmap

| Phase | Method / Model | Primary Objective | Status |
| --- | --- | --- | --- |
| **Exp 1: Baseline** | Transfer Learning (ResNet50 / ConvNeXt-Tiny) | Measure baseline accuracy drop on clean vs. LINE-degraded images (`Tiny-GenImage`). | Completed |
| **Exp 2: Generalized Baseline** | UniversalFakeDetect (CLIP ViT-L/14 backbone) | Benchmark cross-generator generalization against compression. | In Progress |
| **Exp 3: Main Detector** | CO-SPY (CVPR 2025) | Evaluate dual-branch (pixel artifact + semantic) detection. | Planned |
| **Exp 4: Adaptation** | CO-SPY + LINE Augmentation Fine-Tuning | Fine-tune feature fusion layers using LINE-simulated degradation transforms. | Planned |
| **Exp 5: Integration** | Hybrid Engine (C2PA + TruFor + CO-SPY) | Full pipeline integration into the conversational LINE Bot backend. | Planned |

---

## 📂 Project Structure

```text
├── notebooks/
│   ├── Experiment_1_Baseline.ipynb       # ResNet50 baseline & LINE compression simulator
│   ├── Experiment_2_UnivFD.ipynb         # UniversalFakeDetect evaluation
│   └── Experiment_3_COSPY.ipynb          # CO-SPY testing & fine-tuning
├── src/
│   ├── transforms/
│   │   └── line_simulation.py            # LINE resizing, crop, and JPEG compression hooks
│   ├── models/
│   │   ├── baseline_resnet.py
│   │   └── ensemble.py
│   └── provenance/
│       └── c2pa_extractor.py             # C2PA metadata validator
├── data/                                 # Datasets & evaluation splits (Tiny-GenImage, custom)
├── weights/                              # Checkpoints (excluded from Git tracking)
├── requirements.txt
└── README.md

```

---

## 🚀 Quick Start (Google Colab)

### 1. Requirements

* Python 3.10+
* PyTorch 2.0+ (CUDA GPU enabled, e.g., T4 on Colab)

### 2. Running Experiment 1 (Baseline)

Open `notebooks/Experiment_1_Baseline.ipynb` in Google Colab:

1. Set runtime type to **T4 GPU** (`Runtime > Change runtime type > T4 GPU`).
2. Run all cells to:
* Download and stream `TheKernel01/Tiny-GenImage` from Hugging Face.
* Fine-tune a pre-trained ResNet50 binary classifier (`Real` vs. `AI`).
* Benchmark accuracy degradation using `SimulateLINECompression(quality=70, max_size=1080)`.
* Export the model weights (`baseline_ai_detector.pth`).



---

## 📊 LINE Compression Simulation Parameters

To reflect mobile transmission behaviors, test images are subjected to:

* **Max Dimension:** Lanczos downsampling to 1080px bounding box
* **Lossy Compression:** PIL JPEG encoding with quality factors $Q \in \{50, 70, 90\}$
* **Metadata Stripping:** Complete stripping of EXIF and ICC profiles (standard messaging behavior)

---

## 👥 Target Users & Explainability Layer

Outputs are formatted for conversational consumption by Thai older adults using a 3-layer progressive disclosure interface:

1. **Layer 1 (Assessment):** Actionable, respectful verdict (e.g., `ควรระวัง: ภาพนี้มีสัญญาณที่อาจสร้างด้วย AI`).
2. **Layer 2 (Visual Evidence):** Plain-language reasoning (identifying anatomical inconsistencies, unnatural texture patterns, or localized alterations).
3. **Layer 3 (Action Advice):** Direct fraud mitigation steps (e.g., avoiding clicking unverified links or making transfers).

```

<FollowUp label="Want me to generate the requirements.txt file next?" query="Please generate the requirements.txt file for this thesis repository."/>

```
