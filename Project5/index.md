# Hotspot Analysis of Asian Residents in Pennsylvania
This map shows areas of high, low and insignificant clustering of Asian Americans in Pennsylvania. The tracts only showed high and insignificant clustering. 

Data is from the American Community Survey 5-year dataset for 2015-2019. Nearest neighbor (queen) analysis was used to calculate the Getis-Ord local Gi* statistic for Asian residents in Pennsylvania by census tract. The analysis and figures were done in R with packages like tidycensus, tidyverse, sf and ggplot2. Complete R code and walkthrough can be found [here](RCodeForHotspot.md).
