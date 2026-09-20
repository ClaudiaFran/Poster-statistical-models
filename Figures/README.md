# Figures

This directory contains the main figures related to the exploratory analysis,
model interpretation, robustness checks, and local territorial heterogeneity in the
Italian municipal migration study.

---

## Directory structure

### `Explorative_Analysis/`

Contains figures used to describe the data before model fitting and to motivate the
main modelling choices.

These figures are intended as **descriptive support for the modelling strategy**.

---

### `M1/`

Contains figures from the final refit of the global spatio-temporal backbone model
over the full period 2004–2019.

M1 combines:

- common national covariate coefficients;
- a national RW1 temporal component;
- a municipal spatio-temporal latent field;
- spatial dependence based on the municipal adjacency graph;
- AR(1) temporal persistence of the latent spatial field.

See the local `README.md` in the folder for figure-specific descriptions.

---

### `M1_M6_Comparison/`

Contains figures comparing national covariate associations under two different
territorial and temporal specifications:

- **M1_RW**, combining a national RW1 temporal component with a municipal
  spatio-temporal latent field based on the adjacency graph and evolving through
  an AR(1) process;
- **M6_RW**, replacing the municipal spatio-temporal field with region and
  province random intercepts, together with national RW1 and scalar AR(1)
  temporal components.

Both models estimate common covariate coefficients across municipalities.

The purpose of this comparison is to assess whether the main national associations
are robust to a substantial change in the representation of territorial and temporal
heterogeneity.

The comparison shows that several main associations retain the same direction across
models, while the magnitude and uncertainty of some effects remain model-dependent.

See the local `README.md` for a detailed interpretation.

---

### `M3/`

Contains figures from the final 2004–2019 refit of the partitioned overlap model.

M3 estimates local province-level models while including neighbouring municipalities
from adjacent provinces during fitting in order to reduce artificial boundary
effects. Only estimates corresponding to the core province are retained in the final
national reconstruction.

The figures describe:

- spatial variation in the dependence parameter `rho`;
- spatial variation in temporal persistence `xi`;
- local variance and precision parameters;
- province-specific posterior means of covariate effects;
- the posterior credibility of the local sign of each association.

These figures are used to study **territorial heterogeneity in covariate
associations**, complementing the common national coefficients estimated by M1.

See the local `README.md` in the folder for a detailed interpretation of each figure.

---

## How the figure folders relate to the research questions

The figure structure mirrors the main analytical goals of the project:

1. **National covariate associations**  
   Interpreted primarily through M1 and checked for robustness against M6.

2. **Spatial and temporal structure**  
   Interpreted primarily through the municipal spatio-temporal field in M1, with
   M6 serving as a simpler administrative benchmark with national temporal
   components.

3. **Territorial heterogeneity in the associations**  
   Investigated through M3, where regression coefficients and spatio-temporal
   parameters are allowed to vary locally.

---

## Folder summary

| Folder | Main purpose |
|---|---|
| `Explorative_Analysis/` | Data exploration |
| `M1/` | Interpretation of national associations and global spatio-temporal structure |
| `M1_M6_Comparison/` | Robustness of national coefficients to territorial and temporal specification |
| `M3/` | Local heterogeneity in effects and spatio-temporal parameters |
