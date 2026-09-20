# Bayesian Spatio-Temporal Modeling of Net Migration across Italian Municipalities
### Associations, Spatio-Temporal Structure, and Territorial Heterogeneity

[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-blue?logo=github)](https://github.com/ClaudiaFran/Poster-statistical-models)
[![R](https://img.shields.io/badge/Language-R-276DC3?logo=r)](https://www.r-project.org/)
[![INLA](https://img.shields.io/badge/Inference-R--INLA-orange)](https://www.r-inla.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Authors:**
* **Claudia Franceschini** &nbsp;([claudia.franceschini@phd.unipd.it](mailto:claudia.franceschini@phd.unipd.it))
* **Sophie Grace Parolin** &nbsp;([sophiegrace.parolin@phd.unipd.it](mailto:sophiegrace.parolin@phd.unipd.it))
* **Teresa Secco** &nbsp;([teresa.secco@unive.it](mailto:teresa.secco@unive.it))

*Department of Statistical Sciences, University of Padua*  
*Course in Statistical Models — Ph.D. in Statistical Sciences*

---

## 📌 Project Overview

This repository contains the scientific poster, exploratory figures, and modeling workflow for studying the spatio-temporal dynamics of **municipal net migration in Italy** using a nationwide panel covering 16 consecutive years:
* **$S = 7{,}896$ municipalities** (harmonized according to 2024 administrative borders)
* **$T = 16$ years** ($2004–2019$)
* **$N = 126{,}336$ municipality–year observations**

The response is the municipal **net migration rate per 1,000 residents**:

```math
y_{st}
=
1{,}000
\times
\frac{\mathrm{Net\ Migration}_{st}}
{\mathrm{Resident\ Population}_{st}}
```

For model estimation, the response and continuous covariates are globally
standardized using common national means and standard deviations.

### Main Research Questions

1. How are demographic, economic, and territorial characteristics associated with Italian municipal net migration?

2. What local spatial variation and temporal persistence emerge in net migration at the italian municipal scale?

3. Does allowing for local heterogeneity reveal spatially varying covariate associations beyond a common national specification?
---

## 🔬 Methodological Framework

### 1. Areal Topology & Leroux Spatial Precision matrix
The Italian municipal system is represented as an irregular areal contiguity network $W$ on harmonized 2024 borders ($|V| = 7{,}896$ municipalities, $|E| = 22{,}609$ undirected edges, average degree $\bar{d} = 5.73$ neighbors; 14 island municipalities are retained as natural isolated nodes without artificial marine links):
```math 
W_{ij} = \begin{cases} 1, & \text{if } i \text{ and } j \text{ share a terrestrial boundary}, \\ 0, & \text{otherwise}. \end{cases}
```

Spatial precision is parameterized via the **proper Leroux CAR** specification:
```math 
Q_S(\rho) = \rho(D - W) + (1 - \rho)I_S, \qquad \rho \in (0, 1), \quad D_{ii} = \sum_j W_{ij}
```

### 2. Global Spatio-Temporal Backbone (M1)
The standardized municipal response is decomposed into fixed covariate effects, a **national temporal component**, a **spatio-temporal latent field**,
and Gaussian residual error:
```math
y_{st}^*
=
\alpha
+
\mathbf{x}_{st}^{*\prime}\boldsymbol{\beta}
+
f_t
+
w_{st}
+
\epsilon_{st},
\qquad
\epsilon_{st}
\sim
\mathcal{N}(0,\sigma_\epsilon^2)
```

* **National Temporal Component ($f_t$):** Modeled via an intrinsic first-order random walk with sum-to-zero constraint:
```math
f_t - f_{t-1} \sim \mathcal{N}(0, \, \tau_{\mathrm{RW1}}^2), \qquad t=2,\ldots,T, \qquad \sum_{t=1}^T f_t = 0
```
* **Spatio-Temporal Latent Field ($\mathbf{w}_t$):** Follows a first-order autoregressive AR(1) transition with Leroux spatial precision:
```math
  \mathbf{w}_1 \sim \mathcal{N}\left(\mathbf{0}, \, \tau_{ST}^2 Q_S(\rho)^{-1}\right), \qquad \mathbf{w}_t \mid \mathbf{w}_{t-1} \sim \mathcal{N}\left(\xi \mathbf{w}_{t-1}, \, \tau_{ST}^2 Q_S(\rho)^{-1}\right), \quad t=2,\ldots,T
```
yielding the separable Kronecker-structured joint precision matrix over $\mathbb{R}^{ST}$:
```math
Q_{ST}(\rho, \xi, \tau_{ST}^2) = \frac{1}{\tau_{ST}^2} Q_T(\xi) \otimes Q_S(\rho)
```

### 3. Model Benchmark & Strategies
We benchmark the global backbone against alternative paradigms:
* **M1 (INLA Global)**: Fast approximate Bayesian inference via Integrated Nested Laplace Approximations with an RW(1) trend and AR(1)$\times$CAR field.
* **M2 (MCMC Global)**: Custom Metropolis-within-Gibbs sampler adapted from Mozdzen et al. (2022) (20,000 iterations, 10,000 burn-in, thinning = 10).
* **M3 (Partitioned Overlap INLA)**: 107 province-specific submodels including first-order neighbouring municipalities to reduce boundary effects (Orozco-Acosta et al., 2023).
* **M4 (Partitioned Disjoint INLA)**: 107 strictly disjoint provincial models.
* **M5 (BSTC-DP Clustering)**: Bayesian nonparametric Dirichlet Process clustering on municipal parameters $\bm{\phi}_s = (\bm{\beta}_s, \xi_s)$ (Mozdzen et al., 2022).
* **M6 (Administrative Hierarchical Baseline)**: Regional and provincial IID effects with a national AR(1) trend, without areal graph topology.
---

## 📊 Key Results & Findings

### 1. Out-of-Sample Predictive Comparison
Evaluated across two rolling-origin one-year-ahead out-of-sample folds ($2004\text{--}2017 \to 2018$ and $2004\text{--}2018 \to 2019$, $N_{\mathrm{test}} = 15{,}792$ observations):

| Model | Implementation | RMSE | MAE | $\text{Cov}_{95}$ | $\text{IS}_{95}$ | Computing Time | Max RSS Memory |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **M1** | **INLA Global** | **13.69** | **8.81** | 0.961 | 84.65 | ~19 min | 107.9 GiB |
| **M2** | **MCMC Global** | 13.69 | 8.82 | 0.961 | 84.63 | ~5.8 hrs | 2.6 GiB |
| **M3** | **Overlap INLA** | 13.96 | 9.09 | 0.962 | **77.09** | 107 jobs | Distributed |
| **M4** | **Disjoint INLA** | 14.20 | 9.34 | 0.962 | 77.60 | 107 jobs | Distributed |
| **M5** | **BSTC-DP** | 17.18 | 10.83 | 0.931 | 99.30 | ~6.7 hrs | 4.2 GiB |
| **M6** | **Admin IID** | 13.77 | 8.90 | **0.972** | 88.33 | ~1 min | 1.9 GiB |

* $\text{IS}_{95}$: Interval Score at 95% (lower values indicate sharper, well-calibrated predictive intervals).
* **M1/M2** provide the lowest RMSE and MAE, while **M3** achieves the lowest 95% interval score, indicating the best overall trade-off between predictive interval width and coverage among the compared specifications.

### 2. Global Model (M1) Posterior Estimates

| Parameter | Interpretation | Posterior Mean | Posterior SD | 2.5% | 97.5% |
| :--- | :--- | :---: | :---: | :---: | :---: |
| $\rho$ | Spatial dependence (Leroux CAR) | **0.7774** | 0.0263 | 0.7197 | 0.8224 |
| $\xi$ | Temporal persistence (AR1) | **0.6118** | 0.0144 | 0.5825 | 0.6392 |
| $\tau^2_{ST}$ | Spatio-temporal field variance | **0.3623** | 0.0268 | 0.3139 | 0.4190 |
| $\sigma^2_\epsilon$ | Residual error variance | **0.6689** | 0.0086 | 0.6521 | 0.6860 |
| $\tau^2_{\mathrm{RW1}}$ | Random walk trend variance | **0.1808** | 0.0809 | 0.0562 | 0.3636 |

### 3. Substantive Insights

* **National temporal evolution:** Exploratory temporal patterns suggested a marked change in the overall migration level around the years of the 2008 financial crisis. For this reason, we included a national RW(1) component to flexibly capture common year-to-year variation shared across municipalities, without forcing this variation into the covariate effects or the local spatio-temporal field.

* **Employment and net migration:** The municipal employment rate shows the strongest positive national association with net migration
  ($\hat{\beta} \approx +0.250$). Positive associations are also estimated for mean age and population density growth.

* **Negative national associations:** Greater difficulty in accessing essential services and changes in the foreign-resident share are negatively associated with net migration under the global specification. 

* **Residual spatial structure:** Annual residual Moran's $I$ remains slightly negative ($-0.07$ to $-0.03$, one-sided $p \approx 1$), providing no evidence of remaining positive spatial autocorrelation after fitting the global model. This suggests that the Leroux CAR component captures the dominant spatial dependence.

* **Territorial heterogeneity (M3):** The positive association between employment and net migration is broadly stable across provinces, although its magnitude varies locally. Several other covariates show substantially greater territorial heterogeneity, including changes in magnitude and, in some areas, in sign.

---

## 📁 Repository Structure

```plaintext
├── Figures/                              # Figures for exploration, interpretation, and robustness analyses
│   │
│   ├── Explorative_Analysis/             # Exploratory analysis and motivation of modelling choices
│   │   └── README.md
│   │
│   ├── M1/                               # Global spatio-temporal backbone model
│   │   ├── ...                           # M1 interpretative figures
│   │   └── README.md
│   │
│   ├── M1_M6_Comparison/                 # Robustness comparison between territorial specifications
│   │   ├── m1_m6_coefficients.png        # Comparison of national covariate associations
│   │   └── README.md
│   │
│   ├── M3/                               # Partitioned overlap model: local territorial heterogeneity
│   │   ├── M3_hyperparameters_dependence.png
│   │   ├── M3_hyperparameters_variance.png
│   │   ├── M3_local_effects_first5_mean.png
│   │   ├── M3_local_effects_first5_sign.png
│   │   ├── M3_local_effects_second5_mean.png
│   │   ├── M3_local_effects_second5_sign.png
│   │   └── README.md
│   │
│   └── README.md                         # Overview and interpretation guide for all figure folders
│
├── Poster.pdf                            # Final A1 scientific poster
├── main_poster.tex                       # LaTeX source code for the poster
└── README.md                             # Main project documentation
```

---


## 💻 Computational Environment & Requirements

The scripts and models are executed in **R (>= 4.4)**. Key packages include:
* **Bayesian inference**: `INLA` ([R-INLA project](https://www.r-inla.org/))
* **Spatial data & GIS**: `sf`, `giscoR`, `spdep`
* **Data wrangling & Visualization**: `tidyverse` (`dplyr`, `tidyr`, `ggplot2`), `patchwork`, `scales`, `RColorBrewer`
* **MCMC baseline**: Custom C++/R Metropolis-within-Gibbs sampler

---

## 📚 References

* **Mozdzen, P. et al.** (2022). *Bayesian modeling and clustering for spatio-temporal areal data: An application to Italian unemployment.* **Spatial Statistics**, 52, 100715.
* **Orozco-Acosta, E., Adin, A., & Ugarte, M. D.** (2021). *Scalable Bayesian modeling for smoothing disease risks in large spatial data sets using INLA.* **Spatial Statistics**, 41, 100496.
* **Orozco-Acosta, E., Adin, A., & Ugarte, M. D.** (2023). *Big problems in spatio-temporal disease mapping: methods and software.* **Computer Methods and Programs in Biomedicine**, 231, 107403.
* **Rue, H., Martino, S., & Chopin, N.** (2009). *Approximate Bayesian inference for latent Gaussian models by using integrated nested Laplace approximations.* **JRSS-B**, 71(2), 319–392.

---

## 📬 Contacts

For inquiries regarding the code, methodology, or results:
* **Claudia Franceschini**: [claudia.franceschini@phd.unipd.it](mailto:claudia.franceschini@phd.unipd.it)
* **Sophie Grace Parolin**: [sophiegrace.parolin@phd.unipd.it](mailto:sophiegrace.parolin@phd.unipd.it)
* **Teresa Secco**: [teresa.secco@unive.it](mailto:teresa.secco@unive.it)
