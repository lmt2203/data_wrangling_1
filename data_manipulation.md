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

## `select` - operate on columns

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

## `filter` - operate on rows

``` r
filter (litters_df, gd0_weight < 22)
```

    ## # A tibble: 8 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Mod7  #59                 17          33.4          19               8
    ## 3 Mod7  #103                21.4        42.1          19               9
    ## 4 Mod7  #106                21.7        37.8          20               5
    ## 5 Mod7  #62                 19.5        35.9          19               7
    ## 6 Low8  #53                 21.8        37.2          20               8
    ## 7 Low8  #100                20          39.2          20               8
    ## 8 Low8  #4/84               21.8        35.2          20               4
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, gd_of_birth == 20)
```

    ## # A tibble: 32 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 3 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # … with 29 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
filter(litters_df, pups_dead_birth == 0)
```

    ## # A tibble: 38 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #1/2/95/2             27        42            19               8
    ## 2 Con7  #5/5/3/83/3-3         26        41.4          19               6
    ## 3 Con7  #4/2/95/3-3           NA        NA            20               6
    ## # … with 35 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
filter(litters_df, gd0_weight <=20)
```

    ## # A tibble: 4 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Mod7  #59                 17          33.4          19               8
    ## 3 Mod7  #62                 19.5        35.9          19               7
    ## 4 Low8  #100                20          39.2          20               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

Negate things we are interested in

``` r
filter(litters_df, !(gd_of_birth == 20))
```

    ## # A tibble: 17 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  4 Con8  #5/4/3/83/3         28          NA            19               9
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #59                 17          33.4          19               8
    ##  7 Mod7  #103                21.4        42.1          19               9
    ##  8 Mod7  #1/82/3-2           NA          NA            19               6
    ##  9 Mod7  #3/83/3-2           NA          NA            19               8
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## 11 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 12 Mod7  #94/2               24.4        42.9          19               7
    ## 13 Mod7  #62                 19.5        35.9          19               7
    ## 14 Low7  #112                23.9        40.5          19               6
    ## 15 Mod8  #5/93/2             NA          NA            19               8
    ## 16 Mod8  #7/110/3-2          27.5        46            19               8
    ## 17 Low8  #79                 25.4        43.8          19               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, gd_of_birth !=20)
```

    ## # A tibble: 17 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  4 Con8  #5/4/3/83/3         28          NA            19               9
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #59                 17          33.4          19               8
    ##  7 Mod7  #103                21.4        42.1          19               9
    ##  8 Mod7  #1/82/3-2           NA          NA            19               6
    ##  9 Mod7  #3/83/3-2           NA          NA            19               8
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## 11 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 12 Mod7  #94/2               24.4        42.9          19               7
    ## 13 Mod7  #62                 19.5        35.9          19               7
    ## 14 Low7  #112                23.9        40.5          19               6
    ## 15 Mod8  #5/93/2             NA          NA            19               8
    ## 16 Mod8  #7/110/3-2          27.5        46            19               8
    ## 17 Low8  #79                 25.4        43.8          19               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, gd0_weight >=22, gd_of_birth == 20)
```

    ## # A tibble: 16 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  2 Mod7  #3/82/3-2           28          45.9          20               5
    ##  3 Low7  #84/2               24.3        40.8          20               8
    ##  4 Low7  #107                22.6        42.4          20               9
    ##  5 Low7  #85/2               22.2        38.5          20               8
    ##  6 Low7  #98                 23.8        43.8          20               9
    ##  7 Low7  #102                22.6        43.3          20              11
    ##  8 Low7  #101                23.8        42.7          20               9
    ##  9 Low7  #111                25.5        44.6          20               3
    ## 10 Mod8  #97                 24.5        42.8          20               8
    ## 11 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 12 Mod8  #2/95/2             28.5        44.5          20               9
    ## 13 Mod8  #82/4               33.4        52.7          20               8
    ## 14 Low8  #108                25.6        47.5          20               8
    ## 15 Low8  #99                 23.5        39            20               6
    ## 16 Low8  #110                25.5        42.7          20               7
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, !(gd0_weight >=22), !(gd_of_birth == 20))
```

    ## # A tibble: 3 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Mod7  #59                 17          33.4          19               8
    ## 2 Mod7  #103                21.4        42.1          19               9
    ## 3 Mod7  #62                 19.5        35.9          19               7
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, group == "Con7")
```

    ## # A tibble: 7 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2         NA          NA            20               6
    ## 7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, group %in% c("Con7", "Mod8"))
```

    ## # A tibble: 14 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Mod8  #97                 24.5        42.8          20               8
    ##  9 Mod8  #5/93               NA          41.1          20              11
    ## 10 Mod8  #5/93/2             NA          NA            19               8
    ## 11 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 12 Mod8  #7/110/3-2          27.5        46            19               8
    ## 13 Mod8  #2/95/2             28.5        44.5          20               9
    ## 14 Mod8  #82/4               33.4        52.7          20               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_df, group %in% c("Con7", "Mod8"), gd_of_birth == 20)
```

    ## # A tibble: 9 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 3 Con7  #2/2/95/3-2         NA          NA            20               6
    ## 4 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ## 5 Mod8  #97                 24.5        42.8          20               8
    ## 6 Mod8  #5/93               NA          41.1          20              11
    ## 7 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 8 Mod8  #2/95/2             28.5        44.5          20               9
    ## 9 Mod8  #82/4               33.4        52.7          20               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

## `mutate`- allows you to modify existing variables or create new variables

``` r
mutate(litters_df, wt_gain = gd18_weight - gd0_weight)
```

    ## # A tibble: 49 x 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 46 more rows, and 3 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>, wt_gain <dbl>

adding multiple variables at the same time

``` r
mutate (
  litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 x 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 46 more rows, and 3 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>, wt_gain <dbl>

## `arrange` - putting things in order

``` r
arrange(litters_df, pups_born_alive)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Low7  #111                25.5        44.6          20               3
    ## 3 Low8  #4/84               21.8        35.2          20               4
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
arrange(litters_df, gd0_weight, gd18_weight)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Mod7  #59                 17          33.4          19               8
    ## 2 Mod7  #62                 19.5        35.9          19               7
    ## 3 Con7  #85                 19.7        34.7          20               3
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

## Pipe operator to tie together sequence of actions `%>%`

This is one option - would NOT recommend

``` r
litters_data_raw = read_csv("data/FAS_litters.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

``` r
litters_clean_name = janitor::clean_names(litters_data_raw)
litters_data_selected = select(litters_clean_name, -pups_survive)
litters_mutated = mutate (litters_data_selected, wt_gain = gd18_weight - gd0_weight)
litters_without_missing_data = drop_na(litters_mutated, gd0_weight)
```

USE THE PIPE OPERATOR INSTEAD\!\!\!

``` r
litters_df = 
  read_csv("data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  select(-pups_survive) %>% 
  mutate(wt_gain = gd18_weight - gd0_weight) %>% 
  drop_na(gd0_weight)
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

Learning Assessment: Writing a chain of commands

``` r
pups_df = 
  read_csv("data/FAS_pups.csv") %>% 
  janitor::clean_names() %>% 
  filter(sex == 1) %>% 
  select(-pd_ears) %>% 
  mutate (pd_pivot_gt7 = pd_pivot >= 7)
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )
