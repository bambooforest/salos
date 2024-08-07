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
```

If you use the `read_csv` function from `tidyverse`, you will want to
convert the character data (columns) into categorical (“factor”) data in
R. You can do this by uncommenting the code (delete the “\#”) below and
either renaming `df` to whatever you called your dataframe or loading
the data with the variable `df` above.

``` r
#df$language <- as.factor(df$language)
#df$genus <- as.factor(df$genus)
#df$fam <- as.factor(df$fam)
```

How do we have a quick look at the dataset’s structure? How many
variables/columns there are? How many rows there are?

``` r
# Insert your code below here
```

How can we have a quick look at the first few rows (the “head”) of the
data?

``` r
# Insert your code below here
```

How do we get a summary of the contents (i.e., each column) of the data
frame?

``` r
# Insert your code below here
```

# Exploring the data

We can use the `dplyr` functions to explore the data. See the [athletes
file](athletes.md) for examples.

- Select the language, genus, and phoneme count columns.

``` r
# Insert your code below here
```

- Arrange those columns by largest to smallest phoneme inventory size.

``` r
# Insert your code below here
```

- Which language has the most consonants?

``` r
# Insert your code below here
```

- Filter all languages that have more than 100 consonants.

``` r
# Insert your code below here
```

- Which language has the least number of vowels?

``` r
# Insert your code below here
```

- Filter all languages that have less than 3 vowels.

``` r
# Insert your code below here
```

- Which language has the most number of tones?

``` r
# Insert your code below here
```

- How many languages do not have tone?

``` r
# Insert your code below here
```

- How many languages are there in the sample?

``` r
# Insert your code below here
```

- How many language families are there in the sample?

``` r
# Insert your code below here
```

- Use the mutate function to determine the average (mean) number of
  phonemes across all languages in the sample.

``` r
# Insert your code below here
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
