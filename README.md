## Repository for PyRATES Workshop & Project

Created as part of the [PyRATES workshop](https://linked.earth/FROGS) June 2024, this repo contains tutorial files from the workshop as well as all files for the associated project.

This project will be a reproducibility study of [Koslow et al. (2011) “Impact of declining intermediate-water oxygen on deepwater fishes in the California Current”](https://www.researchgate.net/publication/263583233_Impact_of_declining_intermediate-water_oxygen_on_deepwater_fishes_in_the_California_Current).


## Workflow outline:

Code is not available for this paper.

<p align="center">
<b>Part 1. Get and process ichthyoplankton data</b>
</p>

| Step | In | Out |
| --------------------- | --------------------- | --------------------- |
| download ichthyo data | see [ERDDAP](https://coastwatch.pfeg.noaa.gov/erddap/tabledap/erdCalCOFItows.html) | csv file with ichthyo data |
| filter for select stations | output from ^ | dataframe with spp, station, month, year, abundance |
| filter for years 1951-2008 | output from ^ | df with spp, station, month, year, abundance |
| calculate seasonal mean abundances (JF = winter, MAM = spring, JJA = summer, SOND = winter) | output from ^ | df with spp, station, season, year, mean abundance |
| remove years with < 3 seasons of data | output from ^ | df with spp, station, season, year, mean abundance |
| calculate annual mean abundances from seasonal means | output from ^ | df with spp, station, year, mean abundance |
| log transform annual means to normalize variance | output from ^ | df with spp, station, year, log abundance |
| remove species with < half years of data | output from ^ | df of processed ichthyo data |

<p align="center">
<b>Part 2. Get and process oxygen data</b>
</p>

| Step | In | Out |
| --------------------- | --------------------- | --------------------- |
| download bottle data | see [CalCOFI](https://calcofi.org/data/oceanographic-data/bottle-database/) | csv file with CalCOFI bottle data |
| filter for only oxygen data | output from ^ | dataframe with station, time, depth, oxygen |
| filter for select stations | output from ^ | df with station, time, depth, oxygen |
| filter for years 1951-2008 | output from ^ | df with station, time, depth, oxygen |
| filter for depth 200-400m | output from ^ | df with station, time, depth, oxygen |
| calculate annual means | output from ^ | df of processed oxygen data |

<p align="center">
<b>Part 3. Analysis</b>
</p>

| Step | In | Out |
| --------------------- | --------------------- | --------------------- |
| standardize by mean and SD for each spp | df of processed ichthyo data | df with spp, station, year, standardized abundance |
| perform PCA | output from ^ | df of taxa and their loadings (correlations) on PC1 |
| remove intermittent survey years 1967-1983 | df of processed ichthyo data | df with spp, station, year, mean abundance |
| first-differencing | output from ^ | df with spp, station, year, first-differenced abundance |
| correlation of first-differenced timeseries with PC1 | output from PCA and first-differencing | df with spp, correlations, p-values of correlations |
| Spearman's rho correlations | df of processed ichthyo data and df of processed oxygen data | df with spp and oxygen correlations |

<p align="center">
<b>Part 4. Make figures</b>
</p>

| Step | In | Out |
| --------------------- | ------------------------ | --------------------- |
| put together table of spp, PC1 loading, habitat, first-differenced correlations, O2 correlations | dfs from PCA, first-differenced correlations, Spearman's rho correlations | table 2 |
| plot time series of 4 spp log abundance against year (_Cyclothone_ spp., _Diogenichthys atlanticus_, _Bathylagoides wesethi_, _Vinciguerria lucetia_) | df of processed ichthyo data | figure 2 |
| plot time series of PC1 and mean O2 against year | dfs from PCA and processed oxygen data | figure 3 |
