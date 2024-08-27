# UDEL24-Research-Poster

Under construction.../._./

## Excitation and Emission Matrix(EEM) dataset

Website: https://gitlab.com/tensors/tensor_data_eem/-/blob/master/README.md?ref_type=heads

Relevant papers:
1. Bro, R, Multi-way Analysis in the Food Industry. Models, Algorithms, and Applications. 1998. Ph.D. Thesis, University of Amsterdam (NL) & Royal Veterinary and Agricultural University (DK).

2. Kiers, H.A.L. (1998) A three-step algorithm for Candecomp/Parafac analysis of large data sets with multicollinearity, Journal of Chemometrics, 12, 155-171.

```Julia
using CairoMakie, GCPDecompositions, LinearAlgebra
```

#### Load data

```Julia
using MAT, ZipFile
```

```Julia
vars = matread("data/EEM18.mat")
```

Here the tensor data is extracted from the "data" variable, which is inside X in the original data set, and it's named X.

```Julia
X = vars["X"]["data"]
```

```
18×251×21 Array{Float64, 3}:
[:, :, 1] =
 0.0     0.0      0.0      0.0      0.0   …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0   1350.8   4052.41  1350.8      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0   1298.17     0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0   1234.07     0.0   1234.07  …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0   1234.07     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 ⋮                                        ⋱            ⋮                        ⋮
 0.0  1189.99     0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0   …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0     0.0      0.0      0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0

[:, :, 2] =
    0.0      0.0     0.0      0.0   …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0  1366.24     0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 1347.78     0.0     0.0   1347.78     0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0     0.0   1295.27     0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0  1246.7      0.0   …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0   1246.7     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    ⋮                               ⋱            ⋮                        ⋮
    0.0      0.0     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0   1246.7     0.0   1262.48  …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 3740.09     0.0     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
    0.0      0.0     0.0      0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0

[:, :, 3] =
 2.50356e-6      0.0            0.0   …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.000341868  2697.12           0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.00628395      0.0            0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.00522428   1295.33           0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.00601748      0.0            0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.00382403      0.0            0.0   …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.00603934      0.0            0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 ⋮                                    ⋱            ⋮                        ⋮
 0.00232087      0.0            0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.00374822      0.0            0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0             0.00796642     0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0            72.386       1246.14  …  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0             0.00784822     0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0
 0.0             0.00924415     0.0      0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0

;;; … 

[:, :, 19] =
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …   43.3776    23.7197   159.346
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     348.447    179.241    418.359
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       4.78182    4.48782  345.825
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     603.647    519.02     638.992
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     172.288    166.943    366.461
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …  416.474    277.469    388.979
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     367.36     447.365    447.125
 ⋮                        ⋮                        ⋱                          ⋮
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     143.919    198.241    294.625
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     128.531    238.501    226.199
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     111.755    142.27       3.09878
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …    2.81992    2.64655    2.5393
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       4.25314   71.8342     3.82989
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      90.052    209.817    134.954

[:, :, 20] =
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …    34.4047    156.205      279.368
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      142.705     165.535      256.37
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0        1.74505   208.235      209.822
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     1339.61     1445.0       1318.32
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       35.9479    188.938      276.294
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …   231.219     222.272      323.343
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      236.223     291.023      298.169
 ⋮                        ⋮                   ⋱                              ⋮
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       10.7751    128.964      211.017
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       55.1063    170.8        297.401
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       96.656      69.9302      36.8467
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …     1.02908     0.965814     0.926676
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0        1.55212     1.45669     29.2565
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      110.892     124.965      197.434

[:, :, 21] =
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …  128.713     108.887     117.084
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      50.0096     46.9349     27.7495
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     167.062     135.384      21.2541
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     573.729     411.924     445.6
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      38.4153    159.256     170.073
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …  126.287     170.359     172.22
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     300.499     176.47      284.644
 ⋮                        ⋮                   ⋱                            ⋮
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      25.6345     27.7242     55.1368
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     102.031     290.709     209.779
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0      55.1544    157.015     118.178
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  …    0.360633   97.3156    170.676
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0       0.543924    0.510482  179.47
 0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0  0.0     196.964     282.334     335.865
```

Here we extract the 18x3 matrix found in the "mixtures" variable from the original data set.

```Julia
mixtures = vars["mixtures"]
```

Mode ranges for y and x axis labels.

```Julia
emissions_wavelength = vec(vars["mode_ranges"][1, 2])

excitations_wavelength = vec(vars["mode_ranges"][1, 3])
```

EMM data graphs
![image](https://github.com/user-attachments/assets/4323c9eb-51e5-45e0-8dbc-3c8e10b68522)

Plots for each Samples' fluorescence landscape
![image](https://github.com/user-attachments/assets/3c695cf6-e8d3-4e82-bd2a-f101d17ec33d)

Run CP Decomposition 

```Julia
M = gcp(X, 3)
```

Plot the (normalized) factors.

![image](https://github.com/user-attachments/assets/f3d26421-bf1b-4cd7-a893-ae96860007fd)


## Food and Feed dataset

Website: https://www.kaggle.com/datasets/dorbicycle/world-foodfeed-production

Relevant papers: 

1. Bro, R, Multi-way Analysis in the Food Industry. Models, Algorithms, and Applications. 1998. Ph.D. Thesis, University of Amsterdam (NL) & Royal Veterinary and Agricultural University (DK).

2. Kiers, H.A.L. (1998) A three-step algorithm for Candecomp/Parafac analysis of large data sets with multicollinearity, Journal of Chemometrics, 12, 155-171.

using BenchmarkTools, GCPDecompositions, DataFrames, DelimitedFiles, CSV, XLSX, CairoMakie, LinearAlgebra, Makie, GeometryBasics, NaturalEarth, LibGEOS, GADM, ColorSchemes, Colors

Load data

```Julia
FAO_dataset = CSV.read("data/FAO.csv", DataFrame)
```

Create tensor

FAO_tensor

Run CP Decomposition 

```Julia
FAO_M = gcp(FAO_tensor, 3, loss = GCPLosses.NonnegativeLeastSquares(), algorithm = GCPAlgorithms.LBFGSB(;maxfun=500000, maxiter=500000, pgtol = 1e-7))
```

Graph all the components with all their (normalized factors).
![image](https://github.com/user-attachments/assets/9ed58c42-25e1-45c2-a890-09769be2b108)



