Salos 2024 summer school: Working with PHOIBLE
================
Steven Moran and Alena Witzlack
(28 June, 2024)

- [Introduction](#introduction)

------------------------------------------------------------------------

These are the R libraries you will need.

``` r
library(tidyverse)
library(knitr)
```

Recall, if you have not installed these libraries before, then you need
to run the `install.packages()` command **once** before you load them
above, i.e., `install.packages('tidyverse')` in the Console below. Or
you can use the menu above: Click on Tools \> Install Packages.

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

How do we load the data into R/RStudio? You can simply use the variable
name that you loaded the data into. `df` is a commonly used abbreviation
for “data frame”, i.e., the data structure used with in R for example
for tabular data (more below).

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

How do we have a quick look at it? We saved the tabular data into an [R
dataframe](https://stat.ethz.ch/R-manual/R-devel/library/base/html/data.frame.html)
or in the [Tidyverse](https://www.tidyverse.org) (i.e., R packages for
data science) a [tibble](https://tibble.tidyverse.org) – a modernized
version of the R dataframe. We have saved it with the variable name `df`
– so we can simply call `df` to have a look at the data.

In RStudio we can browse the data table below – or we can also click on
the `df` data “object” in the top right hand panel. This works well when
we are working with smaller datasets (say up to a few thousand rows).

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

How do we programmatically look at its structure, i.e., how do we know
what it has in its columns and rows? We can use the `str()` command. We
can also click on the blue arrow in the data object in the top right
panel in RStudio.

``` r
str(df)
```

    ## spc_tbl_ [969 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ language  : chr [1:969] "aal" "aau" "abi" "abt" ...
    ##  $ genus     : chr [1:969] "Biu-Mandara" "Upper Sepik" "Kwa" "Middle Sepik" ...
    ##  $ fam       : chr [1:969] "afas" "sepr" "ncon" "sepr" ...
    ##  $ phonemes  : num [1:969] 36 14 39 24 55 23 39 31 40 25 ...
    ##  $ con       : num [1:969] 27 9 21 17 21 17 23 21 18 18 ...
    ##  $ son       : num [1:969] 6 5 7 12 8 6 8 8 6 6 ...
    ##  $ obs       : num [1:969] 21 4 14 5 13 11 15 13 12 12 ...
    ##  $ vow       : num [1:969] 9 5 18 7 34 6 12 7 22 7 ...
    ##  $ monoph    : num [1:969] 9 5 18 7 17 6 12 7 12 7 ...
    ##  $ qua       : num [1:969] 9 5 9 6 10 6 7 7 7 4 ...
    ##  $ ton       : num [1:969] 0 0 0 0 0 0 4 3 0 0 ...
    ##  $ population: num [1:969] 31000 7270 50500 44000 3500000 16 800000 100000 31800 28900 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   language = col_character(),
    ##   ..   genus = col_character(),
    ##   ..   fam = col_character(),
    ##   ..   phonemes = col_double(),
    ##   ..   con = col_double(),
    ##   ..   son = col_double(),
    ##   ..   obs = col_double(),
    ##   ..   vow = col_double(),
    ##   ..   monoph = col_double(),
    ##   ..   qua = col_double(),
    ##   ..   ton = col_double(),
    ##   ..   population = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>
