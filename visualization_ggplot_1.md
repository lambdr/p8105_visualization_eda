Data viz: ggplot 1
================

Load all necessary pacakges.

``` r
library(tidyverse)
library(ggridges)
```

Get the data for plotting today.

``` r
# Pull 3 weather stations from NOAA dataset
df_weather = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
# Rename and clean variables
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/Derek/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-09-28 10:20:09.047204 (8.524)

    ## file min/max dates: 1869-01-01 / 2023-09-30

    ## using cached file: /Users/Derek/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-09-28 10:20:15.065928 (3.83)

    ## file min/max dates: 1949-10-01 / 2023-09-30

    ## using cached file: /Users/Derek/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-09-28 10:20:17.264748 (0.994)

    ## file min/max dates: 1999-09-01 / 2023-09-30

Let’s make a plot!

``` r
ggplot(df_weather, aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_ggplot_1_files/figure-gfm/first%20plot-1.png)<!-- -->

Pipes and stuff

``` r
ggp_nyc_weather <- 
  df_weather |> 
    filter(name == "CentralPark_NY") |> 
    ggplot(aes(x = tmin, y = tmax)) +
    geom_point()

ggp_nyc_weather
```

![](visualization_ggplot_1_files/figure-gfm/pipes%20and%20saving%20plot-1.png)<!-- -->

## Fancy plot

``` r
ggplot(df_weather, aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = 0.3) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_ggplot_1_files/figure-gfm/color%20and%20smooth-1.png)<!-- -->

Plot with facets

``` r
ggplot(df_weather, aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = 0.3) +
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_ggplot_1_files/figure-gfm/facets-1.png)<!-- -->

Let’s try a different plot. temps are boring

``` r
ggplot(df_weather, aes(x = date, y = tmax, color = name)) +
  geom_point(aes(size = prcp), alpha = 0.3) +
  geom_smooth(se = 0) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values (`geom_point()`).

![](visualization_ggplot_1_files/figure-gfm/tmax%20vs%20date-1.png)<!-- -->

Try assigning specfic colors

``` r
df_weather |> 
  filter(name != "CentralPark_NY") |> 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(alpha = 0.7, size = 0.5)
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_ggplot_1_files/figure-gfm/color%20assignment-1.png)<!-- -->
