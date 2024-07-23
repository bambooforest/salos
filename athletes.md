Examples from dplyr with the athletes dataset
================
Steven Moran and Alena Witzlack-Makarevich
2024-07-23

# Introduction

Let’s use the data set `athletes.csv` for an example of working with
some of `tidyverse` tools.

First load the R libraries.

``` r
library(tidyverse)
```

# Loading data and having a look

``` r
athletes <- read_csv("data/athletes.csv")
```

Have a look.

``` r
head(athletes)
```

    ## # A tibble: 6 × 12
    ##     age birthdate  gender height name           weight gold_medals silver_medals
    ##   <dbl> <date>     <chr>   <dbl> <chr>           <dbl>       <dbl>         <dbl>
    ## 1    17 1996-04-12 Male     1.72 Aaron Blunck       68           0             0
    ## 2    27 1986-05-14 Male     1.85 Aaron March        85           0             0
    ## 3    21 1992-06-30 Male     1.78 Abzal Azhgali…     68           0             0
    ## 4    21 1992-05-25 Male     1.68 Abzal Rakimga…     NA           0             0
    ## 5    21 1992-07-30 Male     1.86 Adam Barwood       82           0             0
    ## 6    21 1992-12-18 Male     1.75 Adam Cieslar       57           0             0
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <chr>,
    ## #   country <chr>

``` r
tail(athletes)
```

    ## # A tibble: 6 × 12
    ##     age birthdate  gender height name           weight gold_medals silver_medals
    ##   <dbl> <date>     <chr>   <dbl> <chr>           <dbl>       <dbl>         <dbl>
    ## 1    31 1982-12-05 Female   1.7  Zina Kocher        60           0             0
    ## 2    28 1985-06-14 Female   1.68 Zoe Gillings       65           0             0
    ## 3    27 1986-07-31 Male     1.71 Zoltan Kelemen     NA           0             0
    ## 4    22 1991-03-01 Male     1.76 Zongyang Jia       68           0             0
    ## 5    19 1995-02-06 Female   1.58 Zsofia Konya       54           0             0
    ## 6    23 1990-05-21 Female  NA    Zuzana Stromk…     NA           0             0
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <chr>,
    ## #   country <chr>

Have a look at the structure of the data with `str()`.

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

Character variables can converted into “factor” in R, i.e., categorical
data.

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

Another useful function is `summary()`:

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

# Tidyverse functions for data wrangling

Now some functions so that we can select, filter, transform, extract,
and summarize aspects of the data.

## `select()`

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

These fuctions do not modify the data.

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

But you can save the results into a new data frame.

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

## `arrange()`

`arrange()` changes the order of the rows.

``` r
arrange(athletes, height, age)
```

    ## # A tibble: 2,859 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    21 1992-07-02 Male     1.46 Liam Firus        NA           0             0
    ##  2    22 1992-01-15 Female   1.46 Narumi Takah…     NA           0             0
    ##  3    28 1985-12-08 Female   1.48 Meagan Duham…     NA           0             1
    ##  4    34 1980-01-29 Female   1.48 Michiko Toma…     NA           0             0
    ##  5    21 1992-07-01 Female   1.49 Kirsten Moor…     NA           0             1
    ##  6    18 1995-07-06 Female   1.5  Brooklee Han      NA           0             0
    ##  7    22 1991-05-05 Female   1.5  Stephanie Ma…     50           0             0
    ##  8    23 1990-06-30 Female   1.5  Anais Carade…     53           0             0
    ##  9    24 1990-02-07 Female   1.5  Danielle Obr…     NA           0             0
    ## 10    30 1983-10-14 Female   1.5  Jessica Smith     53           0             0
    ## # ℹ 2,849 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

``` r
arrange(athletes, desc(height), age)
```

    ## # A tibble: 2,859 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    36 1977-03-18 Male     2.06 Zdeno Chara      117           0             0
    ##  2    24 1989-09-28 Male     2.02 Aleksandar B…    106           0             0
    ##  3    21 1992-05-04 Male     2    Ramon Zenhae…     95           0             0
    ##  4    26 1987-12-30 Male     2    Moritz Geisr…     98           0             0
    ##  5    34 1979-07-29 Male     2    Andre Lakos      107           0             0
    ##  6    34 1979-03-12 Male     2    Ben Sandford      95           0             0
    ##  7    32 1982-02-03 Male     1.99 Sybren Jansma    107           0             0
    ##  8    24 1989-03-10 Male     1.98 Brynjar Joku…    107           0             0
    ##  9    25 1988-11-21 Male     1.98 Len Valjas        91           0             0
    ## 10    29 1985-01-01 Male     1.98 Jannis Baeck…    108           0             0
    ## # ℹ 2,849 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

``` r
arrange(athletes, gender, desc(age))
```

    ## # A tibble: 2,859 × 12
    ##      age birthdate  gender height name          weight gold_medals silver_medals
    ##    <dbl> <date>     <fct>   <dbl> <chr>          <dbl>       <dbl>         <dbl>
    ##  1    48 1965-11-25 Female   1.65 Angelica Mor…     57           0             0
    ##  2    45 1968-03-09 Female   1.75 Ann Swisshelm     NA           0             0
    ##  3    42 1972-01-27 Female   1.62 Mirjam Ott        NA           0             0
    ##  4    41 1972-02-22 Female   1.66 Claudia Pech…     60           0             0
    ##  5    41 1973-01-25 Female  NA    Erika Brown       NA           0             0
    ##  6    41 1972-12-19 Female   1.8  Yekaterina P…     76           0             0
    ##  7    41 1972-05-03 Female   1.74 Yulia Timofe…     71           0             0
    ##  8    40 1973-07-05 Female   1.55 Allison Pott…     NA           0             0
    ##  9    40 1973-07-07 Female   1.68 Claudia Rieg…     70           0             0
    ## 10    40 1974-01-08 Female   1.7  Deborah Mcco…     74           0             0
    ## # ℹ 2,849 more rows
    ## # ℹ 4 more variables: bronze_medals <dbl>, total_medals <dbl>, sport <fct>,
    ## #   country <fct>

## `mutate()`

`mutate()` always adds new columns at the end of your data set.

First, create with `select()` a narrow data set `athletes_narrow`.

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

### `summarize()`

The dplyr function `summarize()` (or `summarise()`) summarizes multiple
values in a single value.

``` r
athletes %>% summarize(totals = sum(gold_medals))
```

## `group_by()`

Groups table data into groups on which some other operation may operate.

For example, an important function of `summarize()` is in coordination
with the `group_by()` function. The dplyr `group_by` function take an
existing data frame and performs an operation by group.

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

## `filter()`

Probably the most useful `dplyr` function is `filter()`.

``` r
filter(athletes_narrow, height > 2)
```

    ## # A tibble: 2 × 7
    ##   name               gender   age sport      height weight   BMI
    ##   <chr>              <fct>  <dbl> <fct>       <dbl>  <dbl> <dbl>
    ## 1 Aleksandar Bundalo Male      24 Bobsleigh    2.02    106  26.0
    ## 2 Zdeno Chara        Male      36 Ice Hockey   2.06    117  27.6

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

# The table function

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
