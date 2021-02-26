wids\_dataXB\_eda
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ tibble  3.0.6     ✓ purrr   0.3.4
    ## ✓ tidyr   1.1.2     ✓ dplyr   1.0.4
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x lubridate::as.difftime() masks base::as.difftime()
    ## x lubridate::date()        masks base::date()
    ## x dplyr::filter()          masks stats::filter()
    ## x readr::guess_encoding()  masks rvest::guess_encoding()
    ## x lubridate::intersect()   masks base::intersect()
    ## x dplyr::lag()             masks stats::lag()
    ## x purrr::pluck()           masks rvest::pluck()
    ## x lubridate::setdiff()     masks base::setdiff()
    ## x lubridate::union()       masks base::union()

``` r
library(patchwork)
```

``` r
varDict <- read.csv("widsdatathon2021/DataDictionaryWiDS2021.csv") #a csv file on every variable's meaning
```
