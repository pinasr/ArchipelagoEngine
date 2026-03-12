# Build Archipelagic Spatial Weights

Bridges fragmented island networks using K-Nearest Neighbors (KNN) to
ensure 100% connectivity (nc=1). This prevents the "orphaning" of island
units common in standard Queen-contiguity models.

## Usage

``` r
build_archipelago_weight(p_map, k = 5)
```

## Arguments

- p_map:

  An `sf` object containing the geographic boundaries.

- k:

  Integer. Number of neighbors. Default is 5, optimized for Philippine
  archipelagic connectivity.

## Value

A `listw` object compatible with spatial regression models.

## Details

Standard Queen-contiguity models inherently fail in archipelagic
settings. In the Philippine context, Queen logic leaves 16 provinces
(approx. 20%) mathematically isolated, resulting in a fragmented network
with only 80.2% connectivity.

This fragmentation introduces systematic predictive bias, evidenced by
significant Residual Spatial Autocorrelation (Moran's I = 0.024, p \<
0.05) and a higher AIC (201.896).

By enforcing a unified grid (k=5), this function achieves:

- 100% Network Connectivity (nc=1)

- Neutralized Spatial Bias (Moran's I approx. 0, p \> 0.10)

- Robust Spatial Spillovers (Lambda stable at ~0.26)

While the Queen model may appear to have a "tighter" fit
(Log-Likelihood: -96.948), the KNN (k=5) specification (Log-Likelihood:
-97.472) is prioritized for structural robustness and randomized
residuals.

## Examples

``` r
# \donttest{
  # Example: Ensuring 100% connectivity for 81 provinces
  weights <- build_archipelago_weight(raw_data, k = 5)
  spdep::n.comp.nb(weights$neighbours)$nc
#> [1] 1
# }
```
