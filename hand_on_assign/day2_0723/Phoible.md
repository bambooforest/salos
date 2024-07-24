Salos 2024 summer school: Exploring PHOIBLE
================
Steven Moran and Alena Witzlack
(23 July, 2024)

- [Introduction](#introduction)
- [Load the data](#load-the-data)
- [Exploring the data](#exploring-the-data)

------------------------------------------------------------------------

These are the R libraries you will need. Add more if you need them.

``` r
library(tidyverse)
```

# Introduction

The [PHOIBLE database](https://phoible.org) is an online repository of
phonological inventory data. From the website one can browse data by
various categories, e.g.:

- [Inventories](https://phoible.org/inventories)
- [Languages](https://phoible.org/languages)
- [Segments](https://phoible.org/parameters)

# Load the data

First load the data. Use this version of the dataset available in this
repository in the [phoible.csv](data/phoible.csv) in the data directory.

``` r
# Insert your code below here

library(readr)
df <- read_csv("~/Library/CloudStorage/OneDrive-UniversityofHelsinki/internship_projects_summerschool/Salo Summer School/source_data/data/phoible.csv")
```

    ## Rows: 969 Columns: 12
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (3): language, genus, fam
    ## dbl (9): phonemes, con, son, obs, vow, monoph, qua, ton, population
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

If you use the `read_csv` function from `tidyverse`, you will want to
convert the character data (columns) into categorical (“factor”) data in
R. You can do this by uncommenting the code (delete the “\#”) below and
either renaming `df` to whatever you called your dataframe or loading
the data with the variable `df` above.

``` r
df$language <- as.factor(df$language)
df$genus <- as.factor(df$genus)
df$fam <- as.factor(df$fam)
```

How do we have a quick look at the dataset’s structure? How many
variables/columns there are? How many rows there are?

``` r
# Insert your code below here
rows <- nrow(df)
cols <- ncol(df)

cat("There are", rows, "rows in this file\n")
```

    ## There are 969 rows in this file

``` r
cat("There are", cols, "columns in this file\n")
```

    ## There are 12 columns in this file

How can we have a quick look at the first few rows (the “head”) of the
data?

``` r
# Insert your code below here
# check the headers
headers <- colnames(data)
cat(paste(headers, collapse = "\n"))
```

How do we get a summary of the contents (i.e., each column) of the data
frame?

``` r
# Insert your code below here
summary(df)
```

    ##     language             genus          fam         phonemes     
    ##  aal    :  1   Bantoid      : 49   ncon   :270   Min.   : 11.00  
    ##  aau    :  1   Oceanic      : 27   anes   : 80   1st Qu.: 26.00  
    ##  abi    :  1   Gur          : 23   ieur   : 54   Median : 34.00  
    ##  abt    :  1   Pama-Nyungan : 16   afas   : 50   Mean   : 34.78  
    ##  ace    :  1   Western Mande: 15   trng   : 49   3rd Qu.: 41.00  
    ##  acv    :  1   (Other)      :669   nsah   : 43   Max.   :141.00  
    ##  (Other):963   NA's         :170   (Other):423                   
    ##       con             son              obs             vow       
    ##  Min.   : 6.00   Min.   : 1.000   Min.   : 3.00   Min.   : 2.00  
    ##  1st Qu.:17.00   1st Qu.: 6.000   1st Qu.:10.00   1st Qu.: 6.00  
    ##  Median :22.00   Median : 8.000   Median :13.00   Median : 9.00  
    ##  Mean   :23.58   Mean   : 8.793   Mean   :14.79   Mean   :10.32  
    ##  3rd Qu.:28.00   3rd Qu.:11.000   3rd Qu.:18.00   3rd Qu.:13.00  
    ##  Max.   :95.00   Max.   :34.000   Max.   :74.00   Max.   :46.00  
    ##                                                                  
    ##      monoph            qua             ton            population       
    ##  Min.   : 2.000   Min.   : 2.00   Min.   : 0.0000   Min.   :        1  
    ##  1st Qu.: 6.000   1st Qu.: 5.00   1st Qu.: 0.0000   1st Qu.:     2680  
    ##  Median : 9.000   Median : 6.00   Median : 0.0000   Median :    30000  
    ##  Mean   : 9.833   Mean   : 6.56   Mean   : 0.8824   Mean   :  3287718  
    ##  3rd Qu.:12.000   3rd Qu.: 8.00   3rd Qu.: 0.0000   3rd Qu.:   300000  
    ##  Max.   :35.000   Max.   :22.00   Max.   :10.0000   Max.   :840000000  
    ## 

# Exploring the data

We can use the `dplyr` functions to explore the data. See the [athletes
file](athletes.md) for examples.

- Select the language, genus, and phoneme count columns.

``` r
# Insert your code below here
genus_phonemes <- select(df, genus, phonemes)
genus_phonemes
```

    ## # A tibble: 969 × 2
    ##    genus        phonemes
    ##    <fct>           <dbl>
    ##  1 Biu-Mandara        36
    ##  2 Upper Sepik        14
    ##  3 Kwa                39
    ##  4 Middle Sepik       24
    ##  5 Malayic            55
    ##  6 Palaihnihan        23
    ##  7 <NA>               39
    ##  8 Kwa                31
    ##  9 Greater Alor       40
    ## 10 Oceanic            25
    ## # ℹ 959 more rows

- Arrange those columns by largest to smallest phoneme inventory size.

``` r
# Insert your code below here
phonemes_from_large <- arrange(df, -phonemes)
phonemes_from_large
```

    ## # A tibble: 969 × 12
    ##    language genus      fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##    <fct>    <fct>      <fct>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ##  1 ktz      Northern … khoi       141    95    21    74    46     24     7     0
    ##  2 hin      Indic      ieur        94    71    16    55    23     19     6     0
    ##  3 aqc      Lezgic     ncau        91    81    32    49    10     10    10     0
    ##  4 yey      Bantoid    ncon        90    77    18    59    13     13     5     0
    ##  5 skr      <NA>       ieur        83    48    15    33    35     18     9     0
    ##  6 bav      Bantoid    ncon        82    56    34    22    17     17     9     9
    ##  7 ary      Semitic    afas        78    74    19    55     4      4     3     0
    ##  8 prk      Palaung-K… ausa        77    34    17    17    43     18    18     0
    ##  9 bkm      Bantoid    ncon        75    59    30    29     9      9     9     7
    ## 10 pan      Indic      ieur        73    50    14    36    20     20    10     3
    ## # ℹ 959 more rows
    ## # ℹ 1 more variable: population <dbl>

- Which language has the most consonants?

``` r
# Insert your code below here
max_con <- arrange(df, -con)
max_con$language[1]
```

    ## [1] ktz
    ## 969 Levels: aal aau abi abt ace acv ada adj adn adz ael aer aey agm agq ... zun

``` r
# ktz has the most consonants. 
# the next line means the languages sorted alphabetically.
```

- Filter all languages that have more than 100 consonants.

``` r
# Insert your code below here
filter(df, con > 100)
```

    ## # A tibble: 0 × 12
    ## # ℹ 12 variables: language <fct>, genus <fct>, fam <fct>, phonemes <dbl>,
    ## #   con <dbl>, son <dbl>, obs <dbl>, vow <dbl>, monoph <dbl>, qua <dbl>,
    ## #   ton <dbl>, population <dbl>

``` r
# no langauges that has consonants more than 100
```

- Which language has the least number of vowels?

``` r
# Insert your code below here
min_vow <- arrange(df, vow)
# you could check the whole data file
min_vow
```

    ## # A tibble: 969 × 12
    ##    language genus      fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##    <fct>    <fct>      <fct>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ##  1 cuv      <NA>       afas        29    27    10    17     2      2     2     0
    ##  2 gnd      <NA>       afas        34    29    11    18     2      2     2     3
    ##  3 aer      Pama-Nyun… aust        30    27    15    12     3      3     3     0
    ##  4 akl      Meso-Phil… anes        20    17     7    10     3      3     3     0
    ##  5 alc      Alacalufan alac        19    16     6    10     3      3     3     0
    ##  6 are      Pama-Nyun… aust        29    26    20     6     3      3     3     0
    ##  7 blc      Bella Coo… sali        31    28     7    21     3      3     3     0
    ##  8 cad      Caddoan    cadd        23    20    11     9     3      3     3     0
    ##  9 dbl      Pama-Nyun… aust        16    13     9     4     3      3     3     0
    ## 10 duj      Pama-Nyun… aust        23    20    13     7     3      3     3     0
    ## # ℹ 959 more rows
    ## # ℹ 1 more variable: population <dbl>

``` r
#initialize the least number of vowels
least_vow <- min_vow$vow[1]
least_vow
```

    ## [1] 2

``` r
# filter the languages that have the same vowels as the one that has least vowel
filter(min_vow, vow == least_vow)
```

    ## # A tibble: 2 × 12
    ##   language genus fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##   <fct>    <fct> <fct>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 cuv      <NA>  afas        29    27    10    17     2      2     2     0
    ## 2 gnd      <NA>  afas        34    29    11    18     2      2     2     3
    ## # ℹ 1 more variable: population <dbl>

``` r
# cuv and gnd are the languages that have the least number
```

- Filter all languages that have less than 3 vowels.

``` r
# Insert your code below here
filter(min_vow, vow < 3)
```

    ## # A tibble: 2 × 12
    ##   language genus fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##   <fct>    <fct> <fct>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 cuv      <NA>  afas        29    27    10    17     2      2     2     0
    ## 2 gnd      <NA>  afas        34    29    11    18     2      2     2     3
    ## # ℹ 1 more variable: population <dbl>

``` r
# the languages are cuv and gnd
```

- Which language has the most number of tones?

``` r
# Insert your code below here
# sort the data frame from largest tones
max_ton <- arrange(df, -ton)

# initialize the biggest one
most_ton <- max_ton$ton[1]

#filter the other if have the same as the biggest one
filter(max_ton, ton == most_ton)
```

    ## # A tibble: 1 × 12
    ##   language genus   fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##   <fct>    <fct>   <fct>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 bfd      Bantoid ncon        49    21     7    14    18     18     9    10
    ## # ℹ 1 more variable: population <dbl>

``` r
# there is only bfd language that has the most tones
```

- How many languages do not have tone?

``` r
# Insert your code below here

# if ton == 0, update the counter by one
no_ton_count = 0
for (tone in max_ton$ton) {
  if (tone == 0) {
    no_ton_count <- no_ton_count + 1
  }
}

no_ton_count
```

    ## [1] 728

``` r
# another way to calculate is to use filter
filter(df, ton == 0)
```

    ## # A tibble: 728 × 12
    ##    language genus      fam   phonemes   con   son   obs   vow monoph   qua   ton
    ##    <fct>    <fct>      <fct>    <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ##  1 aal      Biu-Manda… afas        36    27     6    21     9      9     9     0
    ##  2 aau      Upper Sep… sepr        14     9     5     4     5      5     5     0
    ##  3 abi      Kwa        ncon        39    21     7    14    18     18     9     0
    ##  4 abt      Middle Se… sepr        24    17    12     5     7      7     6     0
    ##  5 ace      Malayic    anes        55    21     8    13    34     17    10     0
    ##  6 acv      Palaihnih… hoka        23    17     6    11     6      6     6     0
    ##  7 adn      Greater A… trng        40    18     6    12    22     12     7     0
    ##  8 adz      Oceanic    anes        25    18     6    12     7      7     4     0
    ##  9 aer      Pama-Nyun… aust        30    27    15    12     3      3     3     0
    ## 10 aey      Madang     trng        20    15     4    11     5      5     5     0
    ## # ℹ 718 more rows
    ## # ℹ 1 more variable: population <dbl>

- How many languages are there in the sample?

``` r
# Insert your code below here

un_lan <- length(unique(df$language))
un_lan
```

    ## [1] 969

- How many language families are there in the sample?

``` r
# Insert your code below here

un_fam <- length(unique(df$fam))
un_fam
```

    ## [1] 100

- Use the mutate function to determine the average (mean) number of
  phonemes across all languages in the sample.

``` r
# Insert your code below here
# calculate the mean of phonemes of every languages
mean_pho <- select(df, phonemes)
```

- Do the same for consonants, vowels, and tones.

``` r
# Insert your code below here
```

- Can you do the same “by group”, i.e., by language family?

``` r
# Insert your code below here
```

- How about by language genus? (What’s the difference?)

``` r
# Insert your code below here
```

- Are there any NAs in the table?

``` r
# Insert your code below here
```

What other questions can you ask of the data with the tools that you’ve
learned about?
