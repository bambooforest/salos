Salos 2024 summer school: Working with PHOIBLE
================
Steven Moran and Alena Witzlack
(21 June, 2024)

- [Introduction](#introduction)
- [Tabular data](#tabular-data)

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

How do we have a quick look at it?

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
what it has in its columns and rows?

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

# Tabular data

In this course, we are mainly going to be dealing with data in [plain
text](https://en.wikipedia.org/wiki/Plain_text) and structured data in
rectangular format, also known as **tabular data**. Here are some
descriptions:

- <https://en.wikipedia.org/wiki/Table_(information)>
- <https://papl.cs.brown.edu/2016/intro-tabular-data.html>
- <https://www.w3.org/TR/tabular-data-model/>

Tabular data **can be stored in many ways**, e.g.:

- [CSV](https://en.wikipedia.org/wiki/Comma-separated_values)
- [Excel sheets](https://en.wikipedia.org/wiki/Microsoft_Excel)
- [Google sheets](https://en.wikipedia.org/wiki/Google_Sheets)
- [Numbers](https://en.wikipedia.org/wiki/Numbers_(spreadsheet))
- [SQLite](https://en.wikipedia.org/wiki/SQLite)
- [JSON](https://en.wikipedia.org/wiki/JSON)

Which of these formats above are stored in plain text?

In a table data format, every column represents a particular variable
(e.g., a person’s height, number of of vowels) and each row/record
corresponds to a given member of the data set in question (e.g. a
person, in a language). Tabular data are inherently rectangular and
cannot have “ragged rows”. If any row is lacking information for a
particular column, a missing value (`NA`) is stored in that cell.

For most people working with small amounts of data, the data table is
the fundamental unit of organization because it is both a way of
organizing data that can be processed by humans and machines. In
practice, to enter, organize, modify, analyze, and store data in tabular
form – it is common for people to use spreadsheet applications. You are
probably familiar for example with Excel spreadsheets.

Many statistical software packages use similar spreadsheets and many are
able to import Excel spreadsheets. R is no different.

------------------------------------------------------------------------

Tabular (or table) data has several properties. It consists of [rows and
columns](https://en.wikipedia.org/wiki/Row_and_column_vectors) in the
linear algebra sense, and
[rows](https://en.wikipedia.org/wiki/Row_(database)) and
[columns](https://en.wikipedia.org/wiki/Column_(database)) in the
relational database sense.

[Columns](https://en.wikipedia.org/wiki/Column_(database)) in tabular
data contain a set of data of a particular type and contain (typically)
one value (data type – see above) for each row in the table.

Each [row](https://en.wikipedia.org/wiki/Row_(database)) in the table
contains an observation, in which each row represents a set of related
data, i.e., every row has the same structure and each cell in each row
should adhere to the column’s specification (i.e., that data type of
that column).

![Variables and observations in a table](figures/table_example.png) In
sum:

- Every **column** represents a particular variable (height, weight,
  number of vowels).

- Each **row/record** corresponds to a given member of the data set in
  question (e.g. a person, a sentence, a language, a phoneme, a
  measurement).

- Every record shares the same set of variables, i.e., **every row has
  the same set of column headers**.

- Tabular data are **inherently rectangular** and cannot have “ragged
  rows”.

- Each value (each cell) is **known as a datum**.

- If any row is lacking information for a particular column a **missing
  value (NA)** must be stored in that cell.

------------------------------------------------------------------------
