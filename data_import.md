Data Import
================

install.packages(“janitor”) install.packages(“skimr”)

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.0     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readxl)
library(haven)
```

## Read in some data

Read in the litters dataset. clean_names function is in the package
“janitor”. Since we did not library it, so we use “janitor::” to get the
function. You can also library it at the beginning. ckean_names: change
blank into \_ in the columns’ names.

``` r
litters_df = read_csv("data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = janitor::clean_names(litters_df)
```

## Take a look at the data

printing in the console

``` r
litters_df
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
head(litters_df)
```

    ## # A tibble: 6 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
tail(litters_df)
```

    ## # A tibble: 6 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #79                 25.4        43.8          19               8
    ## 2 Low8  #100                20          39.2          20               8
    ## 3 Low8  #4/84               21.8        35.2          20               4
    ## 4 Low8  #108                25.6        47.5          20               8
    ## 5 Low8  #99                 23.5        39            20               6
    ## 6 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

skim can give you missing value number, min, max, mean, p25, p75, small
histogram (but prof dosen’t use it frequently, he prefer view). view
will open up a new window to show full dataset. But that will make Knit
difficult so usually avoid using it in code chunk in R markdown.

view(litters_df)

``` r
skimr::skim(litters_df)
```

|                                                  |            |
|:-------------------------------------------------|:-----------|
| Name                                             | litters_df |
| Number of rows                                   | 49         |
| Number of columns                                | 8          |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |            |
| Column type frequency:                           |            |
| character                                        | 2          |
| numeric                                          | 6          |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |            |
| Group variables                                  | None       |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| group         |         0 |             1 |   4 |   4 |     0 |        6 |          0 |
| litter_number |         0 |             1 |   3 |  15 |     0 |       49 |          0 |

**Variable type: numeric**

| skim_variable   | n_missing | complete_rate |  mean |   sd |   p0 |   p25 |   p50 |   p75 | p100 | hist  |
|:----------------|----------:|--------------:|------:|-----:|-----:|------:|------:|------:|-----:|:------|
| gd0_weight      |        15 |          0.69 | 24.38 | 3.28 | 17.0 | 22.30 | 24.10 | 26.67 | 33.4 | ▃▇▇▆▁ |
| gd18_weight     |        17 |          0.65 | 41.52 | 4.05 | 33.4 | 38.88 | 42.25 | 43.80 | 52.7 | ▃▃▇▂▁ |
| gd_of_birth     |         0 |          1.00 | 19.65 | 0.48 | 19.0 | 19.00 | 20.00 | 20.00 | 20.0 | ▅▁▁▁▇ |
| pups_born_alive |         0 |          1.00 |  7.35 | 1.76 |  3.0 |  6.00 |  8.00 |  8.00 | 11.0 | ▁▃▂▇▁ |
| pups_dead_birth |         0 |          1.00 |  0.33 | 0.75 |  0.0 |  0.00 |  0.00 |  0.00 |  4.0 | ▇▂▁▁▁ |
| pups_survive    |         0 |          1.00 |  6.41 | 2.05 |  1.0 |  5.00 |  7.00 |  8.00 |  9.0 | ▁▃▂▇▇ |

?read_csv

## Options to read_csv

Check out `?read_csv()` for more information.

litters_df = read_csv(“data/FAS_litters.csv”, skip = 10, col_names =
FALSE)

skip = 10: skip the first 10 rows col_name = FALSE: the first row is not
columns’ name and R will name them as X1 to Xn

litters_df = read_csv(“data/FAS_litters.csv”, na = c(““,”NA”, “.”, 999))

na = c(““,”NA”, “.”, 999)): when r see these four will treat them as
missing value.

## Other file formats

Read in an excel file.

``` r
mlb_df = read_excel("data/mlb11.xlsx")
mlb_df
```

    ## # A tibble: 30 × 12
    ##    team        runs at_bats  hits homeruns bat_avg strikeouts stolen_bases  wins
    ##    <chr>      <dbl>   <dbl> <dbl>    <dbl>   <dbl>      <dbl>        <dbl> <dbl>
    ##  1 Texas Ran…   855    5659  1599      210   0.283        930          143    96
    ##  2 Boston Re…   875    5710  1600      203   0.28        1108          102    90
    ##  3 Detroit T…   787    5563  1540      169   0.277       1143           49    95
    ##  4 Kansas Ci…   730    5672  1560      129   0.275       1006          153    71
    ##  5 St. Louis…   762    5532  1513      162   0.273        978           57    90
    ##  6 New York …   718    5600  1477      108   0.264       1085          130    77
    ##  7 New York …   867    5518  1452      222   0.263       1138          147    97
    ##  8 Milwaukee…   721    5447  1422      185   0.261       1083           94    96
    ##  9 Colorado …   735    5544  1429      163   0.258       1201          118    73
    ## 10 Houston A…   615    5598  1442       95   0.258       1164          118    56
    ## # ℹ 20 more rows
    ## # ℹ 3 more variables: new_onbase <dbl>, new_slug <dbl>, new_obs <dbl>

?read_excel

mlb_df = read_excel(“data/mlb11.xlsx”, range = “A1:F7”)

sheet = : import specific sheet range = : import specific range, from
column a to b

Read in a sas file.

``` r
pulse_df = read_sas("data/public_pulse_data.sas7bdat")
pulse_df
```

    ## # A tibble: 1,087 × 7
    ##       ID   age Sex    BDIScore_BL BDIScore_01m BDIScore_06m BDIScore_12m
    ##    <dbl> <dbl> <chr>        <dbl>        <dbl>        <dbl>        <dbl>
    ##  1 10003  48.0 male             7            1            2            0
    ##  2 10015  72.5 male             6           NA           NA           NA
    ##  3 10022  58.5 male            14            3            8           NA
    ##  4 10026  72.7 male            20            6           18           16
    ##  5 10035  60.4 male             4            0            1            2
    ##  6 10050  84.7 male             2           10           12            8
    ##  7 10078  31.3 male             4            0           NA           NA
    ##  8 10088  56.9 male             5           NA            0            2
    ##  9 10091  76.0 male             0            3            4            0
    ## 10 10092  74.2 female          10            2           11            6
    ## # ℹ 1,077 more rows

## Comparison with Base R

what about `read.csv` …?

Don’t use read.csv since it is more difficult to look read_csv is
better.

``` r
litters_base = read.csv("data/FAS_litters.csv")
litters_readr = read_csv("data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_base
```

    ##    Group   Litter.Number GD0.weight GD18.weight GD.of.Birth Pups.born.alive
    ## 1   Con7             #85       19.7        34.7          20               3
    ## 2   Con7       #1/2/95/2       27.0        42.0          19               8
    ## 3   Con7   #5/5/3/83/3-3       26.0        41.4          19               6
    ## 4   Con7     #5/4/2/95/2       28.5        44.1          19               5
    ## 5   Con7     #4/2/95/3-3         NA          NA          20               6
    ## 6   Con7     #2/2/95/3-2         NA          NA          20               6
    ## 7   Con7 #1/5/3/83/3-3/2         NA          NA          20               9
    ## 8   Con8       #3/83/3-3         NA          NA          20               9
    ## 9   Con8         #2/95/3         NA          NA          20               8
    ## 10  Con8     #3/5/2/2/95       28.5          NA          20               8
    ## 11  Con8     #5/4/3/83/3       28.0          NA          19               9
    ## 12  Con8   #1/6/2/2/95-2         NA          NA          20               7
    ## 13  Con8 #3/5/3/83/3-3-2         NA          NA          20               8
    ## 14  Con8       #2/2/95/2         NA          NA          19               5
    ## 15  Con8   #3/6/2/2/95-3         NA          NA          20               7
    ## 16  Mod7             #59       17.0        33.4          19               8
    ## 17  Mod7            #103       21.4        42.1          19               9
    ## 18  Mod7       #1/82/3-2         NA          NA          19               6
    ## 19  Mod7       #3/83/3-2         NA          NA          19               8
    ## 20  Mod7       #2/95/2-2         NA          NA          20               7
    ## 21  Mod7       #3/82/3-2       28.0        45.9          20               5
    ## 22  Mod7       #4/2/95/2       23.5          NA          19               9
    ## 23  Mod7     #5/3/83/5-2       22.6        37.0          19               5
    ## 24  Mod7      #8/110/3-2         NA          NA          20               9
    ## 25  Mod7            #106       21.7        37.8          20               5
    ## 26  Mod7           #94/2       24.4        42.9          19               7
    ## 27  Mod7             #62       19.5        35.9          19               7
    ## 28  Low7           #84/2       24.3        40.8          20               8
    ## 29  Low7            #107       22.6        42.4          20               9
    ## 30  Low7           #85/2       22.2        38.5          20               8
    ## 31  Low7             #98       23.8        43.8          20               9
    ## 32  Low7            #102       22.6        43.3          20              11
    ## 33  Low7            #101       23.8        42.7          20               9
    ## 34  Low7            #111       25.5        44.6          20               3
    ## 35  Low7            #112       23.9        40.5          19               6
    ## 36  Mod8             #97       24.5        42.8          20               8
    ## 37  Mod8           #5/93         NA        41.1          20              11
    ## 38  Mod8         #5/93/2         NA          NA          19               8
    ## 39  Mod8       #7/82-3-2       26.9        43.2          20               7
    ## 40  Mod8      #7/110/3-2       27.5        46.0          19               8
    ## 41  Mod8         #2/95/2       28.5        44.5          20               9
    ## 42  Mod8           #82/4       33.4        52.7          20               8
    ## 43  Low8             #53       21.8        37.2          20               8
    ## 44  Low8             #79       25.4        43.8          19               8
    ## 45  Low8            #100       20.0        39.2          20               8
    ## 46  Low8           #4/84       21.8        35.2          20               4
    ## 47  Low8            #108       25.6        47.5          20               8
    ## 48  Low8             #99       23.5        39.0          20               6
    ## 49  Low8            #110       25.5        42.7          20               7
    ##    Pups.dead...birth Pups.survive
    ## 1                  4            3
    ## 2                  0            7
    ## 3                  0            5
    ## 4                  1            4
    ## 5                  0            6
    ## 6                  0            4
    ## 7                  0            9
    ## 8                  1            8
    ## 9                  0            8
    ## 10                 0            8
    ## 11                 0            8
    ## 12                 0            6
    ## 13                 0            8
    ## 14                 0            4
    ## 15                 0            7
    ## 16                 0            5
    ## 17                 1            9
    ## 18                 0            6
    ## 19                 0            8
    ## 20                 0            7
    ## 21                 0            5
    ## 22                 0            7
    ## 23                 0            5
    ## 24                 0            9
    ## 25                 0            2
    ## 26                 1            3
    ## 27                 2            4
    ## 28                 0            8
    ## 29                 0            8
    ## 30                 0            6
    ## 31                 0            9
    ## 32                 0            7
    ## 33                 0            9
    ## 34                 2            3
    ## 35                 1            1
    ## 36                 1            8
    ## 37                 0            9
    ## 38                 0            8
    ## 39                 0            7
    ## 40                 1            8
    ## 41                 0            9
    ## 42                 0            6
    ## 43                 1            7
    ## 44                 0            7
    ## 45                 0            7
    ## 46                 0            4
    ## 47                 0            7
    ## 48                 0            5
    ## 49                 0            6

``` r
litters_readr
```

    ## # A tibble: 49 × 8
    ##    Group `Litter Number` `GD0 weight` `GD18 weight` `GD of Birth`
    ##    <chr> <chr>                  <dbl>         <dbl>         <dbl>
    ##  1 Con7  #85                     19.7          34.7            20
    ##  2 Con7  #1/2/95/2               27            42              19
    ##  3 Con7  #5/5/3/83/3-3           26            41.4            19
    ##  4 Con7  #5/4/2/95/2             28.5          44.1            19
    ##  5 Con7  #4/2/95/3-3             NA            NA              20
    ##  6 Con7  #2/2/95/3-2             NA            NA              20
    ##  7 Con7  #1/5/3/83/3-3/2         NA            NA              20
    ##  8 Con8  #3/83/3-3               NA            NA              20
    ##  9 Con8  #2/95/3                 NA            NA              20
    ## 10 Con8  #3/5/2/2/95             28.5          NA              20
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: `Pups born alive` <dbl>, `Pups dead @ birth` <dbl>,
    ## #   `Pups survive` <dbl>
