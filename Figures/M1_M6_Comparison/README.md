# M1 vs M6: Coefficient Comparison

This figure compares estimated covariate associations with municipal net
migration rates in Italy over 2004–2019 under two alternative representations
of territorial and temporal heterogeneity:

- **M1_RW** combines a national RW1 temporal component with a municipal
  spatio-temporal latent field based on the adjacency structure, whose evolution
  is governed by an AR(1) process.
- **M6_RW** replaces the municipal spatio-temporal field with region and province
  random intercepts, together with national RW1 and scalar AR(1) temporal
  components.

Both models estimate a common set of covariate coefficients across all
municipalities.

![Coefficient comparison](m1_m6_coefficients.png)

## How to read the figure

Points represent posterior means and horizontal lines show equal-tailed
95% credible intervals. The dashed vertical line marks zero.

The response and continuous covariates are based on the same global
standardization in both models. Each coefficient therefore represents the
conditional association between a one-standard-deviation difference in a
covariate and the net migration rate, expressed in standard-deviation units,
holding the other model components fixed.

## Main findings

Several of the main associations are qualitatively stable across the two
model specifications. In particular, higher employment is positively
associated with net migration, whereas greater travel time to essential
services is negatively associated with it. The estimated directions are also
consistent across specifications for several other covariates.

The magnitude of some associations is nevertheless sensitive to model
specification. The clearest example is population density growth, whose
estimated positive association is substantially larger under M6_RW.
Estimates for unemployment and GDP growth are also relatively close to zero
and more dependent on model specification.

Overall, the comparison suggests that a number of the main national
associations are robust to a substantial change in the representation of
territorial and temporal heterogeneity, while the estimated magnitude and
uncertainty of some coefficients remain model-dependent.

Because M1_RW and M6_RW differ jointly in their latent territorial and temporal
structures, differences between coefficient estimates cannot be attributed
solely to the replacement of the municipal adjacency-based field with
administrative random effects.
