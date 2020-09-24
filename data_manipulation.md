Data Manipulation
================
Linh Tran
9/23/2020

## Loading data and cleaning column names

``` r
options (tibble.print_min = 3)

litters_df = read_csv("data/FAS_litters.csv", 
                      col_types = "ccddiiii")
litters_df = clean_names(litters_df)

pups_df = read_csv("data/FAS_pups.csv",
                   col_types = "ciiiii")
pups_df = clean_names(pups_df)
```

## `select`

Choose some columns and not others.

``` r
select(litters_df, group, gd0_weight)
```

    ## # A tibble: 49 x 2
    ##   group gd0_weight
    ##   <chr>      <dbl>
    ## 1 Con7        19.7
    ## 2 Con7        27  
    ## 3 Con7        26  
    ## # … with 46 more rows

Select from gd0\_weight to gd\_of\_birth

``` r
select(litters_df, group, gd0_weight: gd_of_birth)
```

    ## # A tibble: 49 x 4
    ##   group gd0_weight gd18_weight gd_of_birth
    ##   <chr>      <dbl>       <dbl>       <int>
    ## 1 Con7        19.7        34.7          20
    ## 2 Con7        27          42            19
    ## 3 Con7        26          41.4          19
    ## # … with 46 more rows

Specify what we want to lose (litter number)

``` r
select(litters_df, -litter_number)
```

    ## # A tibble: 49 x 7
    ##   group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_birth
    ##   <chr>      <dbl>       <dbl>       <int>           <int>           <int>
    ## 1 Con7        19.7        34.7          20               3               4
    ## 2 Con7        27          42            19               8               0
    ## 3 Con7        26          41.4          19               6               0
    ## # … with 46 more rows, and 1 more variable: pups_survive <int>

Renaming columns

``` r
select(litters_df, gd_weight_0 = gd0_weight, gd_weight_18 = gd18_weight)
```

    ## # A tibble: 49 x 2
    ##   gd_weight_0 gd_weight_18
    ##         <dbl>        <dbl>
    ## 1        19.7         34.7
    ## 2        27           42  
    ## 3        26           41.4
    ## # … with 46 more rows

Can also use rename()

``` r
rename(litters_df, gd_weight_0 = gd0_weight, gd_weight_18 = gd18_weight)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd_weight_0 gd_weight_18 gd_of_birth pups_born_alive
    ##   <chr> <chr>               <dbl>        <dbl>       <int>           <int>
    ## 1 Con7  #85                  19.7         34.7          20               3
    ## 2 Con7  #1/2/95/2            27           42            19               8
    ## 3 Con7  #5/5/3/83/3-3        26           41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

Select helpers

``` r
select(litters_df, starts_with("gd"))
```

    ## # A tibble: 49 x 3
    ##   gd0_weight gd18_weight gd_of_birth
    ##        <dbl>       <dbl>       <int>
    ## 1       19.7        34.7          20
    ## 2       27          42            19
    ## 3       26          41.4          19
    ## # … with 46 more rows

``` r
select(litters_df, litter_number, everything())
```

    ## # A tibble: 49 x 8
    ##   litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr>         <chr>      <dbl>       <dbl>       <int>           <int>
    ## 1 #85           Con7        19.7        34.7          20               3
    ## 2 #1/2/95/2     Con7        27          42            19               8
    ## 3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

Relocate litter number columne to the beginning

``` r
relocate(litters_df, litter_number)
```

    ## # A tibble: 49 x 8
    ##   litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr>         <chr>      <dbl>       <dbl>       <int>           <int>
    ## 1 #85           Con7        19.7        34.7          20               3
    ## 2 #1/2/95/2     Con7        27          42            19               8
    ## 3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>
