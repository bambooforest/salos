Salos 2024 summer school: Linguistic data from fieldwork to R
================
Steven Moran
(20 June, 2024)

- [Introduction](#introduction)

------------------------------------------------------------------------

``` r
library(tidyverse)
```

------------------------------------------------------------------------

# Introduction

From the [BivalTyp website](https://www.bivaltyp.info):

> > > BivalTyp is a typological database of bivalent verbs and their
> > > encoding frames. As of 2024, the database presents data for 129
> > > languages, mainly spoken in Northern Eurasia. The database is
> > > based on a questionnaire containing 130 predicates given in
> > > context. Language-particular encoding frames are identified based
> > > on the devices (such as cases, adpositions, and verbal indices)
> > > involved in encoding two predefined arguments of each predicate
> > > (e.g. ‘Peter’ and ‘the dog’ in ‘Peter is afraid of the dog’). In
> > > each language, one class of verbs is identified as transitive. The
> > > goal of the project is to explore the ways in which bivalent verbs
> > > can be split between the transitive and different intransitive
> > > valency classes.

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
setwd(".")
df <- read_csv('data_for_download.csv')
```

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

2.  You load the file from the working directory to where the data file
    is, e.g.:

``` r
df <- read_csv('data/data_for_download.csv')
```

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

3.  If the data are available online, e.g., in a GitHub repository (or
    elsewhere), you can load the data directly into R/RStudio with the
    `url()` function and a URL:

``` r
df <- read_csv(url('https://raw.githubusercontent.com/macleginn/bivaltyp/master/data/data_for_download.csv'))
```

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

But be careful – it has to be the URL to the **raw data**, not the
webpage! Click on the “Raw” button in GitHub to get the URL in the
browswer window.

<figure>
<img src="figures/raw.png" alt="Open RStudio." />
<figcaption aria-hidden="true">Open RStudio.</figcaption>
</figure>

------------------------------------------------------------------------
