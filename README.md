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
