Visualization
================

install.packages(“patchwork”) install.packages(“viridis”)

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(patchwork)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2023-12-31") |>
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

    ## using cached file: /Users/yu/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2024-05-18 14:02:16.709216 (8.605)

    ## file min/max dates: 1869-01-01 / 2024-05-31

    ## using cached file: /Users/yu/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2024-05-18 14:02:38.87762 (3.896)

    ## file min/max dates: 1949-10-01 / 2024-05-31

    ## using cached file: /Users/yu/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2024-05-18 14:02:43.732044 (1.022)

    ## file min/max dates: 1999-09-01 / 2024-05-31

``` r
weather_df
```

    ## # A tibble: 3,285 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6
    ## # ℹ 3,275 more rows

## Remember this plot ..?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5)
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Labels

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from rnoaa package; temperatures in 2021 to 2023"
  )
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Scale

Start with the same plot; x and y scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from rnoaa package; temperatures in 2021 to 2023"
  ) +
  scale_x_continuous(
    breaks = c(-15, 0, 15),
    labels = c("-15 C", "0", "15 C")
  ) +
  scale_y_continuous(
    position = "right"
  )
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

-scale_x_continuous: change x axis labels -scale_y_continuous: change y
axis labels -trans = “sqrt”:square root transformation, you can also do
trans = “log” - position = “right”: put y labels on the right side

Look at color scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from rnoaa package; temperatures in 2021 to 2023"
  ) +
  scale_color_hue(
    name = "Location",
    h = c(150,260))
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

-scale_color_hue:map the color aesthetic to hues in the HCL
(hue-chroma-luminance) color space. -h = c(10, 260): Specifies the range
of hues to be used. The hue is an angle in the color wheel, with values
ranging from 0 to 360.

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from rnoaa package; temperatures in 2021 to 2023"
  ) +
 viridis::scale_color_viridis(
   name = "Location",
   discrete = TRUE
 )
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

viridis::scale_color_viridis: give different color to locations discrete
= TRUE: viridis default thinks scales are continue so here need to tell
R that scale is discrete.

## Themes

Shift the legend

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from rnoaa package; temperatures in 2021 to 2023"
  ) +
 viridis::scale_color_viridis(
   name = "Location",
   discrete = TRUE) +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

legend.position = “bottom”: shift the legend from side to the bottom

Change the overall theme

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maximum daily temperature (C)",
    caption = "Data from rnoaa package; temperatures in 2021 to 2023"
  ) +
 viridis::scale_color_viridis(
   name = "Location",
   discrete = TRUE) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

theme_minimal: change the background into white, and without the box.
theme_bw():change the background into white theme_classic():white
background and no gridlines ggthemes::theme_economist: there are many
other theme in ggthemes

## Setting options

``` r
library(tidyverse)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.color = "viridis",
  ggplots.continuous.fill = "viridis"
)

scale_colour_discrete = scale_color_viridis_d
scale_fill_continuous = scale_fill_viridis_d
```

eval = FALSE: code within the block will not be executed. If I have this
in my very first code chunk in the markdown document, this will load
tidyverse and update the theme everywhere. from options …
scale_fill_viridis_d: make sure plots use the virtus color palette Put
this in your Rmarkdown file and unit the theme of plots.

## Data args in ‘geom’

``` r
central_park =
  weather_df %>%
  filter(name == "CentralPark_NY")

moloka =
  weather_df %>%
  filter(name == "Molokai_HI")

ggplot(data = moloka, aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_line(data = central_park)
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Apply different geom to different datasets. If I delete the data =
central_park, geom_line will apply to moloka.

## ‘patchwork’

remember faceting?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, fill = name)) +
  geom_density(alpha = .5) +
  facet_grid(. ~ name)
```

    ## Warning: Removed 19 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

what happens when you want multipanel plots but can’t facet?

``` r
tmax_tmin_p =
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  theme(legend.position = "none")

prcp_dens_p = 
  weather_df %>% 
  filter(prcp >0) %>% 
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = .5)

tmax_date_p = 
  weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_smooth(se = FALSE) +
  theme(legend.position = "none")

tmax_tmin_p + prcp_dens_p + tmax_date_p
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 19 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).
    ## Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
tmax_tmin_p / (prcp_dens_p + tmax_date_p)
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 19 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).
    ## Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->

``` r
(tmax_tmin_p + prcp_dens_p) / tmax_date_p
```

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 19 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).
    ## Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-13-3.png)<!-- -->

tmax_tmin_p + prcp_dens_p + tmax_date_p: put three plots together
tmax_tmin_p / (prcp_dens_p + tmax_date_p): change the position of plots

## Data manipulation

Control your factors

``` r
weather_df %>% 
  mutate(
    name = factor(name),
    name = forcats::fct_relevel(name, c("Molokai_HI"))
  ) %>% 
  ggplot(aes(x = name, y = tmax, fill = name)) +
  geom_violin(alpha = .5)
```

    ## Warning: Removed 19 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->
name = factor(name): change name from character to factor(categorical
data). name = forcats::fct_relevel(name, c(“Molokai_HI”)): number the
Molokai as the first and others stay the same.

What if I wanted densities for tmin and tmax simultaneously?

``` r
weather_df %>% 
  pivot_longer(
    tmax:tmin,
    names_to = "observation",
    values_to = "temperature"
  ) %>% 
  ggplot(aes(x = temperature, fill = observation)) +
  geom_density(alpha = .5) +
  facet_grid(. ~ name)
```

    ## Warning: Removed 38 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](Visualization-II_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
