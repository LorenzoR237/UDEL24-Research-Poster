# UDEL24-Research-Poster

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

```
18×3 Matrix{Float64}:
 5.0   0.0   0.0
 0.0   5.0   0.0
 0.0   0.0   5.0
 1.25  5.0   3.75
 3.75  1.25  5.0
 5.0   3.75  2.5
 3.75  3.75  5.0
 ⋮           
 2.5   3.75  1.25
 3.75  0.0   2.5
 2.5   0.0   3.75
 5.0   0.0   1.25
 3.75  0.0   3.75
 3.75  0.0   5.0
```

#### Mode ranges for y and x axis labels.

```Julia
emissions_wavelength = vec(vars["mode_ranges"][1, 2])
```
```
emissions_wavelength = Float64[
250.0
251.0
252.0
253.0
254.0
255.0
256.0
257.0
258.0
259.0
260.0
261.0
262.0
263.0
264.0
265.0
266.0
267.0
268.0
269.0
more
491.0
492.0
493.0
494.0
495.0
496.0
497.0
498.0
499.0
500.0
]
```

```Julia
excitations_wavelength = vec(vars["mode_ranges"][1, 3])
```

```
excitations_wavelength = Float64[
210.0
215.0
220.0
225.0
230.0
235.0
240.0
245.0
250.0
255.0
260.0
265.0
270.0
275.0
280.0
285.0
290.0
295.0
300.0
305.0
310.0
]
```

#### EMM data graphs
```Julia
with_theme() do
	
	fig = Figure(size = (1150, 800))

	# Plot contour maps
	for idx in 1:size(X, 1)
		row, col = fldmod1(idx, 6)
		contourf(fig[row, col],
			excitations_wavelength, emissions_wavelength, permutedims(X[idx, :, :]), levels = collect(range(-1.1e4, 7e5, 20)), 
			colormap = :deep;
			axis = (;
				title = "Mix $(mixtures[idx, :])",
				xlabel = "excitation (nm)",
				ylabel = "emission (nm)",
				xticks = 220:40:300,
			
			
			)
		)
	end

	Colorbar(fig[:,7]; label = "Fluorescence Intensity", height = Relative(1), colormap = :deep)


	
	# title
	Label(fig[0, :], "EEM Data Graphs"; fontsize = 25)

	fig
end
```
![image](https://github.com/user-attachments/assets/4323c9eb-51e5-45e0-8dbc-3c8e10b68522)

#### Plots for each Samples' fluorescence landscape
```Julia
with_theme() do
    fig = Figure(; size=(2500, 1100))

    # Loop through samples
    for idx in 1:size(X,1)
        ax = Axis3(fig[fldmod1(idx, 6)...]; title="Mix $(mixtures[idx, :])",
            xlabel="emisson (nm)", xticks=250:100:450,
            ylabel="excitation (nm)", yticks=240:30:300,
            zlabel=""
        )
        surface!(ax, emissions_wavelength, excitations_wavelength, X[idx,:,:], colormap = :deep)
		    resize_to_layout!(fig)
    end

    rowgap!(fig.layout, 20)
    colgap!(fig.layout, 30)
    fig
end
```
![image](https://github.com/user-attachments/assets/3c695cf6-e8d3-4e82-bd2a-f101d17ec33d)

#### Run CP Decomposition 
```Julia
M = gcp(X, 3)
```
```
18×251×21 CPD{Float64, 3, Vector{Float64}, Matrix{Float64}} with 3 components
λ weights:
3-element Vector{Float64}: …
U[1] factor matrix:
18×3 Matrix{Float64}: …
U[2] factor matrix:
251×3 Matrix{Float64}: …
U[3] factor matrix:
21×3 Matrix{Float64}: …
```

#### Plot the (normalized) factors.
```Julia
with_theme() do
    fig = Figure()

    # Plot factors (normalized by max)
    for row in 1:ncomponents(M)
		#Concentration of Tryptophan in each sample
        barplot(fig[row,1], 1:size(X,1), normalize(M.U[1][:,component_order[row]], 				Inf), color = :orange, axis=(;xticks=0:3:18))
		#Concentration of Tryptophan in each sample
        lines(fig[row,3], emissions_wavelength, normalize(M.U[2][:,component_order[row]], Inf), color = :orange, axis=(;xticks=250:50:500))
        lines(fig[row,2], excitations_wavelength, normalize(M.U[3][:,component_order[row]], Inf), color = :orange, axis=(;xticks=210:25:310))
    end

	
	
    # Link and hide x axes
    linkxaxes!(contents(fig[:,1])...)
    linkxaxes!(contents(fig[:,2])...)
    linkxaxes!(contents(fig[:,3])...)
    hidexdecorations!.(contents(fig[1:2,:]); ticks=false, grid=false)
	

    # Link and hide y axes
    linkyaxes!(contents(fig.layout)...)
    hideydecorations!.(contents(fig.layout); ticks=false, grid=false)

    # Add labels
    Label(fig[0,1], "Samples"; tellwidth=false, fontsize=15, font=:bold)
    Label(fig[0,2], "Excitation"; tellwidth=false, fontsize=15, font=:bold)
    Label(fig[0,3], "Emission"; tellwidth=false, fontsize=15, font=:bold)

	Label(fig[1,0], "Component 1"; tellheight=false, font = :bold, fontsize=15, rotation = pi/2)
    Label(fig[2,0], "Component 2"; tellheight=false, font = :bold, fontsize=15, 		rotation = pi/2)
    Label(fig[3,0], "Component 3"; tellheight=false, font = :bold, fontsize=15, 			rotation = pi/2)

    fig
end
```

![image](https://github.com/user-attachments/assets/f3d26421-bf1b-4cd7-a893-ae96860007fd)


## Food and Feed dataset

Website: https://www.kaggle.com/datasets/dorbicycle/world-foodfeed-production

Relevant papers: 

1. Bro, R, Multi-way Analysis in the Food Industry. Models, Algorithms, and Applications. 1998. Ph.D. Thesis, University of Amsterdam (NL) & Royal Veterinary and Agricultural University (DK).

2. Kiers, H.A.L. (1998) A three-step algorithm for Candecomp/Parafac analysis of large data sets with multicollinearity, Journal of Chemometrics, 12, 155-171.

using BenchmarkTools, GCPDecompositions, DataFrames, DelimitedFiles, CSV, XLSX, CairoMakie, LinearAlgebra, Makie, GeometryBasics, NaturalEarth, LibGEOS, GADM, ColorSchemes, Colors

#### Load data

```Julia
FAO_dataset = CSV.read("data/FAO.csv", DataFrame)
```
|       | Area Abbrevation | Area Code |     Area      | Item Code |          Item            | Element Code | Element |    Unit     | ... more|
| :---: |      :---:       |   :---:   |     :---:     |  :---:    |          :---:           |    :---:     |  :---:  |    :---:    |  :---:  |
|1      |"AFG"             |     2     | "Afghanistan" |2848       |   "Wheat and products"   |5142          | "Food"  |"1000 tonnes"|         |
|2      |"AFG"             |     2     | "Afghanistan" |2848       |"Rice (Milled Equivalent)"|5142          | "Food"  |"1000 tonnes"|         |
|3      |"AFG"             |     2     | "Afghanistan" |2761       |  "Barley and products"   |5521          | "Feed"  |"1000 tonnes"|         |
|4      |"AFG"             |     2     | "Afghanistan" |2680       |  "Barley and products"   |5542          | "Food"  |"1000 tonnes"|         |
|5      |"AFG"             |     2     | "Afghanistan" |2905       |   "Maize and products"   |5521          | "Feed"  |"1000 tonnes"|         |
|6      |"AFG"             |     2     | "Afghanistan" |2907       |   "Maize and products"   |5542          | "Food"  |"1000 tonnes"|         |
|7      |"AFG"             |     2     | "Afghanistan" |2908       |  "Millet and products"   |5542          | "Food"  |"1000 tonnes"|         |
|8      |"AFG"             |     2     | "Afghanistan" |2909       |     "Cereals, Other"     |5542          | "Food"  |"1000 tonnes"|         |
|9      |"AFG"             |     2     | "Afghanistan" |2911       |  "Potatoes and products" |5542          | "Food"  |"1000 tonnes"|         |
|10     |"AFG"             |     2     | "Afghanistan" |2912       |       "Sugar cane"       |5521          | "Feed"  |"1000 tonnes"|         |
|...more|                  |           |               |           |                          |              |         |             |         |
|21477  |"ZWE"             |    181    |  "Zimbabwe"   |2928       |     "Miscellaneous"      |5142          | "Food"  |"1000 tonnes"|         |

#### Create tensor
```Julia
begin
	countries = unique(FAO_dataset[:, :Area])
	food_types = unique(FAO_dataset[:, :Element])
	years = 1961:2013
	@assert all(in(names(FAO_dataset)), string.("Y", years))

	FAO_tensor = zeros(length(countries), length(years), length(food_types))
	for FAO_row in eachrow(FAO_dataset)
		country_idx   = only(findall(==(FAO_row[:Area]), countries))
		food_type_idx = only(findall(==(FAO_row[:Element]), food_types))
		for (year_idx, year) in enumerate(years)
			quantity = FAO_row["Y$(year)"]
			if !ismissing(quantity)
				FAO_tensor[country_idx, year_idx, food_type_idx] += quantity
			end
		end
	end
end
```
```Julia
FAO_tensor
```
```
174×53×2 Array{Float64, 3}:
[:, :, 1] =
  8761.0   8694.0   8458.0   9430.0   9753.0  …  19642.0  19908.0   21184.0   21471.0
  1612.0   1641.0   1643.0   1767.0   1789.0      6573.0   6780.0    6909.0    6952.0
  7405.0   7141.0   6798.0   7157.0   7425.0     54267.0  58375.0   60816.0   63455.0
  4716.0   4657.0   5124.0   5154.0   5399.0     25992.0  27455.0   27968.0   30121.0
    90.0     92.0    103.0     93.0     82.0       115.0    118.0     113.0     119.0
 33850.0  33231.0  33692.0  34628.0  36863.0  …  61534.0  63810.0   64614.0   65063.0
     0.0      0.0      0.0      0.0      0.0      5114.0   5315.0    5787.0    5862.0
     ⋮                                        ⋱               ⋮              
    93.0     95.0     97.0     96.0    101.0       355.0    354.0     362.0     370.0
  9163.0   9078.0   9467.0  10229.0  10392.0     41113.0  42109.0   40549.0   39706.0
 21752.0  22708.0  23439.0  23939.0  24705.0  …  90451.0  91214.0  103344.0  105399.0
  2815.0   2870.0   2975.0   3049.0   3137.0     16682.0  16744.0   17929.0   18325.0
  2886.0   2967.0   2999.0   3043.0   3148.0      9399.0   9495.0    9859.0   10180.0
  3080.0   3287.0   3289.0   3368.0   3442.0      9063.0   9464.0    9661.0    9524.0

[:, :, 2] =
  720.0   720.0   736.0   740.0   720.0  …   1388.0   1192.0   1522.0   1536.0
   94.0   108.0   124.0   122.0    95.0      1334.0   1334.0   1312.0   1319.0
   83.0    94.0    63.0    98.0    84.0      5804.0   7477.0   8549.0   8706.0
  118.0   118.0   116.0   132.0   128.0     12408.0  13118.0  10096.0  18518.0
    2.0     2.0     2.0     2.0     2.0         0.0      0.0      0.0      0.0
 9552.0  7553.0  6527.0  7010.0  8073.0  …  11508.0  14954.0  11332.0  15780.0
    0.0     0.0     0.0     0.0     0.0       794.0   1108.0   1191.0   1313.0
    ⋮                                    ⋱               ⋮             
    4.0     6.0     6.0     6.0     6.0        13.0     12.0     12.0     12.0
  360.0   291.0   321.0   310.0   249.0      4177.0   4042.0   4647.0   5756.0
 2104.0  2512.0  2614.0  2438.0  2256.0  …  23026.0  21533.0  22210.0  22712.0
  167.0   168.0   172.0   175.0   191.0       326.0    346.0    442.0    420.0
   90.0    90.0    70.0    78.0    88.0       388.0    568.0    652.0    816.0
  180.0   216.0   190.0   370.0   498.0       238.0    225.0    254.0    262.0
```

#### Run CP Decomposition 

```Julia
FAO_M = gcp(FAO_tensor, 3, loss = GCPLosses.NonnegativeLeastSquares(), algorithm = GCPAlgorithms.LBFGSB(;maxfun=500000, maxiter=500000, pgtol = 1e-7))
```
```
FAO_M = 174×53×2 CPD{Float64, 3, Vector{Float64}, Matrix{Float64}} with 3 components
λ weights:
3-element Vector{Float64}: …
U[1] factor matrix:
174×3 Matrix{Float64}: …
U[2] factor matrix:
53×3 Matrix{Float64}: …
U[3] factor matrix:
2×3 Matrix{Float64}: …
```

#### Graph all the components with all their (normalized factors).

```Julia
poster_fig = with_theme() do
    fig = Figure(; size=(800, 500))

    # Plot factors (normalized by max)

	# Reorder
	perm = [3, 1, 2]
	M = deepcopy(FAO_M)
	for i in 1:ndims(M)
		M.U[i] .= M.U[i][:, perm]
	end
	
    for row in 1:ncomponents(M)
		ax = GeoAxis(fig[row,1], xgridvisible = false, ygridvisible = false)
		ax.xticklabelsvisible[] = false
		ax.yticklabelsvisible[] = false
			 
		hm = poly!(ax, natearth_df[NATEARTH_IDX,:geometry];
			color = normalize(M.U[1][:,row], Inf), colorrange = (0,1),
    		strokecolor = :black, strokewidth = 0.5,	
		)
		
		poly!(ax, natearth_df[Missing_Countries, :geometry]; strokecolor=:black, strokewidth = 1, color=(:gray))
		lines(fig[row,2], years, normalize(M.U[2][:,row], Inf), color = :green, axis = (; xticks= 1960:10:2020))


		
        barplot(fig[row,3], 1:length(food_types), normalize(M.U[3][:,row], Inf);
		axis = (; xticks = (1:length(food_types), food_types)), color = :green)
	end


    # Link and hide x axes
    linkxaxes!(contents(fig[:,2])...)
    linkxaxes!(contents(fig[:,3])...)
    hidexdecorations!.(contents(fig[1:end-1,2:3]); ticks=false, grid=false)
	linkyaxes!(contents(fig[:,2])...)
    linkyaxes!(contents(fig[:,3])...)
	hideydecorations!.(contents(fig[1:end,2:3]); ticks=false, grid=false)
	
    # Add labels
    Label(fig[0,1], "Country"; tellwidth=false, font = :bold, fontsize=20)
    Label(fig[0,2], "Year"; tellwidth=false, font = :bold, fontsize=20)
    Label(fig[0,3], "Type"; tellwidth=false, font = :bold, fontsize=20)

	Label(fig[1,0], "Component 1"; tellheight=false, font = :bold, fontsize=15, rotation = pi/2)
    Label(fig[2,0], "Component 2"; tellheight=false, font = :bold, fontsize=15, 		rotation = pi/2)
    Label(fig[3,0], "Component 3"; tellheight=false, font = :bold, fontsize=15, 			rotation = pi/2)
	
	rowgap!(fig.layout, -20)
	colsize!(fig.layout, 3, Relative(1/5))
	colgap!(fig.layout, 1, -30)
	colgap!(fig.layout, 2, -30)
	fig
end
```


![image](https://github.com/user-attachments/assets/9ed58c42-25e1-45c2-a890-09769be2b108)



