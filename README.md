# UDEL24-Research-Poster

Under construction.../._./

## Excitation and Emission Matrix(EEM) dataset

Website:
Relevant Papers:
1. Bro, R, Multi-way Analysis in the Food Industry. Models, Algorithms, and Applications. 1998. Ph.D. Thesis, University of Amsterdam (NL) & Royal Veterinary and Agricultural University (DK).

2. Kiers, H.A.L. (1998) A three-step algorithm for Candecomp/Parafac analysis of large data sets with multicollinearity, Journal of Chemometrics, 12, 155-171.

```Julia
using CairoMakie, GCPDecompositions, LinearAlgebra
```

## Load data

```Julia
using MAT, ZipFile
```

Here the tensor data is extracted from the "data" variable, which is inside X in the original data set, and it's named X.

```Julia
X = vars["X"]["data"]
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


Food and Feed dataset

Website:

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



