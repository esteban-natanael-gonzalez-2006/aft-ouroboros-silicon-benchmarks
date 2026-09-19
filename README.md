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

### 1. Clone the Repository
```bash
git clone [https://github.com/esteban-natanael-gonzalez-2006/aft-ouroboros-silicon-benchmarks.git](https://github.com/esteban-natanael-gonzalez-2006/aft-ouroboros-silicon-benchmarks.git)
cd aft-ouroboros-silicon-benchmarks
```

### 2. Compilation
```bash
For In-Order / Low-Power Cores (e.g., ARM Cortex-A53 / MediaTek Helio G37):
```

For Out-of-Order / High-Performance Cores (e.g., Kryo 585 / Cortex-A77 / Snapdragon 870):
```bash
clang -Wall -Wextra -O3 -march=armv8.2-a -mcpu=cortex-a77 -ffast-math src/ouroboros_descartes_harness.c -o harness -lm
```

For Generic x86_64 / Linux / macOS:
```bash
gcc -Wall -Wextra -O3 src/ouroboros_descartes_harness.c -o harness -lm
```

### 3. Execution
Balanced Mode (Recommended for mobile devices and low-power cores to avoid thermal throttling):
```bash
./harness --moto
```

Performance Mode (Full calibration iterations for high-end mobile SoCs and desktop CPUs):
```bash
./harness
```

## Expected Empirical Results
A successful replication will yield:
Numerical Residual: Maximum relative error \epsilon_{\text{rel}} \le 0.0152\% for N \le 256, bounded by single-precision FP32 rounding over 130,000+ pairs.
Scaling Exponents (\text{OLS Log-Log}):
\text{Pairwise} \to \hat{\alpha} \approx 2.00 \pm 0.05 with R^2 > 0.999.
\text{Descartes} \to \text{Affine Model } T(N) = a + b \cdot N \text{ with } R^2 > 0.97.
Anti-DCE Verification: Inline volatile assembly barriers guarantee that the compiler does not optimize away contractions.
Intellectual Property & Legal Notice
The mathematical theorem and public benchmarking harness contained herein are provided for peer review and scientific verification. All topological phase modulation rules, Butterfly coupling generator sequences, typed-slot architectures, and proprietary binary implementations are strictly reserved under trade secret protection and pending patent rights by the author.
Institutional Contact: esteban@aft-ouroboros.org
