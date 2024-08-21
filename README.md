[![DOI](https://zenodo.org/badge/810518024.svg)](https://zenodo.org/doi/10.5281/zenodo.11508494)

## Repository for PyRATES Project

Created as part of the [PyRATES workshop](https://linked.earth/FROGS) June 2024.

This project will be a partial reproducibility study of [Koslow et al. (2011) “Impact of declining intermediate-water oxygen on deepwater fishes in the California Current”](https://www.researchgate.net/publication/263583233_Impact_of_declining_intermediate-water_oxygen_on_deepwater_fishes_in_the_California_Current).


## Workflow outline:

Note: Code was not available for this paper. In addition, datasets have been updated and changed since this paper was published.

<p align="center">
<b>Part 1. Get and process ichthyoplankton data</b>
</p>

• manual step: query [ERDDAP](https://coastwatch.pfeg.noaa.gov/erddap/tabledap/erdCalCOFIlrvcnt.html) for all CalCOFI ichthyoplankton data in the years 1951-2008, download the data as a csv
• read in data
• filter dataset for needed variables: line, station, scientific name, larval abundance
• filter for the CalCOFI sampling stations used in the study
• remove years with less than three seasons of data (JF = winter, MAM = spring, JJA = summer, SOND = winter)
• calculate seasonal mean abundance for each taxa
• calculate annual mean abundance (based on seasonal means) for each taxa
• remove taxa with data in less than half the years (29)
• log<sub>10</sub> transform annual means to normalize variance

output: ichthyoplankton abundance time series

<p align="center">
<b>Part 2. Get and process oxygen data</b>
</p>

• manual step: download [CalCOFI bottle data](https://calcofi.org/data/oceanographic-data/bottle-database/) as a csv
• read in data
• filter dataset for needed variables: year, line and station, depth, oxygen
• filter for years 1951-2008
• filter for the stations used in this study
• filter for depths 200-400m
• remove years with less than three seasons of data (JF = winter, MAM = spring, JJA = summer, SOND = winter)
• calculate seasonal means of oxygen
• calculate annual means (based on seasonal means) of oxygen

output: oxygen time series

<p align="center">
<b>Part 3. Analysis</b>
</p>

<b>Part 3.1. Principal Components Analysis (PCA)</b>

input: ichthyoplankton abundance time series

• standardize the abundance data by each taxon's mean and standard deviation (Z-score)
• perform PCA
• extract PC1 scores and loadings

output: PC1 time series, PC1 loadings


<b>Part 3.2. Detrending</b>

input: PC1 time series

• remove linear trend from PC1 time series

output: detrended PC1 time series


<b>Part 3.3. First-Differencing</b>

inputs: PC1 time series, oxygen time series, ichthyoplankton abundance time series

• for each dataset, remove the years of intermittent sampling (1967-1983)
• take the first-difference of each time series

outputs: first-differenced PC1 time series, first-differenced oxygen time series, first-differenced ichthyoplankton abundance time series


<b>Part 3.4. Correlations between first-differenced ichthyoplankton and first-differenced PC1</b>

inputs: first-differenced PC1 time series, first-differenced ichthyoplankton abundance time series

• calculate Pearson's _r_ and _p_-value between each taxon's first-differenced time series and the first-differenced PC1 time series

outputs: dataframe of Pearson's correlations ichthyo-PC1


<b>Part 3.5. Correlations between ichthyoplankton abundance and oxygen time series</b>

inputs: ichthyoplankton abundance time series, oxygen time series

• calculate Spearman's _rho_ and _p_-value between each taxon's abundance time series and the oxygen time series

outputs: dataframe of Spearman's correlations ichthyo-oxygen


<b>Part 3.5. Correlations between PC1 and oxygen time series</b>

inputs: PC1 time series, oxygen time series, detrended PC1 time series, first-differenced PC1 time series, first-differenced oxygen time series

• calculate Pearson's _r_ and _p_-value between the original PC1 time series and original oxygen time series
• calculate Pearson's _r_ and _p_-value between the detrended PC1 time series and original oxygen time series
• calculate Pearson's _r_ and _p_-value between the first-differenced PC1 time series and the first-differenced oxygen time series

outputs: dataframe of Pearson's correlations between PC1 and oxygen


<p align="center">
<b>Part 4. Make figures</b>
</p>

<b>Table 2 (partial)</b>

inputs: PC1 loadings, dataframe of Pearson's correlations ichthyo-PC1, dataframe of Spearman's correlations ichthyo-oxygen

• create a table with 4 columns: scientific name, loading on PC1, first-differenced correlation with PC1, and Spearman's correlation with oxygen

<b>Figure 2</b>

inputs: ichthyoplankton abundance time series

• filter the abundance time series for 4 specific taxa (_Cyclothone_ spp., _Diogenichthys atlanticus_, _Bathylagoides wesethi_, _Vinciguerria lucetia_)
• plot the time series of each taxa abundance against year on shared axes

<b>Figure 3</b>

inputs: PC1 time series, oxygen time series

• plot the two time series on a shared x-axis and separate y-axes

<b>Table 3 (partial)</b>

inputs: dataframe of Pearson's correlations between PC1 and oxygen

• create a table to display the three correlation coefficients and their _p_-values