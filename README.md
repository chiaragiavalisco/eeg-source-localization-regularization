# EEG Source Localization: Inverse Problem Solving via Regularization & Truncated SVD

![MATLAB](https://img.shields.io/badge/Language-MATLAB-orange.svg)
![Field](https://img.shields.io/badge/Domain-Biomedical%20Signal%20Processing%20%7C%20Inverse%20Problems-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

Academic computational project developed for the *Numerical Methods for Data Mining* course (Dipartimento di Matematica e Applicazioni "Renato Caccioppoli", Università degli Studi di Napoli Federico II).

This project models the electroencephalography (EEG) forward problem within a 3D spherical volume conductor ($R = 20\text{ cm}$) and compares numerical methods to solve the underdetermined inverse problem of neural source localization.

---

## 📌 Executive Summary

Electroencephalography (EEG) source localization aims to estimate intracranial neural current dipole activity from scalp electric potentials.

Because the number of active sources ($N = 32$) exceeds the number of recorded scalp electrode channels ($M = 5$), the forward operator yields an underdetermined linear system:

$$V(t) = G x(t) + \epsilon(t), \quad M \ll N$$

This repository demonstrates:
1. **Geometric Forward Modeling**: Spherical head model ($R = 20\text{ cm}$), 32 random internal dipoles, and 5 scalp electrodes.
2. **Synthetic Signal Simulation**: Coupled autoregressive (AR) dynamics across 5 channels over 100 discrete time instances with additive Gaussian noise.
3. **Analytic Minimum-Norm Solution**: Minimum $L^2$-norm regularized reconstruction using the right pseudo-inverse $x = G^T (G G^T)^{-1} V$.
4. **Moore-Penrose Pseudo-Inverse (TSVD)**: Truncated SVD thresholding at tolerances $\tau \in \{10^{-1}, 10^{-2}, 10^{-3}, 10^{-4}\}$ to evaluate reconstruction robustness.

---

## 🧠 Mathematical Formulation

### 1. Forward Model & Lead Field Matrix ($G$)
Derived from electrostatic point-source field decay:

$$E(r, t) = \frac{1}{4\pi\varepsilon_0} \sum_{i=1}^N \frac{q_i(t) \mathbf{a}_i(t)}{R^2}$$

The connection between channel $i$ and source $j$ is approximated via:

$$G_{ij} = \frac{c}{\|\mathbf{p}_{\text{sensor}, i} - \mathbf{p}_{\text{source}, j}\|^2}, \quad c = 1$$

where $V \in \mathbb{R}^{5 \times 100}$, $G \in \mathbb{R}^{5 \times 32}$, and $x \in \mathbb{R}^{32 \times 100}$.

### 2. Inverse Methods Comparison

* **Minimum $L^2$-Norm Regularization:**
  $$x_{\text{reg}} = G^T (G G^T)^{-1} V$$
* **Moore-Penrose Pseudo-Inverse (SVD-based):**
  $$x_{\text{pinv}} = G^\dagger_\tau V$$

### 3. Key Findings
* A tolerance threshold of $\tau = 10^{-4}$ achieves high stability and matches the analytic minimum-norm reconstruction (residual error $\|V - G x\| \sim 10^{-15}$).
* A tolerance of $\tau = 10^{-3}$ prematurely truncates significant singular components, yielding non-negligible tracking error ($\sim 10^0$).

---

## 📊 Visualizations & Diagnostic Outputs

* **3D Geometry**: Visual representation of the 32 internal sources inside the spherical volume conductor alongside the 5 surface electrodes.
* **Electrode Tracking**: Multi-panel comparisons across all 5 channels showing ground truth $V_i(t)$, regularized reconstruction, and pseudo-inverses with $\tau = 10^{-4}$ and $\tau = 10^{-3}$.
* **Residual Contour Maps**: Space-time contour plots ($5\text{ channels} \times 100\text{ samples}$) of absolute tracking error $|V - G \hat{x}|$.

---

## 🚀 Getting Started

### Prerequisites
* **MATLAB** (R2019b or newer recommended; requires `tiledlayout`).
* Runs entirely on core MATLAB linear algebra routines (no additional toolboxes required).

### Running the Code
1. Clone this repository:
   ```bash
   git clone [https://github.com/chiaragiavalisco/chiaragiavalisco.github.io.git](https://github.com/chiaragiavalisco/chiaragiavalisco.github.io.git)
   cd chiaragiavalisco.github.io

2. Open MATLAB, navigate to the cloned folder, and run:
   ```matlab
   run('eeg_source_localization.m')
   ```

---

## 🛠 Skills & Competencies Demonstrated

* **Numerical Linear Algebra**: Underdetermined linear systems, Moore-Penrose pseudo-inverses, Singular Value Decomposition (SVD), rank conditioning, and spectral matrix norms.
* **Regularization Theory**: Tikhonov / Minimum-norm formulation, Truncated SVD, noise sensitivity, and hyperparameter/threshold tuning.
* **Biomedical Modeling**: Biophysical volume conduction, forward/inverse EEG problem modeling, lead field matrix formulation.
* **MATLAB Computing**: Vectorized computation, 3D geometric visualization, dynamic simulation, and publication-ready multi-axis plotting.

---

## 👤 Author

**Chiara Giavalisco**  
* Master's Degree Coursework: *Numerical Methods for Data Mining*  
* [LinkedIn Profile](https://www.linkedin.com/in/chiara-giavalisco-28b1b9268/) • [GitHub Profile](https://github.com/chiaragiavalisco) • [Email](mailto:chiara.giavalisco@gmail.com)

---

## 📄 License
This project is open-source and available under the [MIT License](LICENSE).
