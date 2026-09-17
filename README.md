# Bayesian Spatio-Temporal Modeling of Net Migration across Italian Municipalities
### Associations, Spatio-Temporal Structure, and Territorial Heterogeneity

[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-blue?logo=github)](https://github.com/ClaudiaFran/Poster-statistical-models)
[![R](https://img.shields.io/badge/Language-R-276DC3?logo=r)](https://www.r-project.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Authors:**
* **Claudia Franceschini** &nbsp;([claudia.franceschini@phd.unipd.it](mailto:claudia.franceschini@phd.unipd.it))
* **Sophie Grace Parolin** &nbsp;([sophiegrace.parolin@phd.unipd.it](mailto:sophiegrace.parolin@phd.unipd.it))
* **Teresa Secco** &nbsp;([teresa.secco@phd.unipd.it](mailto:teresa.secco@phd.unipd.it))

*Department of Statistical Sciences, University of Padua*  
*Course in Statistical Models — Ph.D. in Statistical Sciences*

---

## 📌 Project Overview

This project investigates the spatio-temporal dynamics of **municipal net migration in Italy** using a nationwide panel covering 16 consecutive years:
* **$S = 7{,}896$ municipalities** (harmonized according to 2024 administrative borders)
* **$T = 16$ years** ($2004$–$2019$)
* **$N = 126{,}336$ municipality–year observations**

The response variable is the **standardized net migration rate per 1,000 residents**:
$$y_{st} = 1{,}000 \times \frac{\text{Net Migration}_{st}}{\text{Resident Population}_{st}}$$

The study investigates:
1. Which demographic, economic, and territorial factors drive internal and international migration flows.
2. The degree of spatial autocorrelation and temporal persistence at the granular municipal scale.
3. Whether a single national model suffices or if territorial partitioning reveals meaningful local heterogeneity.

---

## 🔬 Methodological Framework

### 1. Areal Topology & Spatial Precision
Spatial connectivity is represented via a binary contiguity graph $W$ on harmonized municipal borders ($|V| = 7{,}896$, $|E| = 22{,}609$ undirected edges; average degree $\bar{d} = 5.73$ neighbors; 14 insular municipalities kept as natural isolated nodes). Spatial precision is parameterized through the **Leroux CAR** specification:
$$Q_S(\rho) = \rho(D - W) + (1 - \rho)I_S, \qquad \rho \in (0,1)$$

### 2. Spatio-Temporal Latent Field
Temporal dependence is modeled as a first-order autoregressive process AR(1) on the spatial field $\mathbf{w}_t$:
$$\mathbf{w}_t \mid \mathbf{w}_{t-1} \sim \mathcal{N}\left(\xi \mathbf{w}_{t-1}, \, \tau_{ST}^2 Q_S(\rho)^{-1}\right), \qquad |\xi| < 1$$
yielding a separable Kronecker joint precision matrix:
$$Q_{ST}(\rho, \xi, \tau_{ST}^2) = \frac{1}{\tau_{ST}^2} Q_T(\xi) \otimes Q_S(\rho)$$

### 3. Model Benchmark
We evaluate and compare six competing modeling strategies:
* **M1 (INLA Global)**: National Leroux CAR $\times$ AR(1) spatio-temporal model fitted via Integrated Nested Laplace Approximations.
* **M2 (MCMC Global)**: Matched specification estimated via custom Metropolis-within-Gibbs sampling (adapted from Mozdzen et al., 2022).
* **M3 (Partitioned Overlap INLA)**: 107 province-specific submodels including first-order neighbors to minimize edge boundary bias (Orozco-Acosta et al., 2023).
* **M4 (Partitioned Disjoint INLA)**: 107 province-specific disjoint models.
* **M5 (BSTC-DP Clustering)**: Bayesian nonparametric Dirichlet Process clustering on municipal parameters $\bm{\phi}_s = (\bm{\beta}_s, \xi_s)$.
* **M6 (Administrative Hierarchical Baseline)**: Regional and provincial IID random effects with national AR(1) trend.

---

## 📊 Key Results & Findings

### Model Comparison (One-Year-Ahead Rolling Origin Out-of-Sample)
Evaluated over $2018$ and $2019$ out-of-sample test periods ($N_{\text{test}} = 15{,}792$):

| Model | Specification | RMSE | MAE | $\text{Cov}_{95}$ | $\text{IS}_{95}$ | Computing Time |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **M1** | **INLA Global** | **13.69** | **8.81** | 0.961 | 84.65 | ~19 min |
| **M2** | **MCMC Global** | 13.69 | 8.82 | 0.961 | 84.63 | ~5.8 hrs |
| **M3** | **Overlap INLA** | 13.96 | 9.09 | 0.962 | **77.09** | 107 parallel jobs |
| **M4** | **Disjoint INLA** | 14.20 | 9.34 | 0.962 | 77.60 | 107 parallel jobs |
| **M5** | **BSTC-DP** | 17.18 | 10.83 | 0.931 | 99.30 | ~6.7 hrs |
| **M6** | **Admin IID** | 13.77 | 8.90 | **0.972** | 88.33 | ~1 min |

* $\text{IS}_{95}$: Interval Score at 95% (lower indicates sharper, well-calibrated prediction intervals).

### Substantive Insights
* **Pervasive spatio-temporal structure**: Very strong spatial dependence ($\hat{\rho} \approx 0.964$) and high temporal persistence ($\hat{\xi} \approx 0.696$).
* **Labor market & urban pull**: Employment rate is the primary economic attractant ($\hat{\beta} = +0.250$), followed by population density ($\hat{\beta} = +0.074$).
* **Territorial repulsion & vulnerability**: Essential service accessibility deficit ($\hat{\beta} = -0.053$), foreign share fluctuations ($\hat{\beta} = -0.042$), and unemployment ($\hat{\beta} = -0.026$) drive net outflows.
* **Macroeconomic shock**: A persistent downward structural shift followed the 2008 financial crisis ($\hat{\gamma}_{2008} = -0.416$).
* **Local heterogeneity (M3)**: Local coefficients highlight substantial geographic divergence across northern, central, and southern Italian provinces.

---

## 📁 Repository Structure

```plaintext
├── Poster_figures/                # Final figures formatted for the scientific poster
│   ├── raw_migration_map_comuni_en.png   # Observed municipal rate maps (2004, 2007, 2012, 2017)
│   ├── m1_rw1_coefficients_intervals.png # Fixed effects posterior credible intervals
│   ├── m1_latent_field_municipal.png     # Annual municipal latent field w_t
│   ├── M3_selected_local_effects_5x1.png # Province-level local effect maps
│   ├── covariate_correlations.png        # Predictors correlation matrix
│   └── variance_decomposition.png        # Variance components breakdown
├── Figures/                       # Complete set of descriptive and exploratory figures (Italian)
│   ├── 01_trend_reddito_regioni.png      # Regional per-capita taxable income trends
│   ├── 08_mappa_tasso_regioni.png        # Regional net migration rate maps
│   ├── 09_mappa_tasso_province.png       # Provincial net migration rate maps
│   ├── 10_mappa_tasso_comuni.png         # Municipal net migration rate maps
│   ├── 11_boxplot_tasso_urbanizzazione.png # Rates by DEGURBA urbanization class
│   └── 17_moran_tasso_migratorio.png     # Global Moran's I temporal trajectory
├── Code/                          # R analysis and modeling scripts
│   ├── 01_main.R                         # Data cleaning, harmonisation, and feature engineering
│   ├── 02_descr_final.R                  # Full exploratory and spatial descriptive pipeline
│   └── 03_stationarity.R                 # Panel unit root tests (CADF & CIPS bootstrap)
├── main_01.tex                    # LaTeX source code for the A1 Beamer poster
└── README.md                      # Project documentation
```

---

## 💻 Software & Reproducibility Requirements

The analyses were developed in **R (>= 4.4)** using:
* **Spatial modeling & inference**: `INLA` ([R-INLA project](https://www.r-inla.org/)), `sf`, `giscoR`, `spdep`
* **Data manipulation & graphics**: `tidyverse` (`dplyr`, `tidyr`, `ggplot2`), `patchwork`, `scales`, `RColorBrewer`
* **MCMC sampling**: Custom C++/R routines for Metropolis-within-Gibbs sampling

---

## 📚 References

* **Mozdzen, P. et al.** (2022). *Bayesian modeling and clustering for spatio-temporal areal data: An application to Italian unemployment.* **Spatial Statistics**, 52, 100715.
* **Orozco-Acosta, E., Adin, A., & Ugarte, M. D.** (2021). *Scalable Bayesian modeling for smoothing disease risks in large spatial data sets using INLA.* **Spatial Statistics**, 41, 100496.
* **Orozco-Acosta, E., Adin, A., & Ugarte, M. D.** (2023). *Big problems in spatio-temporal disease mapping: methods and software.* **Computer Methods and Programs in Biomedicine**, 231, 107403.
* **Rue, H., Martino, S., & Chopin, N.** (2009). *Approximate Bayesian inference for latent Gaussian models by using integrated nested Laplace approximations.* **JRSS-B**, 71(2), 319–392.

---

## 📬 Contacts

For inquiries, collaborations, or data requests:
* Claudia Franceschini: [claudia.franceschini@phd.unipd.it](mailto:claudia.franceschini@phd.unipd.it)
* Sophie Grace Parolin: [sophiegrace.parolin@phd.unipd.it](mailto:sophiegrace.parolin@phd.unipd.it)
* Teresa Secco: [teresa.secco@phd.unipd.it](mailto:teresa.secco@phd.unipd.it)
