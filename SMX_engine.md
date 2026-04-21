# Super-Meta-X (SMX) Enterprise

**Advanced Meta-Analysis & Data Synthesis Platform**

*Fig 1: Main Dashboard & Import Pipeline*

---

## Overview

Super-Meta-X (SMX) Enterprise is an advanced, automated statistical environment designed for comprehensive meta-analysis and evidence synthesis. Built natively on the R `meta` and `metafor` ecosystems, SMX provides an interactive, enterprise-grade interface for complex data reshaping, robust variance estimation, missing data imputation, and high-fidelity geometric visualizations optimized for immediate manuscript integration.

---

## Key Features

### 📥 Advanced Data Pipeline & Imputation

- **Multi-Format Ingestion:** Native support for Standard Matrices (CSV, Excel, SAS, SPSS) and Cochrane RevMan Archives (`.rm5`).
- **Data Diagnostics (EDA):** Immediate distributional geometries, mapping, and five-number summary statistics.
- **Quality Control:** Automated filtering rules to purge NA values and duplicate arrays prior to analysis.
- **Non-Parametric Imputation:** Integrated MICE (Multiple Imputation by Chained Equations) engine to intelligently handle missing data vectors.

### 🧮 Core Analytical Engine

- **Statistical Equivalency:** Algorithms run natively against standard implementations of R's `meta` and `metafor` packages.
- **Cluster-Robust Variance:** Incorporates Sandwich Estimators to handle structural study clusters dynamically.
- **Influence & Isolation Algorithms:**
  - Iterative Leave-One-Out (LOO) Matrices
  - Viechtbauer-Cheung Diagnostics
  - Map Baujat Dimensionality
  - Combinatorial GOSH Extrapolation

*Fig 2: Influence Algorithms, GOSH Array & Iterative LOO Mapping*

### 📊 High-Resolution Geometries & Topography

- **Primary Forest Mapping:** Chronological hierarchical ordering, interactive I² and τ² indices, and proportional weight vector visualization.
- **Advanced Projections:** Galbraith Structural Radials, Drapery P-Value Projections, Confidence Limit Topologies, and Proportional Bubble Geometries.
- **Bias Integration:** Direct integration with Risk of Bias 2 (RoB 2) instruments for RCTs.

*Fig 3: Precision vs. Standardized Effect (Galbraith Structural Radial)*

### 📉 Sensitivity & Bias Detection

| Component | Description |
|---|---|
| Distributional Asymmetry | Trigger base asymmetry protocols using Egger's Regression rules. |
| Doi & LFK Modeling | Construct Doi plots and extract LFK indices to quantify publication bias. |
| Weight-Function Heuristic | Negative Exponential Curve modeling to fit publication weight models. |
| Outlier Filtration | Execution of heuristic filters for structural outlier isolation and RVE logic. |

### 🔍 Moderator Analysis (Meta-Regression)

- **Categorical Stratification:** Re-calculate pooling architectures within partitions with Q-test execution for between-group structural deviations.
- **Linear Mixed-Effects Regression:** REML parameter optimization matrices with mean-centering capabilities for continuous numeric covariates.

### 📑 Pipeline I/O & Reporting

- **Automated Compilation:** Trigger RMarkdown compilation sequences to generate HTML application outputs or embed primary geometries directly.
- **State Preservation:** Write active RAM memory matrices directly to `.rds` disks and securely re-hydrate host memory across sessions.
- **Execution Telemetry:** Built-in interception of mathematical flags (e.g., division by zero, rank deficiencies) to ensure server stability.

---

## Visual Reference Gallery

<p align="center">
  <img src="docs/images/image_p1_1.jpeg" alt="Fig 1 - Main Dashboard and Import Pipeline" width="30%">
  <img src="docs/images/image_p2_1.jpeg" alt="Fig 2 - Engine Architecture and Transparency" width="30%">
  <img src="docs/images/image_p3_1.jpeg" alt="Fig 3 - Influence Algorithms and Outlier Detection" width="30%">
</p>

<p align="center">
  <img src="docs/images/image_p4_1.jpeg" alt="Fig 4 - Leave-One-Out Diagnostics Table" width="30%">
  <img src="docs/images/image_p5_1.jpeg" alt="Fig 5 - Publication Filter and Omitted Studies" width="30%">
  <img src="docs/images/image_p6_1.jpeg" alt="Fig 6 - Doi Geometry Projection" width="30%">
</p>

<p align="center">
  <img src="docs/images/image_p7_1.jpeg" alt="Fig 7 - Contour Funnel Mapping" width="30%">
  <img src="docs/images/image_p8_1.jpeg" alt="Fig 8 - Preview and Semantic Map" width="30%">
  <img src="docs/images/image_p9_1.jpeg" alt="Fig 9 - Categorical Stratification and Meta-Regression" width="30%">
</p>

<p align="center">
  <img src="docs/images/image_p10_1.jpeg" alt="Fig 10 - Proportional Bubble Geometry" width="30%">
  <img src="docs/images/image_p11_1.jpeg" alt="Fig 11 - Residual Standard Error Array" width="30%">
  <img src="docs/images/image_p12_1.jpeg" alt="Fig 12 - Forest Topography and Bias Integration" width="30%">
</p>

<p align="center">
  <img src="docs/images/image_p13_1.jpeg" alt="Fig 13 - Forest Topography Duplicate View" width="30%">
  <img src="docs/images/image_p14_1.jpeg" alt="Fig 14 - Cumulative Saturation and Iterative Information Addition" width="30%">
  <img src="docs/images/image_p15_1.jpeg" alt="Fig 15 - RoB 2 Scaffold and Study Data Table" width="30%">
</p>

<p align="center">
  <img src="docs/images/image_p16_1.jpeg" alt="Fig 16 - Estimation Mechanics and Pooling Architecture" width="30%">
  <img src="docs/images/image_p17_1.jpeg" alt="Fig 17 - Resolved Mathematical Parameters and Empirical Vector Distribution" width="30%">
</p>

---

## Technical Stack

| Layer | Technology |
|---|---|
| Engine / Environment | R · RStudio / Shiny Server |
| Statistical Libraries | meta · metafor · mice · clubSandwich |
| Visualization | Custom ggplot2 outputs optimized for academic publication |
| Reporting | RMarkdown · HTML/PDF Compilation |

---

## Repository Structure

```text
/SuperMetaX
├── build_docs.py      # End-to-end automation script (builds HTML & auto-pushes)
├── SMX_engine.md      # Raw Markdown documentation source
├── SMX_engine.html    # Compiled dark-theme documentation with image gallery
├── README.md          # Project overview for GitHub
└── docs/
    └── images/        # UI and system reference screenshots (image_p1_1.jpeg … image_p17_1.jpeg)
```

---

## Author

**Dr. Salman Khalid, MBBS**
King Edward Medical University (KEMU), Class of 2024

- 🌐 Portfolio: [salman24012715.github.io](https://salman24012715.github.io)
- 📧 Email: [salman24012715@gmail.com](mailto:salman24012715@gmail.com) · [salmankhalid@kemu.edu.pk](mailto:salmankhalid@kemu.edu.pk)
- 🆔 ORCID: [0009-0006-3471-2710](https://orcid.org/0009-0006-3471-2710)

---

## License

© 2024–2026 Dr. Salman Khalid. All rights reserved.

This software is proprietary. You may view this repository for evaluation purposes only. Copying, modification, distribution, commercial use, or creation of derivative works is strictly prohibited without prior written permission. See `LICENSE` for full terms.
