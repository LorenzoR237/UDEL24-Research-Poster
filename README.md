# UDEL24-Research-Poster


Excitation and Emission Matrix(EEM) dataset

Website:
Relevant Papers:
1. Bro, R, Multi-way Analysis in the Food Industry. Models, Algorithms, and Applications. 1998. Ph.D. Thesis, University of Amsterdam (NL) & Royal Veterinary and Agricultural University (DK).

2. Kiers, H.A.L. (1998) A three-step algorithm for Candecomp/Parafac analysis of large data sets with multicollinearity, Journal of Chemometrics, 12, 155-171.

using CairoMakie, GCPDecompositions, LinearAlgebra


Load data

using MAT, ZipFile
X = vars["X"]["data"]


Here the tensor data is extracted from the "data" variable, which is inside X in the original data set, and it's named X.


Run CP Decomposition 
   
![image](https://github.com/user-attachments/assets/4323c9eb-51e5-45e0-8dbc-3c8e10b68522)
