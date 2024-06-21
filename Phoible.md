Salos 2024 summer school: Working with PHOIBLE
================
Steven Moran and Alena Witzlack
(21 June, 2024)

- [Introduction](#introduction)

------------------------------------------------------------------------

The libraries you will need.

``` r
library(tidyverse)
```

# Introduction

The [PHOIBLE database](https://phoible.org) is an online repository of
phonological inventory data. Let’s have a look at the website, where we
can explore by various categories:

- [Inventories](https://phoible.org/inventories)
- [Languages](https://phoible.org/languages)
- [Segments](https://phoible.org/parameters)

------------------------------------------------------------------------

But what if we want to explore and work with the data programmatically?
What’s the first thing we need to do?

------------------------------------------------------------------------

We need to access the data. So, where is it?

One option is to download the data from the website:

- <https://phoible.org/download>

Data comes in different data formats (and different data types).

In linguistics, data are usually curated and we have data sets in
different stages of development. For example, the PHOIBLE data are
curated and maintained in a working format in a
[GitHub](https://github.com) repository here:

- <https://github.com/phoible/dev>

This repository contains the raw data and code (scripts) to transform
different data formats into a single spreadsheet:

- <https://github.com/phoible/dev/blob/master/data/phoible.csv>

This is a rather large spreadsheet, so we have made a smaller aggregated
one here:

- <https://github.com/bambooforest/salos/blob/main/data/phoible.csv>

When a data table (aka spreadsheet) is smaller, GitHub will display it
online for you:

- <https://github.com/bambooforest/salos/blob/main/data/phoible.csv>

------------------------------------------------------------------------

How do we load the data into R/RStudio?

``` r
df <- read_csv('data/phoible.csv')
```

    ## Rows: 969 Columns: 12
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (3): language, genus, fam
    ## dbl (9): phonemes, con, son, obs, vow, monoph, qua, ton, population
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df
```

    ## # A tibble: 969 × 12
    ##    language genus      fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##    <chr>    <chr>      <chr>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ##  1 aal      Biu-Manda… afas        36    27     6    21     9      9     9     0
    ##  2 aau      Upper Sep… sepr        14     9     5     4     5      5     5     0
    ##  3 abi      Kwa        ncon        39    21     7    14    18     18     9     0
    ##  4 abt      Middle Se… sepr        24    17    12     5     7      7     6     0
    ##  5 ace      Malayic    anes        55    21     8    13    34     17    10     0
    ##  6 acv      Palaihnih… hoka        23    17     6    11     6      6     6     0
    ##  7 ada      <NA>       ncon        39    23     8    15    12     12     7     4
    ##  8 adj      Kwa        ncon        31    21     8    13     7      7     7     3
    ##  9 adn      Greater A… trng        40    18     6    12    22     12     7     0
    ## 10 adz      Oceanic    anes        25    18     6    12     7      7     4     0
    ## # ℹ 959 more rows
    ## # ℹ 1 more variable: population <dbl>
