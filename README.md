# Sub-Micron Position Reconstruction in 3D Silicon Detectors via Multi-Domain CNNs

This repository contains the complete deep learning pipeline for high-precision hit reconstruction in 3D-trench silicon pixels using the Two-Photon Absorption Transient Current Technique (TPA-TCT). The system achieves a record **fundamental resolution of 0.80 µm**, outperforming the 2024 JINST state-of-the-art benchmarks (1.5 µm) for similar technologies.

## 🚀 Key Achievements
* **Record Precision:** Achieved a **0.80 µm Euclidean MAE** in position regression within the fundamental domain.
* **Symmetry-Aware AI:** Integrated Group Theory (C₂v Irreps) into the feature engineering process to resolve intra-pixel quadrant ambiguities with **91.1% accuracy**.
* **Advanced Optimization:** Executed an 11-hour Bayesian Hyperparameter search (Optuna) and multi-seed statistical validation to ensure sub-micron stability.
* **Selective Reconstruction:** Implemented a confidence-based **Fiducial Cut** system, reaching a reconstruction resolution of **1.15 µm** at 60% event retention.

---

## 📂 Project Pipeline

### 1. [Physics-Informed Feature Engineering](https://www.kaggle.com/code/ibongarciagomez/nb-feature-engineering)
This notebook establishes a comprehensive signal processing pipeline that converts raw TPA-TCT current waveforms into a 110-dimensional multi-domain feature vector. By applying hierarchical PCA across temporal, derivative, and frequency domains, the system captures complex asymmetric charge transport modes that are otherwise obscured in global variance. Furthermore, the pipeline leverages C₂v group theory to extract irreducible representation (Irrep) features, which provide a dedicated physical basis for resolving quadrant sign ambiguities. Final feature importance is validated through Mutual Information ranking and spatial stratification.

### 2. [Multi-Domain CNN Training Pipeline](https://www.kaggle.com/code/igarciag/02-multi-domain-cnn-training-pipeline)
This work implements a specialized dual-branch neural network designed for high-precision hit reconstruction in 3D-trench silicon detectors. The architecture fuses 2-channel waveform data with a physics-informed MLP branch, feeding a multi-task framework that simultaneously performs fundamental domain regression and quadrant classification. Through a two-phase optimization strategy combining Cosine Annealing Warm Restarts with a plateau-based fine-tuning stage, the model achieves a sub-micron fundamental resolution of 0.80 µm.

### 3. [Bayesian HPO Optimization](https://www.kaggle.com/code/ibongarciagomez/nb03-optuna-hpo-optimization)
This framework executes an 11-hour Bayesian hyperparameter optimization search using the Optuna library to tune 14 critical parameters across the entire training pipeline. By implementing aggressive percentile pruning to manage compute resources, the search identifies the optimal balance for the composite loss function and focal alpha weights, maximizing the network’s sensitivity to subtle signal fluctuations.

### 4. [Multi-Seed Validation & Robustness](https://www.kaggle.com/code/igarciag/04-multi-seed-validation)
This validation suite ensures the scientific integrity and robustness of the reconstruction model by performing a multi-seed statistical analysis. By evaluating the model across multiple random initializations and analyzing snapshot ensemble stability, the notebook confirms that the achieved spatial resolutions are consistent and statistically significant.

---

## 📊 Dataset
The associated dataset, **TPA-3DT-MultiDomain-SubMicron**, contains 17,473 laser-induced current events with high-granularity waveforms and hand-crafted symmetry features. 
* **Available on Kaggle:** [TPA-TCT Research Project Facil](https://www.kaggle.com/datasets/igarciag/3d-silicon-submicron-tpa).

---

## 🛠️ Methodology & Physics Background
The core of this project lies in exploiting the **C₂v point-group symmetry** of the 3D-trench pixel. While standard regression struggles with quadrant ambiguity, our **Multi-Domain** approach uses:
1. **Time Domain:** 2-channel 1D-CNN (Raw + dV/dt derivative).
2. **Frequency Domain:** FFT magnitude features via hierarchical PCA.
3. **Symmetry Domain:** B₂ and B₁ Irrep projections to isolate u-sign and v-sign asymmetries.

---

## 📈 Performance Summary

| Metric | v5.9 (Baseline) | **v5.24 (This Work)** |
| :--- | :---: | :---: |
| **Fund. Res. (Euclidean)** | 2.25 µm | **0.80 µm** |
| **Quadrant Accuracy** | 81.2% | **88.5%** |
| **Soft Res. (Full Space)** | 4.06 µm | **2.12 µm** |
| **U-sign Accuracy** | — | **91.1%** |

*Final configuration uses a Top-1 Ensemble, Temperature Optimization (T=0.6552), and a Confidence Gate (margin > 1.22).*

---
*Author: Ibón García Gómez · Research Project · February 2026*
