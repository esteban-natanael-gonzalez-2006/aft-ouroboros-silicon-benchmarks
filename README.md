# AFT_Ouroboros: Silicon Benchmarks & Asymptotic Evaluation

[![License: Evaluation](https://img.shields.io/badge/License-Academic%20Evaluation-blue.svg)](LICENSE)
[![Platform: ARM64 / Snapdragon 870](https://img.shields.io/badge/Hardware-Snapdragon%20870%20(Kryo%20585)-orange.svg)](#hardware-testbed)
[![Paper](https://img.shields.io/badge/Technical%20Paper-PDF%20Available-green.svg)](paper/AFT_Ouroboros_Technical_White_Paper.pdf)

Official experimental benchmark suite and technical white paper for **AFT_Ouroboros**, an associative continuous-state architecture operating within the L2 CPU cache of consumer mobile silicon.

## Technical White Paper
* **Full Report (8 Pages, IEEE format):** [`paper/AFT_Ouroboros_Technical_White_Paper.pdf`](paper/AFT_Ouroboros_Technical_White_Paper.pdf)  
  *AFT_Ouroboros: Asymptotic Reduction, Sub-Watt Silicon Execution, and Empirical Evidence Matrix on Consumer Hardware.*

---

## Validated Empirical Metrics (Qualcomm Snapdragon 870 @ 3.19 GHz)

| Metric | Measured Value | Epistemological Status |
| :--- | :--- | :--- |
| **Anti-DCE Hardware Control (TEST 01)** | Physical execution ratio $T_{B2}/T_{B1} = 675,535\times$ | Measured |
| **Mathematical Equivalence (TEST 11)** | Residual error $\epsilon_{\text{rel}} \le 0.0152\%$ in FP32 vs Pairwise | Measured |
| **Pairwise Quadratic Scaling (TEST 12)** | $\hat{\alpha} = 2.18 \pm 0.19$ ($IC_{95\%}: [1.99, 2.38]$, $R^2 = 0.994$) | Measured |
| **Descartes Asymptotic Scaling (TEST 12)** | Affine model $T(N) = 1.341\text{ }\mu\text{s} + 15.39\text{ ns}\cdot N$ ($R^2 = 0.9996$) | Measured |
| **Intrinsic Training Throughput (TEST 13)** | $15,031\text{ tokens/s}$ constant across epochs ($T_{\text{CPU}} \approx 5.45\text{ s}$) | Measured |
| **Power Dissipation** | Estimated $\le 0.455\text{ W}$ (sub-watt execution) | Calculated |
| **Static Memory Working Set** | $1,024.00\text{ KiB}$ weights in L2, $4.75\text{ KiB}$ activations in L1D | Derived |

---

## Reproducibility Guide

### Requirements
* Linux / Android (Termux) on ARM64 (`aarch64`) or x86_64.
* `clang` (recommended) or `gcc`.
* `taskset` (for CPU affinity).

### 1. Verify Dataset Integrity
```bash
sha256sum data/corpus_curado_100kb.txt
# Expected: e82fa4e7689835bfd8c3db7195c83de620abb6e0cb9441d00ef2e27e5c1ac9ab
