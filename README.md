[![DOI](https://zenodo.org/badge/810518024.svg)](https://zenodo.org/doi/10.5281/zenodo.11508494)

## Repository for PyRATES Project

Created as part of the [PyRATES workshop](https://linked.earth/FROGS) in June 2024 and as a professional development project for learning to code in Python.

This project is a partial reproducibility study of [Koslow et al. (2011) “Impact of declining intermediate-water oxygen on deepwater fishes in the California Current”](https://www.researchgate.net/publication/263583233_Impact_of_declining_intermediate-water_oxygen_on_deepwater_fishes_in_the_California_Current). Koslow et al. (2011) analyzed ichthyoplankton (larval fish) abundance data and hydrographic data obtained from the California Cooperative Oceanic Fisheries Investigations, [CalCOFI](calcofi.org), from the years 1951-2008. They found that the first principal component of the ichthyoplankton dataset was highly correlated to mean midwater oxygen levels, with many mesopelagic fishes showing declines in abundance in years when oxygen was low. I will aim to reproduce the first few parts of this study, covering the ichthyoplankton-oxygen analyses and reproducing Table 2 (partial), Figure 2, Figure 3, and Table 3 (partial).

***

## Workflow outline:

> [!NOTE]
> Original code was not available for this paper. In addition, datasets have been updated and changed since this paper was published, so my results will not be the same as in the paper.

<p align="center">
<b>Part 1. Get and process ichthyoplankton data</b>
</p>

- [x] manual step: query [ERDDAP](https://coastwatch.pfeg.noaa.gov/erddap/tabledap/erdCalCOFIlrvcnt.html) for all CalCOFI ichthyoplankton data in the years 1951-2008, download the data as a csv
- [x] read in data
- [x] filter dataset for needed variables: line, station, scientific name, larval abundance
- [x] filter for the CalCOFI sampling stations used in the study
- [x] remove years with less than three seasons of data (JF = winter, MAM = spring, JJA = summer, SOND = winter)
- [x] calculate seasonal mean abundance for each taxa
- [x] calculate annual mean abundance (based on seasonal means) for each taxa
- [x] remove taxa with data in less than half the years (29)
- [x] log<sub>10</sub> transform annual means to normalize variance

<ins>output</ins>: ichthyoplankton abundance time series

***

<p align="center">
<b>Part 2. Get and process oxygen data</b>
</p>

- [x] manual step: download [CalCOFI bottle data](https://calcofi.org/data/oceanographic-data/bottle-database/) as a csv
- [x] read in data
- [x] filter dataset for needed variables: year, line and station, depth, oxygen
- [x] filter for years 1951-2008
- [x] filter for the stations used in this study
- [x] filter for depths 200-400m
- [x] remove years with less than three seasons of data (JF = winter, MAM = spring, JJA = summer, SOND = winter)
- [x] calculate seasonal means of oxygen
- [x] calculate annual means (based on seasonal means) of oxygen

<ins>output</ins>: oxygen time series

***

<p align="center">
<b>Part 3. Analysis</b>
</p>

<b>Part 3.1. Principal Components Analysis (PCA)</b>

<ins>input</ins>: ichthyoplankton abundance time series

- [x] standardize the abundance data by each taxon's mean and standard deviation (Z-score)
- [x] perform PCA
- [x] extract PC1 scores and loadings

<ins>output</ins>: PC1 time series, PC1 loadings


<b>Part 3.2. Detrending</b>

<ins>input</ins>: PC1 time series

- [x] remove linear trend from PC1 time series

<ins>output</ins>: detrended PC1 time series


<b>Part 3.3. First-Differencing</b>

<ins>inputs</ins>: PC1 time series, oxygen time series, ichthyoplankton abundance time series

- [x] for each dataset, remove the years of intermittent sampling (1967-1983)
- [x] take the first-difference of each time series

<ins>outputs</ins>: first-differenced PC1 time series, first-differenced oxygen time series, first-differenced ichthyoplankton abundance time series


<b>Part 3.4. Correlations between first-differenced ichthyoplankton and first-differenced PC1</b>

<ins>inputs</ins>: first-differenced PC1 time series, first-differenced ichthyoplankton abundance time series

- [x] calculate Pearson's _r_ and _p_-value between each taxon's first-differenced time series and the first-differenced PC1 time series

<ins>outputs</ins>: dataframe of Pearson's correlations ichthyo-PC1


<b>Part 3.5. Correlations between ichthyoplankton abundance and oxygen time series</b>

<ins>inputs</ins>: ichthyoplankton abundance time series, oxygen time series

- [x] calculate Spearman's _rho_ and _p_-value between each taxon's abundance time series and the oxygen time series

<ins>outputs</ins>: dataframe of Spearman's correlations ichthyo-oxygen


<b>Part 3.6. Correlations between PC1 and oxygen time series</b>

<ins>inputs</ins>: PC1 time series, oxygen time series, detrended PC1 time series, first-differenced PC1 time series, first-differenced oxygen time series

- [x] calculate Pearson's _r_ and _p_-value between the original PC1 time series and original oxygen time series
- [x] calculate Pearson's _r_ and _p_-value between the detrended PC1 time series and original oxygen time series
- [x] calculate Pearson's _r_ and _p_-value between the first-differenced PC1 time series and the first-differenced oxygen time series

<ins>outputs</ins>: dataframe of Pearson's correlations between PC1 and oxygen

***

<p align="center">
<b>Part 4. Make figures</b>
</p>

<b>Table 2 (partial)</b>

<ins>inputs</ins>: PC1 loadings, dataframe of Pearson's correlations ichthyo-PC1, dataframe of Spearman's correlations ichthyo-oxygen

- [x] create a table with 4 columns: scientific name, loading on PC1, first-differenced correlation with PC1, and Spearman's correlation with oxygen

<b>Figure 2</b>

<ins>inputs</ins>: ichthyoplankton abundance time series

- [x] filter the abundance time series for 4 specific taxa (_Cyclothone_ spp., _Diogenichthys atlanticus_, _Bathylagoides wesethi_, _Vinciguerria lucetia_)
- [x] plot the time series of each taxa abundance against year on shared axes

<b>Figure 3</b>

<ins>inputs</ins>: PC1 time series, oxygen time series

- [x] plot the two time series on a shared x-axis and separate y-axes

<b>Table 3 (partial)</b>

<ins>inputs</ins>: dataframe of Pearson's correlations between PC1 and oxygen

- [x] create a table to display the three correlation coefficients and their _p_-values

***