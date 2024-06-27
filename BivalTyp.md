Salos 2024 summer school: Working with BivalTyp
================
Steven Moran and Alena Witzlack-Makarevich
(27 June, 2024)

- [Introduction](#introduction)
- [Getting the data](#getting-the-data)
- [Explore the data](#explore-the-data)
- [`select` what you need](#select-what-you-need)
- [`mutate()` character into factor](#mutate-character-into-factor)
- [Get `languages`](#get-languages)
- [`join()` some tables](#join-some-tables)
- [Work with one language](#work-with-one-language)
- [Stack some Germanic languages](#stack-some-germanic-languages)
- [Explore another set of langauges](#explore-another-set-of-langauges)
- [`join()` predicate and data tables](#join-predicate-and-data-tables)

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

    ## spc_tbl_ [17,160 × 15] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ language_no                  : num [1:17160] 60 60 60 60 60 60 60 60 60 60 ...
    ##  $ predicate_no                 : num [1:17160] 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ verb                         : chr [1:17160] "χ’ə" "*" "[ĉ-]ŝa" "w-š’tə" ...
    ##  $ X                            : chr [1:17160] "IO" "*" "ABS" "ERG" ...
    ##  $ Y                            : chr [1:17160] "ABS" "*" "MAL" "ABS" ...
    ##  $ locus                        : chr [1:17160] "X" "*" "Y" "TR" ...
    ##  $ valency_pattern              : chr [1:17160] "IO_ABS" NA "ABS_MAL" "TR" ...
    ##  $ sentence                     : chr [1:17160] "á-č̣’ḳʷən j-qá jə́-χ’-əj-ṭ" "*" "á-č̣’ḳʷən a-lá d-a-ĉ-ŝ-ə́j-ṭ" "á-č̣’ḳʷən-ĉa-kʷa a-háqʷ-kʷa j-á-wə-r-š’t-əj-ṭ" ...
    ##  $ glosses_en                   : chr [1:17160] "DEF-boy 3SG.M.IO-head 3SG.M.IO-ache-PRS-DCL" "*" "DEF-boy DEF-dog 3SG.H.ABS-3SG.N.IO-MAL-be_afraid-PRS-DCL" "DEF-boy-PL.H-PL DEF-stone-PL 3PL.ABS-3N.IO-LOC-3PL.ERG-throw-PRS-DCL" ...
    ##  $ back_translation_en          : chr [1:17160] "‘The boy has a headache.’" "*" "‘The boy is afraid of the dog.’" "‘The boys are throwing the stones.’" ...
    ##  $ comms                        : chr [1:17160] NA "No satisfactory translation has been obtained." NA NA ...
    ##  $ glosses_ru                   : chr [1:17160] "DEF-парень 3SG.M.IO-голова 3SG.M.IO-болеть-PRS-DCL" "*" "DEF-парень DEF-собака 3SG.H.ABS-3SG.N.IO-MAL-бояться-PRS-DCL" "DEF-парень-PL.H-PL DEF-камень-PL 3PL.ABS-3N.IO-LOC-3PL.ERG-бросить-PRS-DCL" ...
    ##  $ back_translation_ru          : chr [1:17160] "‘У парня болит голова.’" "*" "‘Парень боится собаки.’" "‘Мальчики бросают камни.’" ...
    ##  $ verb_original_orthography    : chr [1:17160] "хьы" "*" "[чв-]шва" "ау-щты" ...
    ##  $ sentence_original_orthography: chr [1:17160] "А-чIкIвын й-хъА йЫ-хь-и-тI" "*" "А-чIкIвын а-лА д-а-чв-шв-И-тI" "А-чIкIвын-чва-ква а-хIАхъв-ква й-А-уы-р-щт-и-тI" ...
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
  which we probably won’t need for it?

(When a dataset has many variables it might be more convenient to work
with a smaller version of it containing just what you need.)

# `select` what you need

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
what you wanted. Notice an optional argument `n = 3` added to the
function `head()`. What does it do? Verify your intuition by changing
the value to e.g. `n = 5`.

``` r
head(valency, n = 3)
```

    ## # A tibble: 3 × 6
    ##   language_no predicate_no X     Y     locus valency_pattern
    ##         <dbl>        <dbl> <chr> <chr> <chr> <chr>          
    ## 1          60            1 IO    ABS   X     IO_ABS         
    ## 2          60            2 *     *     *     <NA>           
    ## 3          60            3 ABS   MAL   Y     ABS_MAL

# `mutate()` character into factor

To get a first idea of how much of everything is in the dataset, the
function `summary()` is quite useful:

``` r
summary(valency)
```

    ##   language_no      predicate_no        X                  Y            
    ##  Min.   :  1.00   Min.   :  1.0   Length:17160       Length:17160      
    ##  1st Qu.: 33.75   1st Qu.: 33.0   Class :character   Class :character  
    ##  Median : 66.50   Median : 65.5   Mode  :character   Mode  :character  
    ##  Mean   : 66.50   Mean   : 65.5                                        
    ##  3rd Qu.: 99.25   3rd Qu.: 98.0                                        
    ##  Max.   :132.00   Max.   :130.0                                        
    ##     locus           valency_pattern   
    ##  Length:17160       Length:17160      
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ## 

As it turns out, `summary()` on character data (our `X`, `Y`, `locus`,
and `valency_pattern`) does not yield much of use. Why is this the case?

In R, there is an important distinction between the two data types for
storing textual data (i.e. data with words): character and factor.

We use **character** for textual data which do not represent
categories/classes, e.g. example sentences and their glosses in the
original large valency dataset. Textual data which represent classes or
categories should be stored as **factors** in R and not as character
data type.

For instance, in a patients dataset, patient names would be stored as
characters, as we don’t really care how many `John`’s and `Rose`’s are
there and whether they are more frequent than `Lee`’s and `Monica`’s. On
the other hand, `male` and `female` are categories from a list which
includes these and other possibilities which we would like to be able to
count for statistical analysis, for this reason we treat them as factors
in R.

Different functions to import data into R have different default
specifications: some generously treat all words as factors, others
conservatively treat all words as characters. What does the function
`read_tsv()` (or `read_csv()`) do? It assume that all words are
character and then it is up to you to declare which ones should be
factors. Let’s declare that `X`, `Y`, `locus`, and `valency_pattern` are
actually factors:

Let’s mutate to factor the variable `X`

``` r
valency <- valency %>% mutate(X = factor(X))
```

Compare the now factor `X` to the still character variable `Y`.

``` r
summary(valency)
```

    ##   language_no      predicate_no         X             Y            
    ##  Min.   :  1.00   Min.   :  1.0   NOM    :7578   Length:17160      
    ##  1st Qu.: 33.75   1st Qu.: 33.0   SBJ    :3780   Class :character  
    ##  Median : 66.50   Median : 65.5   ERG    :1877   Mode  :character  
    ##  Mean   : 66.50   Mean   : 65.5   *      :1438                     
    ##  3rd Qu.: 99.25   3rd Qu.: 98.0   DAT    : 794                     
    ##  Max.   :132.00   Max.   :130.0   ABS    : 486                     
    ##                                   (Other):1207                     
    ##     locus           valency_pattern   
    ##  Length:17160       Length:17160      
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ##                                       
    ## 

Let’s change (mutate) the other character variables into factor and have
a look at their summary:

``` r
valency <- valency %>% mutate(Y = factor(Y), locus = factor(locus), valency_pattern = factor(valency_pattern))
```

``` r
summary(valency)
```

    ##   language_no      predicate_no         X              Y        locus    
    ##  Min.   :  1.00   Min.   :  1.0   NOM    :7578   ACC    :2766   * :1438  
    ##  1st Qu.: 33.75   1st Qu.: 33.0   SBJ    :3780   DO     :1937   TR:7307  
    ##  Median : 66.50   Median : 65.5   ERG    :1877   NOM    :1725   X :1306  
    ##  Mean   : 66.50   Mean   : 65.5   *      :1438   *      :1438   XY: 349  
    ##  3rd Qu.: 99.25   3rd Qu.: 98.0   DAT    : 794   DAT    :1075   Y :6760  
    ##  Max.   :132.00   Max.   :130.0   ABS    : 486   ABS    : 823            
    ##                                   (Other):1207   (Other):7396            
    ##  valency_pattern
    ##  TR     :7305   
    ##  NOM_DAT: 755   
    ##  DAT_NOM: 540   
    ##  NOM_INS: 270   
    ##  NOM_ABL: 265   
    ##  (Other):6587   
    ##  NA's   :1438

It’s getting more interesting, but at this stage you probably realize
that we have no idea what languages and what predicates you are dealing
with in this table. These details are part of two separate datasets and
before we embark on any serious exploration, we need to join these
datasets.

# Get `languages`

The only information on languages in our `valency` is some kind of ID
under `language_no`, but what is language number `1`?

Let’s get the dataset with the detail on languages. We use the same
function to read the dataset and process it a bit following the same
procedure as above:

What do we get?

``` r
head(languages)
```

    ## # A tibble: 6 × 23
    ##   language_no language_ru     language language_external expert_ru consultant_ru
    ##         <dbl> <chr>           <chr>    <chr>             <chr>     <chr>        
    ## 1           1 русский         Russian  Russian           Сергей С… <NA>         
    ## 2           2 арабский литер… Arabic_… Standard Arabic   Рамазан … <NA>         
    ## 3           3 гуарани         Guarani… Paraguayan Guara… Дмитрий … <NA>         
    ## 4           4 эстонский       Estonian Estonian          Мерит Ни… <NA>         
    ## 5           5 цахурский       Tsakhur  Tsakhur           Жиль Отье Ахмед Давудов
    ## 6           6 тувинский       Tuvinian Tuvinian          Софья Ал… <NA>         
    ## # ℹ 17 more variables: expert <chr>, consultant <chr>,
    ## #   data_collection_year <chr>, initial_release_date <chr>,
    ## #   last_release_date <chr>, macroarea <chr>, `family (WALS)` <chr>,
    ## #   `genus (WALS)` <chr>, latitude <dbl>, longitude <dbl>,
    ## #   number_nominal_cases <dbl>, source_of_information <chr>,
    ## #   contact_language <chr>, glottocode <chr>, comment_on_glottocode <chr>,
    ## #   WO_WALS <chr>, WO_comment <chr>

There is definitely too much stuff here we are unlikely to use, let’s
`select()` only some variables:

``` r
languages <- languages %>% select(language_no, language, macroarea, glottocode, latitude, longitude)
```

# `join()` some tables

We want to join our two data frames `languages` and `valency`. They
share one variable `language_no`, which we will use to perform the join.

There are various options for joining data frames. The choice matters
when the data frames do not have overlapping set. This is not the case
in our data set, so the various functions will yiled the same result.
But in case you are curious:

- a `left_join()` keeps all observations in data frames x,

- a `right_join()` keeps all observations in data frames,

- a `full_join()` keeps all observations in x and y.

Let’s take `left_join()` and check our enhanced data frame:

``` r
valency <- left_join(languages, valency, by = "language_no")
head(valency)
```

    ## # A tibble: 6 × 11
    ##   language_no language macroarea glottocode latitude longitude predicate_no
    ##         <dbl> <chr>    <chr>     <chr>         <dbl>     <dbl>        <dbl>
    ## 1           1 Russian  Europe    russ1263         56        38            1
    ## 2           1 Russian  Europe    russ1263         56        38            2
    ## 3           1 Russian  Europe    russ1263         56        38            3
    ## 4           1 Russian  Europe    russ1263         56        38            4
    ## 5           1 Russian  Europe    russ1263         56        38            5
    ## 6           1 Russian  Europe    russ1263         56        38            6
    ## # ℹ 4 more variables: X <fct>, Y <fct>, locus <fct>, valency_pattern <fct>

Now that we have some idea about the langauges in our dataset, we can
explore them a bit further.

# Work with one language

Let’s look at some valency details for a langauge or two. `filter()`
allows you to access only those data which fullfil the specified
conditions (e.g. `==` means it has to exactly matching `"English"`)

``` r
valency %>% filter(language == "English") %>% count(locus)
```

    ## # A tibble: 3 × 2
    ##   locus     n
    ##   <fct> <int>
    ## 1 *         3
    ## 2 TR       81
    ## 3 Y        46

Is English any different form German?

``` r
valency %>% filter(language == "German") %>% count(locus)
```

    ## # A tibble: 4 × 2
    ##   locus     n
    ##   <fct> <int>
    ## 1 *         1
    ## 2 TR       71
    ## 3 X         5
    ## 4 Y        53

It is! What about some other Germanic langauges?

# Stack some Germanic languages

Here, a picture (a plot) might do a better job. Stacked barplots is one
option.

``` r
valency %>% filter(language %in% c("English", "German", "Icelandic", "Dutch")) %>% 
      ggplot(aes(x = language, fill = locus)) + 
      geom_bar()
```

![](BivalTyp_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

Interprete the plot

- Which language is the most diverse in terms of valency patterns
  (types)?

- Which language is most transitive (with respect to the 130 meanings in
  the sample)?

- Which language is the least transitive?

# Explore another set of langauges

- Select another set of languages and compare the distributions of their
  valency patterns.

# `join()` predicate and data tables
