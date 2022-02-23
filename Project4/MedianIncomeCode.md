
Load in Relevant Packages.

```{r}
library(tidycensus)
library(tidyverse)
library(tigris)
library(crsuggest)
```

Obtain median income data (variable B19013_001) for census tracts in King County, Washington for the years 2015-2019. 'Year' represents the last year of the 5-year average. Include geometry to enable mapping of the data.'cb = FALSE' indicates that the desired format is a TIGER/line shapefile (through package 'Tigris') rather than a cartographic boundary shapefile.

```{r}
WAtracts <- get_acs(
  geography = "tract",
  variables = "B19013_001", 
  state = "WA", 
  county = "King", 
  geometry = TRUE, 
  year = 2019,
  cb = FALSE
)
```

Transform the geometry to the suggested coordinate reference system. 

```{r}
suggest_crs(WAtracts)
waprojected <- st_transform(WAtracts, crs = 6597)
```

Define a function to erase parts of the map and remove water within the county.

```{r}
st_erase <- function(x, y) {
  st_difference(x, st_union(y))
}

wa_water <- area_water("WA", "King", year = 2019) 
suggest_crs(wa_water)
water_proj<- st_transform(wa_water, crs=6597)

wa_erase <- st_erase(waprojected, water_proj)
```

Visualize the map, personalize, and add labels.

```{r}
ggplot(wa_erase) + 
  geom_sf(aes(fill = estimate)) + 
  scale_fill_viridis_c(labels = scales::dollar) + 
  theme_void() + 
  labs(fill = "Median household\nincome", title = "            Median Household Income in King County, Washington")
```

