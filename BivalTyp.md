Salos 2024 summer school: Working with BivalTyp
================
Steven Moran and Alena Witzlack
(21 June, 2024)

- [Introduction](#introduction)
- [Getting the data](#getting-the-data)
- [Explore the data](#explore-the-data)
- [Select what you need with
  \`select()\`\`](#select-what-you-need-with-select)
- [Join tables](#join-tables)

------------------------------------------------------------------------

``` r
library(tidyverse)
library(readxl)
library(readr)
require(forcats)
library(cowplot)
library(knitr)
library(kableExtra)
```

# Introduction

From the [BivalTyp website](https://www.bivaltyp.info):

> BivalTyp is a typological database of bivalent verbs and their
> encoding frames. As of 2024, the database presents data for 129
> languages, mainly spoken in Northern Eurasia. The database is based on
> a questionnaire containing 130 predicates given in context.
> Language-particular encoding frames are identified based on the
> devices (such as cases, adpositions, and verbal indices) involved in
> encoding two predefined arguments of each predicate (e.g. ‘Peter’ and
> ‘the dog’ in ‘Peter is afraid of the dog’). In each language, one
> class of verbs is identified as transitive. The goal of the project is
> to explore the ways in which bivalent verbs can be split between the
> transitive and different intransitive valency classes.

# Getting the data

The data from BivalTyp are available in a GitHub repository created and
maintained by [Dmitry Nikolayev](https://dnikolaev.com):

- <https://github.com/macleginn/bivaltyp>

First, let’s figure out where the [raw
data](https://en.wikipedia.org/wiki/Raw_data) are.

------------------------------------------------------------------------

Here we find a number of [CSV tabular
data](https://en.wikipedia.org/wiki/Comma-separated_values) files:

- <https://github.com/macleginn/bivaltyp/tree/master/data>

What are in these data files? Let’s explore them.

------------------------------------------------------------------------

OK, now how do we load the data into R/RStudio?

------------------------------------------------------------------------

One way is to download them to your computer.

<figure>
<img src="figures/download.png" alt="Open RStudio." />
<figcaption aria-hidden="true">Open RStudio.</figcaption>
</figure>

But then you need to make sure that they are:

1.  Either in the same folder as the Rmd file you are working on (which
    means you’ve set the working directory to where you Rmd file is):

``` r
valency <- read_tsv('data_for_download.csv')
```

2.  You load the file from the working directory to where the data file
    is, e.g.:

``` r
valency <- read_tsv('data/data_for_download.csv')
```

3.  If the data are available online, e.g., in a GitHub repository (or
    elsewhere), you can load the data directly into R/RStudio with the
    `url()` function and a URL:

``` r
valency <- read_tsv(url('https://raw.githubusercontent.com/macleginn/bivaltyp/master/data/data_for_download.csv'))
```

But be careful – it has to be the URL to the **raw data**, not the
webpage! Click on the “Raw” button in GitHub to get the URL in the
browswer window.

![Open RStudio.](figures/raw.png) Another issue to remember – although
the file is labeled CSV for “comma separated values”, the actual file is
separated by tabs. Linguists often use TSV (i.e., tab separated values)
for display purposes.

In R/RStudio you can specify the
[delimiter](https://en.wikipedia.org/wiki/Delimiter), e.g., that it is
tab instead of comma. Comma is the default standard, but in the wild you
will come across many different characters as delimiters.

Finally, notice, the file we started with is called
`data_for_download.csv`. This is which is not a very informative label
for an R object. We pick a more telling name, e.g. `valency`.

------------------------------------------------------------------------

# Explore the data

Every time you read in a data set, it’s a good idea to have a look at it
from a few angles to make sure that there are no major issues with both
the import and the data themselves before you move on to doing some real
statistics.

Let’s start by looking at the variables with the function `str()` (it
stands for “structure”):

``` r
str(valency)
```

    ## spc_tbl_ [16,770 × 15] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ language_no                  : num [1:16770] 60 60 60 60 60 60 60 60 60 60 ...
    ##  $ predicate_no                 : num [1:16770] 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ verb                         : chr [1:16770] "χ’ə" "*" "[ĉ-]ŝa" "w-š’tə" ...
    ##  $ X                            : chr [1:16770] "IO" "*" "ABS" "ERG" ...
    ##  $ Y                            : chr [1:16770] "ABS" "*" "MAL" "ABS" ...
    ##  $ locus                        : chr [1:16770] "X" "*" "Y" "TR" ...
    ##  $ valency_pattern              : chr [1:16770] "IO_ABS" NA "ABS_MAL" "TR" ...
    ##  $ sentence                     : chr [1:16770] "á-č̣’ḳʷən j-qá jə́-χ’-əj-ṭ" "*" "á-č̣’ḳʷən a-lá d-a-ĉ-ŝ-ə́j-ṭ" "á-č̣’ḳʷən-ĉa-kʷa a-háqʷ-kʷa j-á-wə-r-š’t-əj-ṭ" ...
    ##  $ glosses_en                   : chr [1:16770] "DEF-boy 3SG.M.IO-head 3SG.M.IO-ache-PRS-DCL" "*" "DEF-boy DEF-dog 3SG.H.ABS-3SG.N.IO-MAL-be_afraid-PRS-DCL" "DEF-boy-PL.H-PL DEF-stone-PL 3PL.ABS-3N.IO-LOC-3PL.ERG-throw-PRS-DCL" ...
    ##  $ back_translation_en          : chr [1:16770] "‘The boy has a headache.’" "*" "‘The boy is afraid of the dog.’" "‘The boys are throwing the stones.’" ...
    ##  $ comms                        : chr [1:16770] NA "No satisfactory translation has been obtained." NA NA ...
    ##  $ glosses_ru                   : chr [1:16770] "DEF-парень 3SG.M.IO-голова 3SG.M.IO-болеть-PRS-DCL" "*" "DEF-парень DEF-собака 3SG.H.ABS-3SG.N.IO-MAL-бояться-PRS-DCL" "DEF-парень-PL.H-PL DEF-камень-PL 3PL.ABS-3N.IO-LOC-3PL.ERG-бросить-PRS-DCL" ...
    ##  $ back_translation_ru          : chr [1:16770] "‘У парня болит голова.’" "*" "‘Парень боится собаки.’" "‘Мальчики бросают камни.’" ...
    ##  $ verb_original_orthography    : chr [1:16770] "хьы" "*" "[чв-]шва" "ау-щты" ...
    ##  $ sentence_original_orthography: chr [1:16770] "А-чIкIвын й-хъА йЫ-хь-и-тI" "*" "А-чIкIвын а-лА д-а-чв-шв-И-тI" "А-чIкIвын-чва-ква а-хIАхъв-ква й-А-уы-р-щт-и-тI" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   language_no = col_double(),
    ##   ..   predicate_no = col_double(),
    ##   ..   verb = col_character(),
    ##   ..   X = col_character(),
    ##   ..   Y = col_character(),
    ##   ..   locus = col_character(),
    ##   ..   valency_pattern = col_character(),
    ##   ..   sentence = col_character(),
    ##   ..   glosses_en = col_character(),
    ##   ..   back_translation_en = col_character(),
    ##   ..   comms = col_character(),
    ##   ..   glosses_ru = col_character(),
    ##   ..   back_translation_ru = col_character(),
    ##   ..   verb_original_orthography = col_character(),
    ##   ..   sentence_original_orthography = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

Another way to get some sense of the data is to use the function
`head()`:

``` r
head(valency)
```

    ## # A tibble: 6 × 15
    ##   language_no predicate_no verb   X     Y     locus valency_pattern sentence    
    ##         <dbl>        <dbl> <chr>  <chr> <chr> <chr> <chr>           <chr>       
    ## 1          60            1 χ’ə    IO    ABS   X     IO_ABS          á-č̣’ḳʷən j-…
    ## 2          60            2 *      *     *     *     <NA>            *           
    ## 3          60            3 [ĉ-]ŝa ABS   MAL   Y     ABS_MAL         á-č̣’ḳʷən a-…
    ## 4          60            4 w-š’tə ERG   ABS   TR    TR              á-č̣’ḳʷən-ĉa…
    ## 5          60            5 [z-]qa BEN   ABS   X     BEN_ABS         wəẑə́ zaréma…
    ## 6          60            6 apš    ABS   IO    Y     ABS_IO          wará s-aχš’…
    ## # ℹ 7 more variables: glosses_en <chr>, back_translation_en <chr>, comms <chr>,
    ## #   glosses_ru <chr>, back_translation_ru <chr>,
    ## #   verb_original_orthography <chr>, sentence_original_orthography <chr>

Could you guess what the “sibling” function `tail()` does?

Interpret the output of `str()` and `head()`:

- How many variables are in the dataset?

- Variables of which type are these?

- How many rows does it contain?

- Which variables are you likely to use for statistical analysis and
  which we probably won’t need for it? (When a dataset has many
  variables it might be more convenient to work with a smaller version
  of it containing just what you need.)

# Select what you need with \`select()\`\`

The dataset `valency` contains a lot of textual data (examples and their
translations), which won’t be used for any statistical analysis.

Let’s use the `tidyverse` function `select` and select only the
variables you might need for further analysis: you just list them one
after another as arguments of the function `select()`.

Here we overwrite the object `valency`. (Do not panic, you always have
access to the original dataset in case you need it.)

``` r
valency <- valency  %>% select(language_no, predicate_no, X, Y, locus, valency_pattern)
```

It’s a good idea to check with the familiar functions whether you got
what you wanted. Notice an optional argument `n=3` added tot he function
`head()`. What does it do? Verify your intuition by changing the value
to e.g. `n = 5`.

``` r
head(valency, n = 3)
```

    ## # A tibble: 3 × 6
    ##   language_no predicate_no X     Y     locus valency_pattern
    ##         <dbl>        <dbl> <chr> <chr> <chr> <chr>          
    ## 1          60            1 IO    ABS   X     IO_ABS         
    ## 2          60            2 *     *     *     <NA>           
    ## 3          60            3 ABS   MAL   Y     ABS_MAL

# Join tables
