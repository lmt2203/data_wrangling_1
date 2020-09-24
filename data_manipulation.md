Data Manipulation
================
Linh Tran
9/23/2020

## Loading data and cleaning column names

``` r
options (tibble.print_min = 3)

litters_data = read_csv("data/FAS_litters.csv", 
                      col_types = "ccddiiii")
litters_data = clean_names(litters_data)

pups_data = read_csv("data/FAS_pups.csv",
                   col_types = "ciiiii")
pups_data = clean_names(pups_data)
```
