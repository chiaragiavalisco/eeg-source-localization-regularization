# EEG Source Localization: Inverse Problem Solving via Regularization & Truncated SVD

![MATLAB](https://img.shields.io/badge/Language-MATLAB-orange.svg)
![Field](https://img.shields.io/badge/Domain-Biomedical%20Signal%20Processing%20%7C%20Inverse%20Problems-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

Academic computational project developed for the *Numerical Methods for Data Mining* course (Dipartimento di Matematica e Applicazioni "Renato Caccioppoli", Università degli Studi di Napoli Federico II).

This project models the electroencephalography (EEG) forward problem within a 3D spherical volume conductor ($R = 20\text{ cm}$) and compares numerical regularization techniques to solve the underdetermined inverse problem of neural source localization.

---

## 📌 Executive Summary

Electroencephalography (EEG) source localization aims to estimate intracranial neural current dipole activity from non-invasive scalp electric potentials. 

The overall framework consists of two complementary stages:
* **The Forward Problem**: Computes the electric potentials recorded at the surface electrodes given an assumed configuration of intracranial current sources within a discretized volume conductor model.
* **The Inverse Problem**: Reconstructs the unknown spatio-temporal source activation profiles from the observed multichannel electrode recordings.

In this setup, the head volume conductor is discretized as a homogenous sphere of radius $R = 20\text{ cm}$ containing $N = 32$ distributed neural sources and $M = 5$ surface electrodes over $T = 100$ discrete sampling instances.

The linear mapping relating internal dipole activities to scalp recordings is governed by:

$$V = G x + \epsilon, \quad M \ll N$$

where:
* $V \in \mathbb{R}^{5 \times 100}$ represents the measured potential matrix across all channels over time.
* $x \in \mathbb{R}^{32 \times 100}$ denotes the unknown source intensity matrix.
* $G \in \mathbb{R}^{5 \times 32}$ is the **Lead Field Matrix**, encoding head geometry and conductive properties via an inverse-squared Euclidean distance metric.
* $\epsilon \in \mathbb{R}^{5 \times 100}$ denotes additive measurement noise.

Because the number of active dipole sources significantly exceeds the number of recording sensors ($M \ll N$), the inverse system is severely underdetermined and ill-conditioned, possessing an infinite number of admissible solutions.

This repository demonstrates:
1. **Geometric Forward Modeling**: Construction of a spherical conductor ($R = 20\text{ cm}$), interior dipole coordinates ($N = 32$), and surface electrode placement ($M = 5$).
2. **Dynamic Signal Simulation**: Multichannel coupled autoregressive (AR) dynamics over 100 time points with additive Gaussian noise.
3. **Analytic Minimum-Norm Solution**: Minimum $L^2$-norm regularized reconstruction using the right Moore-Penrose pseudo-inverse $x = G^T (G G^T)^{-1} V$.
4. **Truncated SVD Thresholding**: Moore-Penrose pseudo-inversion across singular value cutoff tolerances $\tau \in \{10^{-1}, 10^{-2}, 10^{-3}, 10^{-4}\}$ to evaluate reconstruction fidelity and numerical stability.

---

## 🧠 Mathematical Formulation

### 1. Forward Model & Lead Field Matrix ($G$)
Derived from electrostatic point-source field decay:

$$E(r, t) = \frac{1}{4\pi\epsilon_0} \sum_{i=1}^N \frac{q_i(t) \mathbf{a}_i(t)}{R^2}$$

where $R = \norm{r - r_i} ||$, $q_i(t)$ is the signal from source $i$, and $\mathbf{a}_i(t)$ is a unit vector pointing in the direction of the line between the charge and the field point $r$.

The lead field matrix $G$ is constructed using the following approximation equation, where the coupling coefficient between electrode channel $i$ and source dipole $j$ is modeled as:

$$G_{ij} = \frac{c}{R^2}, \quad c = 1$$

### 2. Inverse Problem & Regularization Framework
The inverse problem consists of estimating the source activity values that generated the measured electric potential field vector at the electrodes. The general strategy formulates this estimation as a regularized linear optimization problem:

$$\hat{x} = \min_x \left( ||V - Gx ||_2^2 + \sum_{i=1}^k \alpha_i \|W_i x\|_p \right)$$

where:
* $k$ is the number of regularization constraints reflecting a priori physiological information.
* $W_i \in \mathbb{R}^{5 \times 32}$ are weighting matrices associated with the imposed constraints.
* $\alpha_i > 0$ are the regularization parameters controlling the trade-off and relative importance of each penalty term.

### 3. Inverse Methods Comparison

* **Minimum $L^2$-Norm Regularization:**
  Setting $k=1$, $p=2$, and identity weighting simplifies the unconstrained optimization into the minimum $L^2$-norm solution via the right pseudo-inverse:
  $$x_{\text{reg}} = G^T (G G^T)^{-1} V$$
* **Moore-Penrose Pseudo-Inverse (SVD-based):**
  Computed via Singular Value Decomposition with singular-value thresholding tolerance $\tau$:
  $$x_{\text{pinv}} = G^\dagger_\tau V$$

### 4. Key Findings
* A tolerance threshold of $\tau = 10^{-4}$ guarantees numerical robustness, matching the analytic minimum-norm reconstruction with machine-precision residual error ($\|V - G x\| \sim 10^{-15}$).
* A tolerance of $\tau = 10^{-3}$ excessively truncates informative singular components, resulting in non-negligible reconstruction distortion ($\sim 10^0$).
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
   git clone [https://github.com/chiaragiavalisco/eeg-source-localization-regularization.git](https://github.com/chiaragiavalisco/eeg-source-localization-regularization.git)
   cd eeg-source-localization-regularization

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
