# Figures

This directory contains the main figures used to document the exploratory analysis,
model interpretation, robustness checks, and local territorial heterogeneity for the
Italian municipal migration study.

The figures are organised by analytical purpose and model specification.

---

## Directory structure

### `Explorative_Analysis/`

Contains figures used to describe the data before model fitting and to motivate the
main modelling choices.

Typical contents include exploratory summaries of the response and covariates,
temporal patterns, spatial structure, and checks used to motivate the treatment of
non-stationarity and the inclusion of flexible temporal components.

These figures are intended as **descriptive support for the modelling strategy**,
rather than as inferential results.

---

### `M1/`

Contains figures from the final refit of the global spatio-temporal backbone model
over the full period 2004–2019.

M1 combines:

- common national covariate coefficients;
- a national temporal component;
- a municipal spatio-temporal latent field;
- spatial dependence based on the municipal adjacency graph;
- temporal persistence of the latent spatial field.

The figures in this folder are used to interpret the main national associations and
the global spatio-temporal structure.

See the local `README.md` in the folder for figure-specific descriptions.

---

### `M1_M6_Comparison/`

Contains robustness figures comparing the national covariate associations estimated
under two different territorial specifications:

- **M1_RW**, based on a municipal spatio-temporal latent field;
- **M6_RW**, based on region and province random intercepts and national temporal
  components.

Both models estimate common covariate coefficients across municipalities.

The purpose of this comparison is not to identify a single preferred model, but to
assess whether the main national associations are robust to a substantial change in
the way territorial heterogeneity is represented.

The comparison shows that several main directions are stable across models, while
the magnitude and uncertainty of some associations remain model-dependent.

See the local `README.md` for the detailed interpretation.

---

### `M3/`

Contains figures from the final 2004–2019 refit of the partitioned overlap model.

M3 estimates local province-level models while including neighbouring municipalities
from adjacent provinces during fitting in order to reduce artificial boundary
effects. Only estimates corresponding to the core province are retained for the final
national reconstruction.

The figures describe:

- spatial variation in the dependence parameter `rho`;
- spatial variation in temporal persistence `xi`;
- local variance and precision parameters;
- province-specific posterior means of covariate effects;
- the posterior credibility of the local sign of each association.

These figures are used to study **territorial heterogeneity in the covariate
associations**, complementing the common national coefficients estimated by M1.

See the local `README.md` in the folder for a detailed interpretation of each figure.

---

## How the figure folders relate to the research questions

The figure structure mirrors the main analytical goals of the project:

1. **National covariate associations**  
   Interpreted primarily through M1 and checked for robustness against M6.

2. **Spatial and temporal structure**  
   Interpreted through the global spatio-temporal field in M1, with M6 serving as a
   simpler administrative benchmark.

3. **Territorial heterogeneity in the associations**  
   Investigated through M3, where regression coefficients and spatio-temporal
   parameters are allowed to vary locally.

The out-of-sample comparison is used to compare alternative model specifications on
a common predictive basis. For interpretation, the relevant models are then refitted
using the full 2004–2019 period.

---

## Interpretation notes

All reported covariate effects should be interpreted as **conditional associations**,
not as causal effects.

Posterior means describe the estimated direction and magnitude of an association,
while 95% credible intervals quantify posterior uncertainty.

For local M3 results, a positive or negative posterior mean does not by itself imply
a credibly non-zero effect; the corresponding credible-sign maps should therefore be
used to assess whether the 95% credible interval excludes zero.

Differences between model specifications should not be attributed to a single model
component in isolation, because the latent territorial and temporal structures differ
jointly across models.

---

## Folder summary

| Folder | Main purpose |
|---|---|
| `Explorative_Analysis/` | Data exploration and motivation of modelling choices |
| `M1/` | Interpretation of national associations and global spatio-temporal structure |
| `M1_M6_Comparison/` | Robustness of national coefficients to territorial specification |
| `M3/` | Local heterogeneity in effects and spatio-temporal parameters |

