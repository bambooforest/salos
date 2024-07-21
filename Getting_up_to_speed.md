Getting up to speed
================
Steven Moran and Alena Witzlack-Makarevich
(21 July, 2024)

- [Table data](#table-data)
- [Wide vs long table formats](#wide-vs-long-table-formats)
  - [Wide](#wide)
  - [Long](#long)
  - [“Tidy data” in R](#tidy-data-in-r)
- [Data wrangling](#data-wrangling)
  - [tidyverse](#tidyverse)
  - [Loading data](#loading-data)
  - [Accessing and mainpulating data](#accessing-and-mainpulating-data)
  - [Some examples](#some-examples)
    - [`select()`](#select)
    - [`arrange()`](#arrange)
    - [`mutate()`](#mutate)
    - [`summarize()`](#summarize)
    - [`group_by()`](#group_by)
    - [`filter()`](#filter)
  - [Filtering NA](#filtering-na)
  - [The table function](#the-table-function)
  - [Some practice problems](#some-practice-problems)
- [Data types](#data-types)
- [Data types in programming vs
  statistics](#data-types-in-programming-vs-statistics)
- [Data types in statistics](#data-types-in-statistics)
  - [Qualitative versus quantitative
    variables](#qualitative-versus-quantitative-variables)
- [Data types in R](#data-types-in-r)
- [Data structures in R](#data-structures-in-r)
- [Cheat sheets](#cheat-sheets)

------------------------------------------------------------------------

These are the R libraries you will need for this report:

``` r
# install.packages('tidyverse')
# install.packages('knitr')
library(tidyverse)
library(knitr)
```

If you have not installed these libraries before, then you need to run
the `install.packages()` command **once** before you load them above,
i.e., `install.packages('tidyverse')` in the Console below.

Or you can use the menu above in RStudio: Click on Tools \> Install
Packages.

------------------------------------------------------------------------

# Table data

In this course, we are mainly going to be dealing with data in [plain
text](https://en.wikipedia.org/wiki/Plain_text) and structured data in
rectangular format, also known as **tabular data**. Here are some
descriptions ([1](https://en.wikipedia.org/wiki/Table_(information)),
[2](https://papl.cs.brown.edu/2016/intro-tabular-data.html),
[3](https://www.w3.org/TR/tabular-data-model/)).

Tabular data **can be stored in many ways**, e.g.:

- [CSV](https://en.wikipedia.org/wiki/Comma-separated_values)
- [Excel sheets](https://en.wikipedia.org/wiki/Microsoft_Excel)
- [Google sheets](https://en.wikipedia.org/wiki/Google_Sheets)
- [Numbers](https://en.wikipedia.org/wiki/Numbers_(spreadsheet))
- [SQLite](https://en.wikipedia.org/wiki/SQLite)
- [JSON](https://en.wikipedia.org/wiki/JSON)

Which of these formats above are stored in plain text?

------------------------------------------------------------------------

In a table data format, every:

- **Column** represents a particular variable (e.g., a person’s height,
  number of of vowels)

- **Row (record)** corresponds to a given member of the data set in
  question (e.g. a person, in a language).

If any row is lacking information for a particular column, a missing
value (`NA`) is stored in that cell.

<figure>
<img src="figures/table_example.png"
alt="Variables and observations in a table" />
<figcaption aria-hidden="true">Variables and observations in a
table</figcaption>
</figure>

------------------------------------------------------------------------

Are there any missing values above?

<!--
Tabular data are inherently rectangular and cannot have "[ragged rows](https://en.wikipedia.org/wiki/Irregular_matrix)". 
-->

------------------------------------------------------------------------

For most people working with small amounts of data, the data table is
the fundamental unit of organization because it is both a way of
organizing data that can be processed by humans and machines.

In practice, to enter, organize, modify, analyze, and store data in
tabular form – it is common for people to use spreadsheet applications.
You are probably familiar with Excel spreadsheets.

Many statistical software packages use similar spreadsheets and many are
able to import Excel spreadsheets. R is no different.

------------------------------------------------------------------------

Table data has several properties. It consists of [rows and
columns](https://en.wikipedia.org/wiki/Row_and_column_vectors) in the
linear algebra sense, and
[rows](https://en.wikipedia.org/wiki/Row_(database)) and
[columns](https://en.wikipedia.org/wiki/Column_(database)) in the
relational database sense.

- [Columns](https://en.wikipedia.org/wiki/Column_(database)) in tabular
  data contain a set of data of a particular type and contain
  (typically) one value (data type – see above) for each row in the
  table.

- Each [row](https://en.wikipedia.org/wiki/Row_(database)) in the table
  contains an observation, in which each row represents a set of related
  data, i.e., every row has the same structure and each cell in each row
  should adhere to the column’s specification (i.e., that data type of
  that column).

------------------------------------------------------------------------

In sum:

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

# Wide vs long table formats

There are two basic presentations of tabular data:

- Wide
- Long (aka narrow)

## Wide

[Wide](https://en.wikipedia.org/wiki/Wide_and_narrow_data) tabular data
is unstacked and it is presented so that each different data variable is
in a separate column.

``` r
# This is code to create a data frame (table) in R
df_wide <- data.frame(Person = c('Bob', 'Alice', 'Steve'),
                 Age = c(32, 24, 64),
                 Weight = c(168, 150, 144),
                 Height = c(180, 175, 165))

# This is one way of viewing that table
df_wide %>% kable()
```

| Person | Age | Weight | Height |
|:-------|----:|-------:|-------:|
| Bob    |  32 |    168 |    180 |
| Alice  |  24 |    150 |    175 |
| Steve  |  64 |    144 |    165 |

<!-- if you want to do it by hand in .md insert this:
&#10;| Person | Age | Weight | Height |
|----------|--------|---| 
| Bob | 32 | 168 | 180 |
| Alice | 24 | 150 | 175 |
| Steve | 64 | 144 | 165 |
&#10;-->

## Long

[Long](https://en.wikipedia.org/wiki/Wide_and_narrow_data) tabular data
is stacked, so that one column contains all of the values and an
additional column denotes the context of those values, e.g.:

``` r
# This is code to create a data frame (table) in R
df_long <- data.frame(Person = c('Bob', 'Bob', 'Bob', 'Alice', 'Alice', 'Alice', 'Steve', 'Steve', 'Steve'),
                      Variable = c('Age', 'Weight', 'Height', 'Age', 'Weight', 'Height', 'Age', 'Weight', 'Height'),
                      Value = c(32, 168, 180, 24, 150, 175, 64, 144, 165))

df_long %>% kable()
```

| Person | Variable | Value |
|:-------|:---------|------:|
| Bob    | Age      |    32 |
| Bob    | Weight   |   168 |
| Bob    | Height   |   180 |
| Alice  | Age      |    24 |
| Alice  | Weight   |   150 |
| Alice  | Height   |   175 |
| Steve  | Age      |    64 |
| Steve  | Weight   |   144 |
| Steve  | Height   |   165 |

Why do people use long format? It looks more difficult to work with
manually.

------------------------------------------------------------------------

One important reason is that it is a data model that encodes data in a
space-efficient manner.

For example, if you have a [sparse
matrix](https://en.wikipedia.org/wiki/Sparse_matrix) in wide format, you
may have lots of `NA` or `0` cells.

In long format, you can simply leave those out (because you can infer
them when translating from long to wide format).

This long data format basically encodes what is called an
[entity-attribute-value data
model](https://en.wikipedia.org/wiki/Entity–attribute–value_model).

Similar data models are used in all kinds of applications, such as
[knowledge graphs](https://en.wikipedia.org/wiki/Knowledge_graph), in
search engines, [graph
databases](https://en.wikipedia.org/wiki/Graph_database), and so on.

## “Tidy data” in R

The `tidyverse` works on [tidy
data](https://r4ds.had.co.nz/tidy-data.html), i.e., a consistent way to
organize data in R.

One of the goals of the `tidyverse` suite of tools is to make an
interface between data input and data output – that is, once you have
data in the tidy data format, working with the tools in the tidyverse
become much simpler.

**In other words, to play and have fun with the tools in tidyverse, you
should first get your data into the tidy format.**

As shown above, the same tabular data can be formatted in different
ways. (The picture is actually more complex because data can be
represented in many different ways in tables, e.g., [see
here](https://r4ds.had.co.nz/tidy-data.html).)

**To create tidy data, there are three rules you must follow
\[@WickhamGrolemund2016\]:**

1.  Each variable must have its own column.
2.  Each observation must have its own row.
3.  Each value must have its own cell.

<!--
So, variables in columns, observations in rows, values in cells -- in one table! 
&#10;This boils down to: put your data in a table (or data frame or tibble in R) and put each variable in a column.
-->

# Data wrangling

So how do we explore our data in R?

<!-- 
[Data wrangling](https://en.wikipedia.org/wiki/Data_wrangling) is the process(es) of cleaning, structuring (aka transforming), and enriching data into a format that allows one to do things like visualization and analysis. 
&#10;Typically, [raw data](https://en.wikipedia.org/wiki/Raw_data) is converted into structured data. During this process, raw data often need to be cleaned, organized, and transformed into formats that allow for data analysis, i.e., converted into data formats or data structures that can be fed directly into functions for code or statistical analysis.
&#10;Here's a [visualization of the process](https://en.wikipedia.org/wiki/Data_wrangling#/media/File:Data_Wrangling_From_Messy_To_Clean_Data_Management.jpg) of converting raw data to formatted (or structured) data:
&#10;![Visualization of data wrangling](figures/Data_Wrangling_From_Messy_To_Clean_Data_Management.jpg)
&#10;***
&#10;Broadly speaking, the steps in data wrangling include:
&#10;* **Explore your data** -- look at and think about your data and maybe come up with some questions to ask.
&#10;* **Structure your data** -- organize the data (necessarily if its in a raw format) and structure it for the functions or methods that will take it as input.
&#10;* **Clean your data** -- in the process of dealing with data (especially raw data), you may need to clean it, e.g., get all the dates into the same format (Feb 1 vs 2/1 vs 1/2 etc.).
&#10;* **Enrich your data** -- do you need more data for your analysis? E.g., you have a list of languages but you need to know where they are spoken to plot them on a world map.
&#10;* **Validate your data** -- basically making sure that your structured and cleaned data is actually structured and clean -- also commonly refereed to as [data validation](https://en.wikipedia.org/wiki/Data_validation) and in software development, 
[software testing](https://en.wikipedia.org/wiki/Software_testing).
&#10;* **Publish your data** -- publish your analysis, findings, etc., for consumption, reproducibility, archiving, etc.
&#10;
# Data wrangling in R
&#10;-->

Here is a visualization of how the data science workflow works in the
book [R for Data Science](https://r4ds.had.co.nz/index.html) within the
so-called [tidyverse](https://www.tidyverse.org):

- <https://r4ds.had.co.nz/introduction.html>

<figure>
<img src="figures/WickhamGrolemund.png"
alt="Typical data science project workflow" />
<figcaption aria-hidden="true">Typical data science project
workflow</figcaption>
</figure>

That is you:

1.  **Load** (aka “import”) your data into R – this entails that the
    data is already in a loadable format (e.g., text, CSV file, Excel
    spreadsheet, relational database).

2.  **Tidy** the data – in terms of the R
    [tidyverse](https://www.tidyverse.org), i.e., a set of R packages
    aimed to make data science easy (more on this below).

3.  **Transform** your data – select the data of interest, extend your
    data by adding other data source(s), summarize results.

4.  **Visualize** your data – create plots to explore and interact with
    your data and to develop questions to answer.

5.  **Model** your data – to answer questions and hypotheses.

6.  **Communicate** your results – through scientific reports like we
    the ones we are developing in class.

All of these steps fall under the rubric of [computer
programming](https://en.wikipedia.org/wiki/Computer_programming). You
may not become expert computer programmers, or software engineers, but
in this course will need to be able to do some basic coding in R.

**Keep in mind: you do not need to be an expert coder, or programmer, to
do data science!**

## tidyverse

The [tidyverse](https://www.tidyverse.org/) is an opinionated collection
of R packages designed for data science. All packages share an
underlying design philosophy, grammar, and data structures.
[Tidyverse](https://www.tidyverse.org) includes several R
[packages](https://www.tidyverse.org/packages/) for data science.

We may not use all of them in this class, but they currently include:

- [ggplot2](https://ggplot2.tidyverse.org): for creating graphics
- [dplyr](https://dplyr.tidyverse.org): for data manipulation
- [tidyr](https://tidyr.tidyverse.org): to make your data tidy
- [readr](https://readr.tidyverse.org): for reading / loading data
- [purr](https://purrr.tidyverse.org): enhancements for functional
  programming
- [tibble](https://tibble.tidyverse.org): update of the R [data
  frame](https://stat.ethz.ch/R-manual/R-devel/library/base/html/data.frame.html)
- [stringr](https://stringr.tidyverse.org): functions for working with
  strings
- [forcats](https://forcats.tidyverse.org): functions for dealing with
  [factors](https://stat.ethz.ch/R-manual/R-devel/library/base/html/factor.html)
  in R

To load the `tidyverse` package, which then includes all the libraries
above, first install it and then load:

``` r
# install.packages("tidyverse") # run this first if you need to install tidyverse
library(tidyverse)
```

## Loading data

There are numerous ways to load data into R. The most common for
text-based tables are:

| Function | Format                         | Filename typically ends with |
|----------|--------------------------------|------------------------------|
| read_csv | comma separated values         | csv                          |
| read_tsv | tab delimited separated values | tsv                          |

<!--
&#10;The `readr` is the `tidyverse` library that lets us read in plain text data in various formats:
&#10;| Function | Format | Filename typically ends with |
|----------|--------|---| 
| read_table | white space separated values | txt |
| read_csv | comma separated values|  csv |
| read_csv2 | semicolon separated values | csv |
| read_tsv | tab delimited separated values | tsv |
| read_delim | general text file format, must define delimiter | txt |
&#10;Below we will load some csv data and have a look at it.
&#10;-->

For loading Excel spreadsheets, we can use `tidyverse` with the
`[readxl](https://readxl.tidyverse.org)` package, e.g.:

| Function   | Format                 | Filename typically ends with |
|------------|------------------------|------------------------------|
| read_excel | auto detect the format | xls, xlsx                    |
| read_xls   | original format        | xls                          |
| read_xlsx  | new format             | xlsx                         |

<!-- 
&#10;There are also load functions in "base" R. It contains similar functions to `tidyverse` functions, e.g., `read.csv`, `read.table`. They load data slightly differently and into different data types.
&#10;
```r
df <- read_csv('data/athletes.csv')
```
&#10;```
## Rows: 2859 Columns: 12
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): gender, name, sport, country
## dbl  (7): age, height, weight, gold_medals, silver_medals, bronze_medals, to...
## date (1): birthdate
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
&#10;```r
class(df)
```
&#10;```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```
&#10;```r
str(df)
```
&#10;```
## spc_tbl_ [2,859 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ age          : num [1:2859] 17 27 21 21 21 21 18 23 17 21 ...
##  $ birthdate    : Date[1:2859], format: "1996-04-12" "1986-05-14" ...
##  $ gender       : chr [1:2859] "Male" "Male" "Male" "Male" ...
##  $ height       : num [1:2859] 1.72 1.85 1.78 1.68 1.86 1.75 1.7 1.78 1.63 1.62 ...
##  $ name         : chr [1:2859] "Aaron Blunck" "Aaron March" "Abzal Azhgaliyev" "Abzal Rakimgaliev" ...
##  $ weight       : num [1:2859] 68 85 68 NA 82 57 76 80 NA 56 ...
##  $ gold_medals  : num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
##  $ silver_medals: num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
##  $ bronze_medals: num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
##  $ total_medals : num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
##  $ sport        : chr [1:2859] "Freestyle Skiing" "Snowboard" "Short Track" "Figure Skating" ...
##  $ country      : chr [1:2859] "United States" "Italy" "Kazakhstan" "Kazakhstan" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   age = col_double(),
##   ..   birthdate = col_date(format = ""),
##   ..   gender = col_character(),
##   ..   height = col_double(),
##   ..   name = col_character(),
##   ..   weight = col_double(),
##   ..   gold_medals = col_double(),
##   ..   silver_medals = col_double(),
##   ..   bronze_medals = col_double(),
##   ..   total_medals = col_double(),
##   ..   sport = col_character(),
##   ..   country = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```
&#10;One reason that people choose a library like `readr::read_csv` over base R's `read.csv` is because the latter automatically converts strings to factors. If you use base R's `read.csv` and want to keep strings as factors, you have to additionally set the parameter `stringsAsFactors` to FALSE, i.e.:
&#10;
```r
df <- read.csv('data/athletes.csv', stringsAsFactors = FALSE)
class(df)
```
&#10;```
## [1] "data.frame"
```
&#10;```r
str(df)
```
&#10;```
## 'data.frame':    2859 obs. of  12 variables:
##  $ age          : int  17 27 21 21 21 21 18 23 17 21 ...
##  $ birthdate    : chr  "1996-04-12" "1986-05-14" "1992-06-30" "1992-05-25" ...
##  $ gender       : chr  "Male" "Male" "Male" "Male" ...
##  $ height       : num  1.72 1.85 1.78 1.68 1.86 1.75 1.7 1.78 1.63 1.62 ...
##  $ name         : chr  "Aaron Blunck" "Aaron March" "Abzal Azhgaliyev" "Abzal Rakimgaliev" ...
##  $ weight       : int  68 85 68 NA 82 57 76 80 NA 56 ...
##  $ gold_medals  : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ silver_medals: int  0 0 0 0 0 0 0 0 0 0 ...
##  $ bronze_medals: int  0 0 0 0 0 0 0 0 0 0 ...
##  $ total_medals : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ sport        : chr  "Freestyle Skiing" "Snowboard" "Short Track" "Figure Skating" ...
##  $ country      : chr  "United States" "Italy" "Kazakhstan" "Kazakhstan" ...
```
-->

## Accessing and mainpulating data

Once you’ve loaded a data set into a data frame (or
[tibble](https://tibble.tidyverse.org)), you can begin to work with it.

<!-- `dplyr` was developed by [Hadley Wickham](https://en.wikipedia.org/wiki/Hadley_Wickham) (the author of `plyr`, `ggplot2, etc.`). He also co-wrote a great introduction on the topic, look at [R for Data Science](https://r4ds.had.co.nz/) [@WickhamGrolemund2016], specifically the section [dplyr basics](https://r4ds.had.co.nz/transform.html?q=dplyr#dplyr-basics).-->

`dplyr` is a library in the `tidyverse` that contains five basic “verbs”
or functions for accessing table data

- `select()`: for selecting (“subsetting”) variables/columns

- `arrange()`: for re-ordering rows

- `mutate()`: for adding new columns

- `summarize()` (or `summarise()` if you prefer [British
  spelling](https://en.wikipedia.org/wiki/American_and_British_English_spelling_differences)):
  for reducing each group to a smaller number of summary statistics

- `group_by()`: group an existing table into a grouped table, where
  operations are applied to each group (usually used in conjunction with
  another function, such as `summarize()`)

- `filter()`: for filtering (“subsetting”) rows

Let’s try them out!

------------------------------------------------------------------------

## Some examples

Let’s use the data set `athletes.csv` for an example of working with
some of `tidyverse` tools.

The data set `athletes.csv` contains various details about the athletes
who participated in the [2014 Winter Olympics in
Sochi](https://en.wikipedia.org/wiki/2014_Winter_Olympics). Here we use
the version of this data set converted by [Dana
Silver](https://www.danasilver.org/sochi/). The data was originally
retrieved in [JSON format](https://en.wikipedia.org/wiki/JSON) from
Kimono Lab’s Sochi API on February 18, 2014.

<!-- When we load tabular (or table) data, we will usually be using the R data frame. A data frame is a table in which columns contain values of one data type and each row contains a set of values of that data type. The data frame is the most common way of storing data in R. ([Under the hood](https://en.wiktionary.org/wiki/under_the_hood), data frames are a list of equal length vectors -- each element of the list can be thought of as a column and the length of each element of the list is its number of rows.) Data frames have the following characteristics:
&#10;* Column names should not be empty
* Row names should be unique
* Data stored in the data frame can be of type numeric, factor, of character
* Each column should contain the same number of items
-->

If you are working within this repository on your local computer, you
can load the data in this directory with this command:

``` r
athletes <- read_csv("data/athletes.csv")
```

<!-- You can also give it the full path, depending on where the data is on your local computer. -->

Or wrap the `url` function into the `read_csv` function and load the
data from online:

``` r
athletes <- read_csv(url("https://raw.githubusercontent.com/bambooforest/salos/main/data/athletes.csv"))
```

<!--
Note that GitHub displays well-formatted CSV files as tabular data:
&#10;* https://github.com/bambooforest/IntroDataScience/blob/main/4_data_wrangling/datasets/athletes.csv
&#10;But if you want to load it from the web, you need to use the raw data (note the button on the GitHub page with the label "raw", which results in this URL:
&#10;* https://raw.githubusercontent.com/bambooforest/IntroDataScience/main/4_data_wrangling/datasets/athletes.csv
-->

Now that you’ve loaded the `athletes.csv` data, let’s have a look at its
structure with the `str()` function:

``` r
str(athletes)
```

    ## spc_tbl_ [2,859 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ age          : num [1:2859] 17 27 21 21 21 21 18 23 17 21 ...
    ##  $ birthdate    : Date[1:2859], format: "1996-04-12" "1986-05-14" ...
    ##  $ gender       : chr [1:2859] "Male" "Male" "Male" "Male" ...
    ##  $ height       : num [1:2859] 1.72 1.85 1.78 1.68 1.86 1.75 1.7 1.78 1.63 1.62 ...
    ##  $ name         : chr [1:2859] "Aaron Blunck" "Aaron March" "Abzal Azhgaliyev" "Abzal Rakimgaliev" ...
    ##  $ weight       : num [1:2859] 68 85 68 NA 82 57 76 80 NA 56 ...
    ##  $ gold_medals  : num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ silver_medals: num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ bronze_medals: num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ total_medals : num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ sport        : chr [1:2859] "Freestyle Skiing" "Snowboard" "Short Track" "Figure Skating" ...
    ##  $ country      : chr [1:2859] "United States" "Italy" "Kazakhstan" "Kazakhstan" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   age = col_double(),
    ##   ..   birthdate = col_date(format = ""),
    ##   ..   gender = col_character(),
    ##   ..   height = col_double(),
    ##   ..   name = col_character(),
    ##   ..   weight = col_double(),
    ##   ..   gold_medals = col_double(),
    ##   ..   silver_medals = col_double(),
    ##   ..   bronze_medals = col_double(),
    ##   ..   total_medals = col_double(),
    ##   ..   sport = col_character(),
    ##   ..   country = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

<!--
Recall our discussion about [data types for computer programming](https://github.com/bambooforest/IntroDataScience/tree/main/3_data#data-types-for-computer-programming) and [data types for statistics](https://github.com/bambooforest/IntroDataScience/tree/main/3_data#data-types-in-statistics).
&#10;**What kind of variables do we have in our data set?**
&#10;-->

Some variables should be converted from the data type
[character](https://stat.ethz.ch/R-manual/R-devel/library/base/html/character.html)
to
[factor](https://stat.ethz.ch/R-manual/R-devel/library/base/html/factor.html).

In R, a factor is a data type that is used to represent categorical
data, while a character data type is used to represent text data. We
discuss these below.

``` r
athletes$gender <- as.factor(athletes$gender)
athletes$sport <- as.factor(athletes$sport)
athletes$country <- as.factor(athletes$country)
```

Now note the change in data type:

``` r
str(athletes)
```

    ## spc_tbl_ [2,859 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ age          : num [1:2859] 17 27 21 21 21 21 18 23 17 21 ...
    ##  $ birthdate    : Date[1:2859], format: "1996-04-12" "1986-05-14" ...
    ##  $ gender       : Factor w/ 2 levels "Female","Male": 2 2 2 2 2 2 2 2 1 1 ...
    ##  $ height       : num [1:2859] 1.72 1.85 1.78 1.68 1.86 1.75 1.7 1.78 1.63 1.62 ...
    ##  $ name         : chr [1:2859] "Aaron Blunck" "Aaron March" "Abzal Azhgaliyev" "Abzal Rakimgaliev" ...
    ##  $ weight       : num [1:2859] 68 85 68 NA 82 57 76 80 NA 56 ...
    ##  $ gold_medals  : num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ silver_medals: num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ bronze_medals: num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ total_medals : num [1:2859] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ sport        : Factor w/ 15 levels "Alpine Skiing",..: 7 14 11 6 1 10 1 1 6 1 ...
    ##  $ country      : Factor w/ 88 levels "Albania","Andorra",..: 83 38 41 41 57 63 54 70 67 26 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   age = col_double(),
    ##   ..   birthdate = col_date(format = ""),
    ##   ..   gender = col_character(),
    ##   ..   height = col_double(),
    ##   ..   name = col_character(),
    ##   ..   weight = col_double(),
    ##   ..   gold_medals = col_double(),
    ##   ..   silver_medals = col_double(),
    ##   ..   bronze_medals = col_double(),
    ##   ..   total_medals = col_double(),
    ##   ..   sport = col_character(),
    ##   ..   country = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

<!-- 
Also, recall our discussion about [tabular data](https://github.com/bambooforest/IntroDataScience/tree/main/3_data#tabular-data).
&#10;What are the variables in `athletes`?
&#10;What are the objects of observation?
&#10;If any row is lacking information for a particular column, a missing value called `NA` will be inserted in that cell.
-->

One useful function is `summary()`, which will summarize the contents of
the data frame.

``` r
summary(athletes)
```

    ##       age          birthdate             gender         height     
    ##  Min.   :15.00   Min.   :1959-02-02   Female:1155   Min.   :1.460  
    ##  1st Qu.:22.00   1st Qu.:1984-10-01   Male  :1704   1st Qu.:1.680  
    ##  Median :25.00   Median :1988-03-13                 Median :1.760  
    ##  Mean   :25.84   Mean   :1987-10-02                 Mean   :1.752  
    ##  3rd Qu.:29.00   3rd Qu.:1991-05-14                 3rd Qu.:1.820  
    ##  Max.   :55.00   Max.   :1998-12-31                 Max.   :2.060  
    ##                                                     NA's   :139    
    ##      name               weight        gold_medals      silver_medals    
    ##  Length:2859        Min.   : 40.00   Min.   :0.00000   Min.   :0.00000  
    ##  Class :character   1st Qu.: 62.00   1st Qu.:0.00000   1st Qu.:0.00000  
    ##  Mode  :character   Median : 72.00   Median :0.00000   Median :0.00000  
    ##                     Mean   : 73.11   Mean   :0.03358   Mean   :0.03218  
    ##                     3rd Qu.: 83.00   3rd Qu.:0.00000   3rd Qu.:0.00000  
    ##                     Max.   :120.00   Max.   :3.00000   Max.   :2.00000  
    ##                     NA's   :378                                         
    ##  bronze_medals      total_medals                  sport     
    ##  Min.   :0.00000   Min.   :0.00000   Ice Hockey      : 469  
    ##  1st Qu.:0.00000   1st Qu.:0.00000   Alpine Skiing   : 328  
    ##  Median :0.00000   Median :0.00000   Cross-Country   : 310  
    ##  Mean   :0.03253   Mean   :0.09829   Freestyle Skiing: 271  
    ##  3rd Qu.:0.00000   3rd Qu.:0.00000   Snowboard       : 239  
    ##  Max.   :2.00000   Max.   :3.00000   Biathlon        : 211  
    ##                                      (Other)         :1031  
    ##           country    
    ##  United States: 229  
    ##  Russian Fed. : 226  
    ##  Canada       : 220  
    ##  Switzerland  : 163  
    ##  Germany      : 154  
    ##  Austria      : 130  
    ##  (Other)      :1737

Now some functions that we can select, filter, transform, extract, and
summarize aspects of the data.

### `select()`

<!--
The data set contains the details on age, birth date, gender, height, name, weight, medal counts, sport, and country and a few more variables. Not all of these variables are of interest for a specific research question and often one wants to have a "narrower" version of a data set which contains only the variables of immediate interest. 
&#10;To prepare such a version of your data set, use the function `select()` to select the variables (columns) you need.
-->

First, you can select the variables of interest just by naming them.
Note that this way you can also modify the order of the variables:

``` r
select(athletes, name, height, weight)
```

    ## # A tibble: 2,859 × 3
    ##    name              height weight
    ##    <chr>              <dbl>  <dbl>
    ##  1 Aaron Blunck        1.72     68
    ##  2 Aaron March         1.85     85
    ##  3 Abzal Azhgaliyev    1.78     68
    ##  4 Abzal Rakimgaliev   1.68     NA
    ##  5 Adam Barwood        1.86     82
    ##  6 Adam Cieslar        1.75     57
    ##  7 Adam Lamhamedi      1.7      76
    ##  8 Adam Zampa          1.78     80
    ##  9 Adelina Sotnikova   1.63     NA
    ## 10 Adeline Baud        1.62     56
    ## # ℹ 2,849 more rows

- You can use `:` to select all columns in a range between two specified
  columns (inclusively):

``` r
select(athletes, age:weight)
```

    ## # A tibble: 2,859 × 6
    ##      age birthdate  gender height name              weight
    ##    <dbl> <date>     <fct>   <dbl> <chr>              <dbl>
    ##  1    17 1996-04-12 Male     1.72 Aaron Blunck          68
    ##  2    27 1986-05-14 Male     1.85 Aaron March           85
    ##  3    21 1992-06-30 Male     1.78 Abzal Azhgaliyev      68
    ##  4    21 1992-05-25 Male     1.68 Abzal Rakimgaliev     NA
    ##  5    21 1992-07-30 Male     1.86 Adam Barwood          82
    ##  6    21 1992-12-18 Male     1.75 Adam Cieslar          57
    ##  7    18 1995-04-22 Male     1.7  Adam Lamhamedi        76
    ##  8    23 1990-09-13 Male     1.78 Adam Zampa            80
    ##  9    17 1996-07-01 Female   1.63 Adelina Sotnikova     NA
    ## 10    21 1992-09-28 Female   1.62 Adeline Baud          56
    ## # ℹ 2,849 more rows

- You can exclude a variable with the help of `-`.

``` r
select(athletes, -birthdate, -age)
```

    ## # A tibble: 2,859 × 10
    ##    gender height name             weight gold_medals silver_medals bronze_medals
    ##    <fct>   <dbl> <chr>             <dbl>       <dbl>         <dbl>         <dbl>
    ##  1 Male     1.72 Aaron Blunck         68           0             0             0
    ##  2 Male     1.85 Aaron March          85           0             0             0
    ##  3 Male     1.78 Abzal Azhgaliyev     68           0             0             0
    ##  4 Male     1.68 Abzal Rakimgali…     NA           0             0             0
    ##  5 Male     1.86 Adam Barwood         82           0             0             0
    ##  6 Male     1.75 Adam Cieslar         57           0             0             0
    ##  7 Male     1.7  Adam Lamhamedi       76           0             0             0
    ##  8 Male     1.78 Adam Zampa           80           0             0             0
    ##  9 Female   1.63 Adelina Sotniko…     NA           0             0             0
    ## 10 Female   1.62 Adeline Baud         56           0             0             0
    ## # ℹ 2,849 more rows
    ## # ℹ 3 more variables: total_medals <dbl>, sport <fct>, country <fct>

Note that `dplyr` functions never modify their input data frames. If you
want to save the result e.g., of the `select()`function, you need to use
the assignment operator `<-` and either overwrite the original data
frame or create a new one.

For instance, here the original data frame remains unchanged:

``` r
select(athletes, birthdate, age)
```

    ## # A tibble: 2,859 × 2
    ##    birthdate    age
    ##    <date>     <dbl>
    ##  1 1996-04-12    17
    ##  2 1986-05-14    27
    ##  3 1992-06-30    21
    ##  4 1992-05-25    21
    ##  5 1992-07-30    21
    ##  6 1992-12-18    21
    ##  7 1995-04-22    18
    ##  8 1990-09-13    23
    ##  9 1996-07-01    17
    ## 10 1992-09-28    21
    ## # ℹ 2,849 more rows

``` r
athletes
```

    ## # A tibble: 2,859 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    17 1996-04-12 Male     1.72 Aaron Blunck      68           0             0
    ##  2    27 1986-05-14 Male     1.85 Aaron March       85           0             0
    ##  3    21 1992-06-30 Male     1.78 Abzal Azhgal…     68           0             0
    ##  4    21 1992-05-25 Male     1.68 Abzal Rakimg…     NA           0             0
    ##  5    21 1992-07-30 Male     1.86 Adam Barwood      82           0             0
    ##  6    21 1992-12-18 Male     1.75 Adam Cieslar      57           0             0
    ##  7    18 1995-04-22 Male     1.7  Adam Lamhame…     76           0             0
    ##  8    23 1990-09-13 Male     1.78 Adam Zampa        80           0             0
    ##  9    17 1996-07-01 Female   1.63 Adelina Sotn…     NA           0             0
    ## 10    21 1992-09-28 Female   1.62 Adeline Baud      56           0             0
    ## # ℹ 2,849 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

And here we create a new data frame (table) with the selected columns
only:

``` r
athletes_age <- select(athletes, birthdate, age)
athletes_age
```

    ## # A tibble: 2,859 × 2
    ##    birthdate    age
    ##    <date>     <dbl>
    ##  1 1996-04-12    17
    ##  2 1986-05-14    27
    ##  3 1992-06-30    21
    ##  4 1992-05-25    21
    ##  5 1992-07-30    21
    ##  6 1992-12-18    21
    ##  7 1995-04-22    18
    ##  8 1990-09-13    23
    ##  9 1996-07-01    17
    ## 10 1992-09-28    21
    ## # ℹ 2,849 more rows

<!--
There are a number of **helper functions** you can use within `select()`. Try to guess what they do before executing the commands:
&#10;1. **`ends_with()`**
&#10;
```r
select(athletes, ends_with("medals"))
```
&#10;```
## # A tibble: 2,859 × 4
##    gold_medals silver_medals bronze_medals total_medals
##          <dbl>         <dbl>         <dbl>        <dbl>
##  1           0             0             0            0
##  2           0             0             0            0
##  3           0             0             0            0
##  4           0             0             0            0
##  5           0             0             0            0
##  6           0             0             0            0
##  7           0             0             0            0
##  8           0             0             0            0
##  9           0             0             0            0
## 10           0             0             0            0
## # ℹ 2,849 more rows
```
&#10;2. **`contains()`**
&#10;
```r
select(athletes, contains("eigh"))
```
&#10;```
## # A tibble: 2,859 × 2
##    height weight
##     <dbl>  <dbl>
##  1   1.72     68
##  2   1.85     85
##  3   1.78     68
##  4   1.68     NA
##  5   1.86     82
##  6   1.75     57
##  7   1.7      76
##  8   1.78     80
##  9   1.63     NA
## 10   1.62     56
## # ℹ 2,849 more rows
```
&#10;3. For details on other helper functions, see `?select`.
&#10;
```r
?select
```
-->

### `arrange()`

`arrange()` changes the order of the rows. It takes a data frame and a
set of column names to order by. If you list more than one column name,
each additional column is used to break ties in the values of preceding
columns.

1.  Guess what the following code will do before executing it!

``` r
arrange(athletes, height, age)
```

<!-- The data set is first order in an ascending order by `height`. In case two or more athletes have the same height, `age` is used to break ties (also in ascending order), e.g. several athletes have the height of 1.50 cm, they are then order from youngest to oldest. -->

2.  Guess what the following code will do before executing it!

``` r
arrange(athletes, desc(height), age)
```

<!-- The data set is first order in a descending order by `height`. In case two or more athletes have the same height, `age` is used to break ties (but now in ascending order), e.g. among the tallest athletes are the four athletes with the height of 2.00 cm, they are then order from youngest to oldest. -->

3.  Use only the `arrange()` to display the oldest female athlete at the
    top of the sorted data set. How old is she?

``` r
arrange(athletes, gender, desc(age))
```

<!-- Angelica Morrone is 48 years old. -->

### `mutate()`

Besides working with existing columns, it is sometimes necessary to add
new columns that are functions of existing columns. That’s what the
`mutate()` function is for.

`mutate()` always adds new columns at the end of your data set so we’ll
start by creating a narrower data set so we can see the new variables.
(Remember that when you use in RStudio, an easy way to see all the
columns is `View()`.)

First, create with `select()` a narrow data set `athletes_narrow` which
only contains the variables `name`, `gender`, `age`, `sport`, `height`,
and `weight`. (Here, we add a few more columns than we need now, because
we plan to use `athletes_narrow` later).

``` r
athletes_narrow <- select(athletes, name, gender, age, sport, height, weight)
athletes_narrow
```

    ## # A tibble: 2,859 × 6
    ##    name              gender   age sport            height weight
    ##    <chr>             <fct>  <dbl> <fct>             <dbl>  <dbl>
    ##  1 Aaron Blunck      Male      17 Freestyle Skiing   1.72     68
    ##  2 Aaron March       Male      27 Snowboard          1.85     85
    ##  3 Abzal Azhgaliyev  Male      21 Short Track        1.78     68
    ##  4 Abzal Rakimgaliev Male      21 Figure Skating     1.68     NA
    ##  5 Adam Barwood      Male      21 Alpine Skiing      1.86     82
    ##  6 Adam Cieslar      Male      21 Nordic Combined    1.75     57
    ##  7 Adam Lamhamedi    Male      18 Alpine Skiing      1.7      76
    ##  8 Adam Zampa        Male      23 Alpine Skiing      1.78     80
    ##  9 Adelina Sotnikova Female    17 Figure Skating     1.63     NA
    ## 10 Adeline Baud      Female    21 Alpine Skiing      1.62     56
    ## # ℹ 2,849 more rows

Next, add the column BMI (body mass index). The BMI is calculated as the
body mass (`weight`) divided by the square of the body `height`. It is
universally expressed in units of kg/m<sup>2</sup>.

``` r
mutate(athletes_narrow, BMI = weight/height^2)
```

    ## # A tibble: 2,859 × 7
    ##    name              gender   age sport            height weight   BMI
    ##    <chr>             <fct>  <dbl> <fct>             <dbl>  <dbl> <dbl>
    ##  1 Aaron Blunck      Male      17 Freestyle Skiing   1.72     68  23.0
    ##  2 Aaron March       Male      27 Snowboard          1.85     85  24.8
    ##  3 Abzal Azhgaliyev  Male      21 Short Track        1.78     68  21.5
    ##  4 Abzal Rakimgaliev Male      21 Figure Skating     1.68     NA  NA  
    ##  5 Adam Barwood      Male      21 Alpine Skiing      1.86     82  23.7
    ##  6 Adam Cieslar      Male      21 Nordic Combined    1.75     57  18.6
    ##  7 Adam Lamhamedi    Male      18 Alpine Skiing      1.7      76  26.3
    ##  8 Adam Zampa        Male      23 Alpine Skiing      1.78     80  25.2
    ##  9 Adelina Sotnikova Female    17 Figure Skating     1.63     NA  NA  
    ## 10 Adeline Baud      Female    21 Alpine Skiing      1.62     56  21.3
    ## # ℹ 2,849 more rows

Notice that `mutate` does not overwrite the existing data frame.

``` r
mutate(athletes_narrow, BMI = weight/height^2)
```

    ## # A tibble: 2,859 × 7
    ##    name              gender   age sport            height weight   BMI
    ##    <chr>             <fct>  <dbl> <fct>             <dbl>  <dbl> <dbl>
    ##  1 Aaron Blunck      Male      17 Freestyle Skiing   1.72     68  23.0
    ##  2 Aaron March       Male      27 Snowboard          1.85     85  24.8
    ##  3 Abzal Azhgaliyev  Male      21 Short Track        1.78     68  21.5
    ##  4 Abzal Rakimgaliev Male      21 Figure Skating     1.68     NA  NA  
    ##  5 Adam Barwood      Male      21 Alpine Skiing      1.86     82  23.7
    ##  6 Adam Cieslar      Male      21 Nordic Combined    1.75     57  18.6
    ##  7 Adam Lamhamedi    Male      18 Alpine Skiing      1.7      76  26.3
    ##  8 Adam Zampa        Male      23 Alpine Skiing      1.78     80  25.2
    ##  9 Adelina Sotnikova Female    17 Figure Skating     1.63     NA  NA  
    ## 10 Adeline Baud      Female    21 Alpine Skiing      1.62     56  21.3
    ## # ℹ 2,849 more rows

``` r
athletes_narrow
```

    ## # A tibble: 2,859 × 6
    ##    name              gender   age sport            height weight
    ##    <chr>             <fct>  <dbl> <fct>             <dbl>  <dbl>
    ##  1 Aaron Blunck      Male      17 Freestyle Skiing   1.72     68
    ##  2 Aaron March       Male      27 Snowboard          1.85     85
    ##  3 Abzal Azhgaliyev  Male      21 Short Track        1.78     68
    ##  4 Abzal Rakimgaliev Male      21 Figure Skating     1.68     NA
    ##  5 Adam Barwood      Male      21 Alpine Skiing      1.86     82
    ##  6 Adam Cieslar      Male      21 Nordic Combined    1.75     57
    ##  7 Adam Lamhamedi    Male      18 Alpine Skiing      1.7      76
    ##  8 Adam Zampa        Male      23 Alpine Skiing      1.78     80
    ##  9 Adelina Sotnikova Female    17 Figure Skating     1.63     NA
    ## 10 Adeline Baud      Female    21 Alpine Skiing      1.62     56
    ## # ℹ 2,849 more rows

To add the new column to it permanently, you have to overwrite the
original data frame:

``` r
athletes_narrow <- mutate(athletes_narrow, BMI = weight/height^2)
athletes_narrow
```

    ## # A tibble: 2,859 × 7
    ##    name              gender   age sport            height weight   BMI
    ##    <chr>             <fct>  <dbl> <fct>             <dbl>  <dbl> <dbl>
    ##  1 Aaron Blunck      Male      17 Freestyle Skiing   1.72     68  23.0
    ##  2 Aaron March       Male      27 Snowboard          1.85     85  24.8
    ##  3 Abzal Azhgaliyev  Male      21 Short Track        1.78     68  21.5
    ##  4 Abzal Rakimgaliev Male      21 Figure Skating     1.68     NA  NA  
    ##  5 Adam Barwood      Male      21 Alpine Skiing      1.86     82  23.7
    ##  6 Adam Cieslar      Male      21 Nordic Combined    1.75     57  18.6
    ##  7 Adam Lamhamedi    Male      18 Alpine Skiing      1.7      76  26.3
    ##  8 Adam Zampa        Male      23 Alpine Skiing      1.78     80  25.2
    ##  9 Adelina Sotnikova Female    17 Figure Skating     1.63     NA  NA  
    ## 10 Adeline Baud      Female    21 Alpine Skiing      1.62     56  21.3
    ## # ℹ 2,849 more rows

There are many functions for creating new variables that you can use
with `mutate()`. Think of a function and look up
[here](https://r4ds.had.co.nz/transform.html?q=dplyr#mutate-funs)
\[@WickhamGrolemund2016\] for possible solutions.

### `summarize()`

The dplyr function `summarize()` (or `summarise()`) summarizes multiple
values in a single value.

Let’s create a data frame as an [reproducible
example](http://adv-r.had.co.nz/Reproducibility.html). What is a
reproducible example:

- <https://stackoverflow.com/questions/5963269/how-to-make-a-great-r-reproducible-example>
- <https://stackoverflow.com/help/minimal-reproducible-example>
- <http://adv-r.had.co.nz/Reproducibility.html>
- <https://xiangxing98.github.io/R_Learning/R_Reproducible.nb.html>

``` r
df <- data.frame(
    color = c('blue', 'black', 'blue', 'blue', 'black'),
    value = c(1, 2, 3, 4, 5)
)

df
```

    ##   color value
    ## 1  blue     1
    ## 2 black     2
    ## 3  blue     3
    ## 4  blue     4
    ## 5 black     5

So, let’s sum up the value column with `summarize()`.

``` r
summarize(df, total = sum(value))
```

    ##   total
    ## 1    15

We can do the same thing with the `athletes` data frame, for example,
for gold medals.

``` r
athletes %>% summarize(totals = sum(gold_medals))
```

### `group_by()`

Groups table data into groups on which some other operation may operate.
(`ungroup()` removes grouping.) This function is usually used along with
some other function.

For example, an important function of `summarize()` is in coordination
with the `group_by()` function. The dplyr `group_by` function take an
existing data frame and performs an operation by group. For example,
let’s group our data frame `df` by the color column.

``` r
df %>% group_by(color) %>% summarize(total = sum(value))
```

    ## # A tibble: 2 × 2
    ##   color total
    ##   <chr> <dbl>
    ## 1 black     7
    ## 2 blue      8

You can also save the results to a new data frame.

``` r
by_color <- df %>% group_by(color) %>% summarize(total = sum(value))
by_color
```

    ## # A tibble: 2 × 2
    ##   color total
    ##   <chr> <dbl>
    ## 1 black     7
    ## 2 blue      8

We can also use `group_by` on the athletes data. For example, how many
gold medals per country?

``` r
athletes %>% group_by(country) %>% summarize(gold_medals = sum(gold_medals))
```

    ## # A tibble: 88 × 2
    ##    country    gold_medals
    ##    <fct>            <dbl>
    ##  1 Albania              0
    ##  2 Andorra              0
    ##  3 Argentina            0
    ##  4 Armenia              0
    ##  5 Australia            0
    ##  6 Austria              2
    ##  7 Azerbaijan           0
    ##  8 Belarus              5
    ##  9 Belgium              0
    ## 10 Bermuda              0
    ## # ℹ 78 more rows

Maybe for viewing purposes it’s better to arrange them by number of gold
medals instead of alphabetically by country name.

``` r
athletes %>% group_by(country) %>% summarize(gold_medals = sum(gold_medals)) %>% arrange(desc(gold_medals))
```

    ## # A tibble: 88 × 2
    ##    country       gold_medals
    ##    <fct>               <dbl>
    ##  1 Russian Fed.           16
    ##  2 Germany                15
    ##  3 Sweden                  8
    ##  4 Norway                  7
    ##  5 Korea                   6
    ##  6 Netherlands             6
    ##  7 United States           6
    ##  8 Belarus                 5
    ##  9 Switzerland             5
    ## 10 Canada                  4
    ## # ℹ 78 more rows

As we continue to [pipe](https://r4ds.had.co.nz/pipes.html) commands
together, i.e., putting multiple operations (aka dplyr verbs) together –
we can use [white
space](https://en.wikipedia.org/wiki/Whitespace_character) to style the
code. More on style below.

``` r
athletes %>% 
  group_by(country) %>% 
  summarize(gold_medals = sum(gold_medals)) %>% 
  arrange(desc(gold_medals))
```

    ## # A tibble: 88 × 2
    ##    country       gold_medals
    ##    <fct>               <dbl>
    ##  1 Russian Fed.           16
    ##  2 Germany                15
    ##  3 Sweden                  8
    ##  4 Norway                  7
    ##  5 Korea                   6
    ##  6 Netherlands             6
    ##  7 United States           6
    ##  8 Belarus                 5
    ##  9 Switzerland             5
    ## 10 Canada                  4
    ## # ℹ 78 more rows

### `filter()`

Probably the most useful `dplyr` function is `filter()`. It allows you
to subset observations based on their values. Or in other words, you can
extract some rows from your data set on the basis of certain conditions.
The first argument of this function is the name of the data frame. The
second and subsequent arguments are the expressions that filter the data
frame.

For example, we can select all athletes who are taller than 2.00 m. Here
we continue to work with `athletes_narrow`:

``` r
filter(athletes_narrow, height > 2)
```

    ## # A tibble: 2 × 7
    ##   name               gender   age sport      height weight   BMI
    ##   <chr>              <fct>  <dbl> <fct>       <dbl>  <dbl> <dbl>
    ## 1 Aleksandar Bundalo Male      24 Bobsleigh    2.02    106  26.0
    ## 2 Zdeno Chara        Male      36 Ice Hockey   2.06    117  27.6

To use `filter()` effectively, you have to know how to select the
observations that you want using the comparison operators in R. The
standard comparison operators are `>`, `>=`, `<`, `<=`, `!=` (not
equal), and `==` (equal).

The operator `==` can be used with numeric variables to filter a
specific values, e.g., all athletes who are 15 years old:

``` r
filter(athletes_narrow, age == 15)
```

    ## # A tibble: 9 × 7
    ##   name              gender   age sport            height weight   BMI
    ##   <chr>             <fct>  <dbl> <fct>             <dbl>  <dbl> <dbl>
    ## 1 Alina Muller      Female    15 Ice Hockey         1.62     50  19.1
    ## 2 Anna Seidel       Female    15 Short Track       NA        43  NA  
    ## 3 Ayumu Hirano      Male      15 Snowboard          1.6      50  19.5
    ## 4 Elizaveta Ukolova Female    15 Figure Skating     1.71     NA  NA  
    ## 5 Gianina Ernst     Female    15 Ski Jumping        1.53     44  18.8
    ## 6 Marco Ladner      Male      15 Freestyle Skiing   1.75     65  21.2
    ## 7 Perrine Laffont   Female    15 Freestyle Skiing   1.62     50  19.1
    ## 8 Polina Edmunds    Female    15 Figure Skating     1.65     NA  NA  
    ## 9 Yulia Lipnitskaya Female    15 Figure Skating     1.58     NA  NA

When you’re starting out with R, the easiest mistake to make is to use =
instead of == when testing for equality. When this happens you’ll get an
informative error. Try it out:

``` r
filter(athletes_narrow, age = 15)
```

## Filtering NA

Another important function when filtering is to identify and potentially
filter out `NA` cells. These are missing or unknown values in the data
set.

<!-- R [provides a function](https://stat.ethz.ch/R-manual/R-devel/library/base/html/NA.html) called `is.na()` that tests whether a value is missing or not. `NA` is a special value in R.
&#10;
```r
x <- NA # special value NA
y <- 1 # integer
z <- 'NA' # character (string)
is.na(x)
```
&#10;```
## [1] TRUE
```
&#10;```r
is.na(y)
```
&#10;```
## [1] FALSE
```
&#10;```r
is.na(z)
```
&#10;```
## [1] FALSE
```
-->

Let’s filter the rows in `athletes` that do not have a height value,
i.e., they are `NA` in the table.

``` r
athletes %>% filter(is.na(height))
```

    ## # A tibble: 139 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    25 1988-05-12 Female     NA Aja Evans         NA           0             0
    ##  2    21 1992-09-21 Male       NA Aleksander A…     NA           0             0
    ##  3    24 1989-05-28 Male       NA Alexey Negod…     NA           0             0
    ##  4    20 1993-05-08 Female     NA Anastasiya N…     NA           0             0
    ##  5    22 1991-05-13 Male       NA Anders Fanne…     NA           0             0
    ##  6    27 1986-05-22 Male       NA Anders Gloee…     NA           0             0
    ##  7    28 1985-10-24 Female     NA Angeli Vanla…     NA           0             0
    ##  8    31 1982-11-06 Female     NA Ann Kristin …     NA           0             0
    ##  9    15 1998-03-31 Female     NA Anna Seidel       43           0             0
    ## 10    23 1991-02-05 Female     NA Anna Sloan        NA           0             0
    ## # ℹ 129 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

You can also filter to **remove** `NA`s, which is often useful for when
you want to visualize the data. Use the logical operation `!` mentioned
above, i.e., “not”.

``` r
athletes %>% filter(!is.na(height))
```

    ## # A tibble: 2,720 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    17 1996-04-12 Male     1.72 Aaron Blunck      68           0             0
    ##  2    27 1986-05-14 Male     1.85 Aaron March       85           0             0
    ##  3    21 1992-06-30 Male     1.78 Abzal Azhgal…     68           0             0
    ##  4    21 1992-05-25 Male     1.68 Abzal Rakimg…     NA           0             0
    ##  5    21 1992-07-30 Male     1.86 Adam Barwood      82           0             0
    ##  6    21 1992-12-18 Male     1.75 Adam Cieslar      57           0             0
    ##  7    18 1995-04-22 Male     1.7  Adam Lamhame…     76           0             0
    ##  8    23 1990-09-13 Male     1.78 Adam Zampa        80           0             0
    ##  9    17 1996-07-01 Female   1.63 Adelina Sotn…     NA           0             0
    ## 10    21 1992-09-28 Female   1.62 Adeline Baud      56           0             0
    ## # ℹ 2,710 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

You can also combined the filters. For example, if you want all rows in
`athletes` that do not have `NA` values for `height` and `weight`.
Notice how the number of rows decreases.

``` r
athletes %>% filter(!is.na(height)) %>% filter(!is.na(weight))
```

    ## # A tibble: 2,479 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    17 1996-04-12 Male     1.72 Aaron Blunck      68           0             0
    ##  2    27 1986-05-14 Male     1.85 Aaron March       85           0             0
    ##  3    21 1992-06-30 Male     1.78 Abzal Azhgal…     68           0             0
    ##  4    21 1992-07-30 Male     1.86 Adam Barwood      82           0             0
    ##  5    21 1992-12-18 Male     1.75 Adam Cieslar      57           0             0
    ##  6    18 1995-04-22 Male     1.7  Adam Lamhame…     76           0             0
    ##  7    23 1990-09-13 Male     1.78 Adam Zampa        80           0             0
    ##  8    21 1992-09-28 Female   1.62 Adeline Baud      56           0             0
    ##  9    21 1992-11-22 Male     1.86 Adrian Krain…     75           0             0
    ## 10    21 1992-08-07 Male     1.78 Adrien Backs…     73           0             0
    ## # ℹ 2,469 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

If you want to check if there are any `NA`s in a column, you can also
use the `any()` function.

``` r
any(is.na(athletes$height))
```

    ## [1] TRUE

``` r
any(is.na(athletes$age))
```

    ## [1] FALSE

------------------------------------------------------------------------

## The table function

Another useful function is called `table()`. What does it do?

``` r
table(athletes$sport)
```

    ## 
    ##    Alpine Skiing         Biathlon        Bobsleigh    Cross-Country 
    ##              328              211              176              310 
    ##          Curling   Figure Skating Freestyle Skiing       Ice Hockey 
    ##              100              149              271              469 
    ##             Luge  Nordic Combined      Short Track         Skeleton 
    ##              110               55              115               47 
    ##      Ski Jumping        Snowboard    Speed Skating 
    ##              100              239              179

Note you need to use the `exclude = FALSE` parameter, if you want the
`table()` function to count `NA`s. How many athlete

``` r
table(athletes$height)
```

    ## 
    ## 1.46 1.48 1.49  1.5 1.51 1.52 1.53 1.54 1.55 1.56 1.57 1.58 1.59  1.6 1.61 1.62 
    ##    2    2    1    5    1    7    9    6   11    9   26   25   11   61   34   51 
    ## 1.63 1.64 1.65 1.66 1.67 1.68 1.69  1.7 1.71 1.72 1.73 1.74 1.75 1.76 1.77 1.78 
    ##   75   51   89   49   60  104   60  147   60   96  103   80  122   64   77  128 
    ## 1.79  1.8 1.81 1.82 1.83 1.84 1.85 1.86 1.87 1.88 1.89  1.9 1.91 1.92 1.93 1.94 
    ##   58  184   86   93  144   73  107   57   52   69   30   32   26   24   27   10 
    ## 1.95 1.96 1.97 1.98 1.99    2 2.02 2.06 
    ##    4    6    1    4    1    4    1    1

``` r
table(athletes$height, exclude = FALSE)
```

    ## 
    ## 1.46 1.48 1.49  1.5 1.51 1.52 1.53 1.54 1.55 1.56 1.57 1.58 1.59  1.6 1.61 1.62 
    ##    2    2    1    5    1    7    9    6   11    9   26   25   11   61   34   51 
    ## 1.63 1.64 1.65 1.66 1.67 1.68 1.69  1.7 1.71 1.72 1.73 1.74 1.75 1.76 1.77 1.78 
    ##   75   51   89   49   60  104   60  147   60   96  103   80  122   64   77  128 
    ## 1.79  1.8 1.81 1.82 1.83 1.84 1.85 1.86 1.87 1.88 1.89  1.9 1.91 1.92 1.93 1.94 
    ##   58  184   86   93  144   73  107   57   52   69   30   32   26   24   27   10 
    ## 1.95 1.96 1.97 1.98 1.99    2 2.02 2.06 <NA> 
    ##    4    6    1    4    1    4    1    1  139

------------------------------------------------------------------------

## Some practice problems

Use the data frame `athletes_narrow`.

1.  Filter all the athletes who are heavier than 110 kg.

2.  Filter the athletes who weigh exactly 110 or more kg. How many
    athletes are these? To get the answer, use the function `nrow()` for
    **n**umber of **row**s’ in addition to `filter()`
    (i.e. `nrows(filter(...))`). We recommend though looking first at
    the filter output to be sure that it worked the way you intended
    before applying `nrow()`.

3.  Filter all the athletes who weigh between 105 and 110 kg. How many
    are they?

# Data types

Tabular data typically contains numerical data or [categorical
data](https://en.wikipedia.org/wiki/Categorical_variable) (see data
types discussed below). Numerical data is either:

- Numerical (aka discrete) – integer values, e.g., counts, indices.
- Continuous – data that can take any value within in interval, e.g.,
  temperature.

[Categorical data](https://en.wikipedia.org/wiki/Categorical_variable)
represents sets of values that represent possible categories. They are
not subject to the laws of arithmetic (but note they can be counted.
Categorical data includes:

- Binary – dichotomous data, i.e., True vs False (often encoded 1
  vs. 0).
- Ordinal – categorical data with explicit ordering, e.g., grades,
  ranks, 5-star reviews.

The types of data in your table, i.e., the [statistical data
types](https://en.wikipedia.org/wiki/Statistical_data_type) **constrain
or determine the types of statistics you can do with your data!** See
data types discussion below.

<!--
# Data types
&#10;[Data types](https://en.wikipedia.org/wiki/Data_type) for computer programming typically refer to various *types* of data that can be interpreted by the computer's [compiler](https://en.wikipedia.org/wiki/Compiler). In programming languages, these include types such as:
&#10;* [Integers](https://en.wikipedia.org/wiki/Integer_(computer_science))
* [Floats (floating point numbers)](https://en.wikipedia.org/wiki/Floating-point_arithmetic)
* [Characters](https://en.wikipedia.org/wiki/Character_(computing)) -- ask me about these
* [Strings](https://en.wikipedia.org/wiki/String_(computer_science)) -- sequence of characters
* [Boolean](https://en.wikipedia.org/wiki/Boolean_data_type) -- two possible values (True vs False)
* [Enumerated types](https://en.wikipedia.org/wiki/Enumerated_type) -- categorical data, i.e., a set of values (e.g., factors in R)
&#10;In a [typed programming language](https://en.wikipedia.org/wiki/Type_system), the data type provides information for the compiler to check the [correctness of the computer program](https://en.wikipedia.org/wiki/Correctness_(computer_science)).
-->

# Data types in programming vs statistics

[Data types for computer
programming](https://en.wikipedia.org/wiki/Data_type) have comparable
types of [data types in
statistics](https://en.wikipedia.org/wiki/Statistical_data_type) – or
how else would we use computer programs to do statistics? For example:

| Programming | Statistics |
|----|----|
| [Integer](https://en.wikipedia.org/wiki/Integer_(computer_science)) | [Count data](https://en.wikipedia.org/wiki/Count_data) |
| [Boolean](https://en.wikipedia.org/wiki/Boolean_data_type) | [Binary data](https://en.wikipedia.org/wiki/Binary_data) |
| [Floating-point](https://en.wikipedia.org/wiki/Floating-point_arithmetic) | [Interval scale](https://en.wikipedia.org/wiki/Level_of_measurement#Interval_scale), [Ratio scale](https://en.wikipedia.org/wiki/Positive_real_numbers#Ratio_scale) |
| [Enumerated type](https://en.wikipedia.org/wiki/Enumerated_type) | [Categorical variable](https://en.wikipedia.org/wiki/Categorical_variable) |
| [List](https://en.wikipedia.org/wiki/List_(abstract_data_type)), [Array](https://en.wikipedia.org/wiki/Array_data_type) | [Random vector](https://en.wikipedia.org/wiki/Multivariate_random_variable) |

Why is important to keep this in mind? How you can programmatically
access, perform functions or statistics on these data types – whether in
programming code or with statistical methods – depends on what **type**
of data you are dealing with. For example:

- You cannot apply arithmetic to qualitative / categorical values.
- Ordinal scales are equidistant, so you can rank them, but how much x
  is from y is unknown.

One last distinction to make clear is between the term “variable” in
computer programming and in statistical analysis.

A [variable in computer
programming](https://en.wikipedia.org/wiki/Variable_(computer_science))
is used to store information that can be referenced and manipulated by a
computer program. One assigns a value to a variable, e.g., in R:

``` r
my_name <- 'Steven'
my_name
```

    ## [1] "Steven"

Variables can be manipulated, e.g.:

``` r
my_full_name <- paste0(my_name, ' Moran')
my_full_name
```

    ## [1] "Steven Moran"

Another example:

``` r
x <- 1
y <- 1
x + y
```

    ## [1] 2

# Data types in statistics

## Qualitative versus quantitative variables

In contrast to variables in computer programming, variables in
statistics are **properties** or **characteristics** used to measure a
population of individuals. A variable is thus a quantity whose value can
change across a population.

They include:

- **Qualitative variables** – measure non-numeric qualities and are not
  subject to the laws of arithmetic.
- **Quantitative variables** – measure numeric quantities and arithmetic
  can be applied to them.

Qualitative variables are also call **categorical** or **discrete**
variables. Quantitative variables can be measured, so that their rank or
score can tell you about the degree or amount of variable.

A hierarchy of variable types in statistics is given in the image below
taken from this [Stats and R](https://statsandr.com/terms/) blog.

<figure>
<img src="figures/variable-types-and-examples.png"
alt="Variable types." />
<figcaption aria-hidden="true">Variable types.</figcaption>
</figure>

As you can see, variables in statistics can be classified into four
types under qualitative and quantitative variables:

- **Nominal** – a qualitative variable where no ordering is possible
  (e.g., eye color) as implied by its **levels** – levels can be for
  example binary (e.g., do you smoke?) or multilevel (e.g., what is your
  degree – where each degree is a level).
- **Ordinal** – a qualitative variable in which an order is implied in
  the levels, e.g., if the side effects of a drug taken are measured as
  “light”, “moderate”, “severe”, then this qualitative ordinal value has
  a clear order or ranking (but note we don’t know *how* different these
  levels are from one to the next!).
- **Discrete** – variables that can only take specific values (e.g.,
  whole numbers) – no values can exist between these numbers.
- **Continuous** – variables can take the full range of values (e.g.,
  floating point numbers) – there are an infinite number of potential
  values between values.

Consider these examples:

- What number were you wearing in the race? – “5”!
- What place did you finish in? – “5”!
- How many minutes did it take you to finish? – “5”!

The three “5”s all look the same.

However, the three variables (identification number, finish place, and
time) are quite different. Because of the differences, each “5” has a
different possible interpretation.

What kind of of variables are each of these below?[^1]

``` r
df <- readr::read_csv('data/variables_quiz.csv', na=character())
knitr::kable(df)
```

| Variable | qualitative | quantitative…3 | quantitative…4 |
|:---|:---|:---|:---|
|  | discrete | discrete | continuous |
| a\. shoe size (as bought in a store) |  |  |  |
| b\. price of a salad in a salad bar |  |  |  |
| c. vote for a political party |  |  |  |
| d. travelling time to a holiday destination |  |  |  |
| e\. color of eyes |  |  |  |
| f. gender |  |  |  |
| g\. reaction time in a lexical recognition task |  |  |  |
| h\. customers’ satisfaction on a scale from 1 to 10 |  |  |  |
| i\. number of words in a written sentence |  |  |  |
| j\. spoken utterance length |  |  |  |
| k\. number of goals per player in a football event |  |  |  |
| l\. body height of a person |  |  |  |
| m\. number of vowels in a language |  |  |  |

Answers are available [here](data/variables_quiz_answers.csv).

# Data types in R

R has **five** basic [data
types](https://en.wikipedia.org/wiki/Data_type):

- Integer – whole numbers
- Numeric – numbers with decimal points (aka doubles or floats)
- Character – aka ‘strings’ (or “strings”; must have quotes around them)
- Logical – binary value, either TRUE or FALSE
- Complex – imaginary number (don’t worry about these for this class)

Each can be assigned to a
[variable](https://en.wikipedia.org/wiki/Variable_(computer_science)).
In other words, variables store data of different data types. Different
data types can do different things.

Generally, R does not explicitly state what data type is being assigned
but infers it from the thing (on the right) being assigned to the
variable (on the left).

``` r
i <- 1
n <- 0.1
c <- 'string'
l <- TRUE
```

The variables can then be used or manipulated with different
[operators](https://en.wikipedia.org/wiki/Operator_(computer_programming))
or
[functions](https://en.wikipedia.org/wiki/Function_(computer_programming)).

``` r
i + i
```

    ## [1] 2

``` r
i + n
```

    ## [1] 1.1

<!--
But note that not all operators or functions work with all data types.
&#10;
```r
# We comment this line of code out because otherwise knitting this document fails.
# Below is the erorr you will get if you try to add two strings with `+`.
&#10;# c + c 
# Error in c + c : non-numeric argument to binary operator
```
&#10;If you want to [concatenate](https://en.wikipedia.org/wiki/Concatenation) two strings (two character data types) in R, there is a function called `paste()` to do so.
&#10;
```r
paste(c, c)
```
&#10;```
## [1] "string string"
```
&#10;```r
paste(c, "+", c)
```
&#10;```
## [1] "string + string"
```
&#10;```r
paste("Here is a", c, "and another", c, "and another", c, "...")
```
&#10;```
## [1] "Here is a string and another string and another string ..."
```
&#10;When you have a variable and want to check what its data type is, use the `class()` function.
&#10;
```r
class(i)
```
&#10;```
## [1] "numeric"
```
&#10;```r
class(n)
```
&#10;```
## [1] "numeric"
```
&#10;```r
class(c)
```
&#10;```
## [1] "character"
```
&#10;```r
class(l)
```
&#10;```
## [1] "logical"
```
-->

An important set of operators when working with data types in R involve
comparison.

- is equal to: `==`
- is not equal to: `!=` (exclamation point or “bang” typically means NOT
  in programming languages)
- less than: `<`
- greater than: `>`
- less than or equal to: `<=`
- greater than or equal to: `>=`

Some examples.

``` r
i == i
```

    ## [1] TRUE

``` r
i < n
```

    ## [1] FALSE

``` r
l != l
```

    ## [1] FALSE

``` r
c != c
```

    ## [1] FALSE

``` r
# or without variables
1 == 1
```

    ## [1] TRUE

``` r
"cat" == "cats"
```

    ## [1] FALSE

``` r
"cat" != "cat"
```

    ## [1] FALSE

# Data structures in R

R has six basic [data
structures](https://en.wikipedia.org/wiki/Data_structure):

- Vector: an ordered collection of the same basic data types, i.e.,
  1-dimension (1D) of homogeneous data types
- List: a generic object that contain objects of different types, i.e.,
  1D, heterogeneous
- Matrix: a vector with attributes (called dimensions) of the same data
  types, i.e., 2D homogeneous
- Array: multidimensional matrices of the same data types, i.e., no
  dimensionality, homogeneous
- Data frame: an object consisting of columns and rows (2D,
  heterogeneous)

<!--
  Homogeneous   Heterogeneous
1d  Atomic vector   List
2d  Matrix  Data frame
nd  Array
&#10;add tibbles
-->

Here’s a nice overview:

- <https://intro2r.com/data-structures.html>

When you have a sequence of the **same** data type, R calls these
vectors.

``` r
# A vector of strings
fruits <- c("banana", "apple", "orange")

# A vector of numbers
numbers <- c(1, 2, 1)

# A vector of logical values
me_likes <- c(TRUE, FALSE, TRUE)
```

You can print these to see their contents.

``` r
fruits
```

    ## [1] "banana" "apple"  "orange"

``` r
numbers
```

    ## [1] 1 2 1

``` r
me_likes
```

    ## [1]  TRUE FALSE  TRUE

Certain functions also work on vectors.

``` r
length(fruits)
```

    ## [1] 3

``` r
sort(fruits)
```

    ## [1] "apple"  "banana" "orange"

``` r
unique(numbers)
```

    ## [1] 1 2

``` r
summary(numbers)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   1.000   1.000   1.000   1.333   1.500   2.000

Columns in your table data – your data frame – will consist of vectors
of the same length, which may have different data types **across
columns**. Here we create a data frame (tabular data object) from our
vectors above.

``` r
df <- data.frame (
  fruits,
  numbers,
  me_likes
)

df
```

    ##   fruits numbers me_likes
    ## 1 banana       1     TRUE
    ## 2  apple       2    FALSE
    ## 3 orange       1     TRUE

Of course you can also specify the vectors within the `data.frame()`
function.

``` r
df2 <- data.frame (
  countries = c("Japan", "Germany", "UK"),
  GDP_trillions = c(4.8, 3.7, 2.6),
  population_millions = c(127, 82, 67)
)

df2
```

    ##   countries GDP_trillions population_millions
    ## 1     Japan           4.8                 127
    ## 2   Germany           3.7                  82
    ## 3        UK           2.6                  67

Like functions on vectors, certain function can be used on data frames.

``` r
summary(df2)
```

    ##   countries         GDP_trillions  population_millions
    ##  Length:3           Min.   :2.60   Min.   : 67.0      
    ##  Class :character   1st Qu.:3.15   1st Qu.: 74.5      
    ##  Mode  :character   Median :3.70   Median : 82.0      
    ##                     Mean   :3.70   Mean   : 92.0      
    ##                     3rd Qu.:4.25   3rd Qu.:104.5      
    ##                     Max.   :4.80   Max.   :127.0

Lastly, factors in R are used to categorize data.

``` r
beer_types <- factor(c("IPA", "Stout", "Pilsner", "Pilsner", "Porter", "IPA", "Stout", "IPA"))
beer_types
```

    ## [1] IPA     Stout   Pilsner Pilsner Porter  IPA     Stout   IPA    
    ## Levels: IPA Pilsner Porter Stout

Factors become important when we work with categorical variables for
things like statistical testing.

# Cheat sheets

- [RMarkdown
  cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf)

[^1]: Note we set the `na` parameter of `readr::read_csv` to
    `na=character()` so that the blank cells in the CSV file are not
    interpreted as [NA](https://en.wikipedia.org/wiki/N/A), i.e., “not
    applicable”. Note also the “\_1” in the output – ask me about it!)
