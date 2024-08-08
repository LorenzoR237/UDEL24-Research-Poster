# UDEL24-Research-Poster


Excitation and Emission Matrix(EEM) dataset

Website:
Relevant Papers:
1. Bro, R, Multi-way Analysis in the Food Industry. Models, Algorithms, and Applications. 1998. Ph.D. Thesis, University of Amsterdam (NL) & Royal Veterinary and Agricultural University (DK).

2. Kiers, H.A.L. (1998) A three-step algorithm for Candecomp/Parafac analysis of large data sets with multicollinearity, Journal of Chemometrics, 12, 155-171.

using CairoMakie, GCPDecompositions, LinearAlgebra


Load data

using MAT, ZipFile

Here the tensor data is extracted from the "data" variable, which is inside X in the original data set, and it's named X.

X = vars["X"]["data"]

Here we extract the 18x3 matrix found in the "mixtures" variable from the original data set.

mixtures = vars["mixtures"]

Mode ranges for y and x axis labels.

emissions_wavelength = vec(vars["mode_ranges"][1, 2])

excitations_wavelength = vec(vars["mode_ranges"][1, 3])

EMM data graphs
![image](https://github.com/user-attachments/assets/4323c9eb-51e5-45e0-8dbc-3c8e10b68522)

Plots for each Samples' fluorescence landscape
![image](https://github.com/user-attachments/assets/3c695cf6-e8d3-4e82-bd2a-f101d17ec33d)

Run CP Decomposition 

Decomposition

Graph all the components with all their elements

M = gcp(X, 3)

Plot the (normalized) factors.

component_order = [1,3,2]

![image](https://github.com/user-attachments/assets/f3d26421-bf1b-4cd7-a893-ae96860007fd)




