mde: Missing Data Explorer
================
2021-09-19

<!-- badges: start -->

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3890659.svg)](https://doi.org/10.5281/zenodo.3890659)
[![CRAN\_Status\_Badge](https://r-pkg.org/badges/version/mde)](https://cran.r-project.org/package=mde)
[![CRAN\_Release\_Badge](https://www.r-pkg.org/badges/version-ago/mde)](https://CRAN.R-project.org/package=mde)
[![Codecov test
coverage](https://codecov.io/gh/Nelson-Gon/mde/branch/master/graph/badge.svg)](https://codecov.io/gh/Nelson-Gon/mde?branch=master)
[![R-CMD-check](https://github.com/Nelson-Gon/mde/actions/workflows/devel-check.yaml/badge.svg)](https://github.com/Nelson-Gon/mde/actions/workflows/devel-check.yaml)
![test-coverage](https://github.com/Nelson-Gon/mde/workflows/test-coverage/badge.svg)
[![Project
Status](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/)
[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://lifecycle.r-lib.org/articles/stages.html)
[![license](https://img.shields.io/badge/license-GPL--3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Downloads](https://cranlogs.r-pkg.org/badges/mde)](https://cran.r-project.org/package=mde)
[![TotalDownloads](https://cranlogs.r-pkg.org/badges/grand-total/mde?color=green)](https://cran.r-project.org/package=mde)
[![GitHub last
commit](https://img.shields.io/github/last-commit/Nelson-Gon/mde.svg)](https://github.com/Nelson-Gon/mde/commits/master)
[![GitHub
issues](https://img.shields.io/github/issues/Nelson-Gon/mde.svg)](https://GitHub.com/Nelson-Gon/mde/issues/)
[![GitHub
issues-closed](https://img.shields.io/github/issues-closed/Nelson-Gon/mde.svg)](https://GitHub.com/Nelson-Gon/mde/issues?q=is%3Aissue+is%3Aclosed)
[![PRs
Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://makeapullrequest.com)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Nelson-Gon/mde/graphs/commit-activity)
<!-- badges: end -->

<img src='https://github.com/Nelson-Gon/mde/blob/master/man/figures/mde_icon_2.png?raw=true' align="right" height="120" width="120"/>

The goal of `mde` is to ease exploration of missingness.

**Installation**

**CRAN release**

``` r
install.packages("mde")
```

**Stable Development version**

``` r
devtools::install_github("Nelson-Gon/mde")


devtools::install_github("Nelson-Gon/mde",  build_vignettes=TRUE)
```

**Unstable Development version**

``` r
devtools::install_github("Nelson-Gon/mde@develop")
```

**Loading the package**

``` r
library(mde)
#> Welcome to mde. This is mde version 0.3.2.
#>  Please file issues and feedback at https://www.github.com/Nelson-Gon/mde/issues
#> Turn this message off using 'suppressPackageStartupMessages(library(mde))'
#>  Happy Exploration :)
```

## Exploring missingness

To get a simple missingness report, use `na_summary`:

``` r
na_summary(airquality)
#>   variable missing complete percent_complete percent_missing
#> 1      Day       0      153        100.00000        0.000000
#> 2    Month       0      153        100.00000        0.000000
#> 3    Ozone      37      116         75.81699       24.183007
#> 4  Solar.R       7      146         95.42484        4.575163
#> 5     Temp       0      153        100.00000        0.000000
#> 6     Wind       0      153        100.00000        0.000000
```

To sort this summary by a given column :

``` r
na_summary(airquality,sort_by = "percent_complete")
#>   variable missing complete percent_complete percent_missing
#> 3    Ozone      37      116         75.81699       24.183007
#> 4  Solar.R       7      146         95.42484        4.575163
#> 1      Day       0      153        100.00000        0.000000
#> 2    Month       0      153        100.00000        0.000000
#> 5     Temp       0      153        100.00000        0.000000
#> 6     Wind       0      153        100.00000        0.000000
```

To sort by `percent_missing` instead:

``` r
na_summary(airquality, sort_by = "percent_missing")
#>   variable missing complete percent_complete percent_missing
#> 1      Day       0      153        100.00000        0.000000
#> 2    Month       0      153        100.00000        0.000000
#> 5     Temp       0      153        100.00000        0.000000
#> 6     Wind       0      153        100.00000        0.000000
#> 4  Solar.R       7      146         95.42484        4.575163
#> 3    Ozone      37      116         75.81699       24.183007
```

To sort the above in descending order:

``` r
na_summary(airquality, sort_by="percent_missing", descending = TRUE)
#>   variable missing complete percent_complete percent_missing
#> 3    Ozone      37      116         75.81699       24.183007
#> 4  Solar.R       7      146         95.42484        4.575163
#> 1      Day       0      153        100.00000        0.000000
#> 2    Month       0      153        100.00000        0.000000
#> 5     Temp       0      153        100.00000        0.000000
#> 6     Wind       0      153        100.00000        0.000000
```

To exclude certain columns from the analysis:

``` r
na_summary(airquality, exclude_cols = c("Day", "Wind"))
#>   variable missing complete percent_complete percent_missing
#> 1    Month       0      153        100.00000        0.000000
#> 2    Ozone      37      116         75.81699       24.183007
#> 3  Solar.R       7      146         95.42484        4.575163
#> 4     Temp       0      153        100.00000        0.000000
```

To include or exclude via regex match:

``` r
na_summary(airquality, regex_kind = "inclusion",pattern_type = "starts_with", pattern = "O|S")
#>   variable missing complete percent_complete percent_missing
#> 1    Ozone      37      116         75.81699       24.183007
#> 2  Solar.R       7      146         95.42484        4.575163
```

``` r
na_summary(airquality, regex_kind = "exclusion",pattern_type = "regex", pattern = "^[O|S]")
#>   variable missing complete percent_complete percent_missing
#> 1      Day       0      153              100               0
#> 2    Month       0      153              100               0
#> 3     Temp       0      153              100               0
#> 4     Wind       0      153              100               0
```

To get this summary by group:

``` r
test2 <- data.frame(ID= c("A","A","B","A","B"), Vals = c(rep(NA,4),"No"),ID2 = c("E","E","D","E","D"))

na_summary(test2,grouping_cols = c("ID","ID2"))
#> # A tibble: 2 x 7
#>   ID    ID2   variable missing complete percent_complete percent_missing
#>   <chr> <chr> <chr>      <dbl>    <dbl>            <dbl>           <dbl>
#> 1 B     D     Vals           1        1               50              50
#> 2 A     E     Vals           3        0                0             100
```

``` r
na_summary(test2, grouping_cols="ID")
#> Warning in na_summary.data.frame(test2, grouping_cols = "ID"): All non grouping
#> values used. Using select non groups is currently not supported
#> # A tibble: 4 x 6
#>   ID    variable missing complete percent_complete percent_missing
#>   <chr> <chr>      <dbl>    <dbl>            <dbl>           <dbl>
#> 1 A     Vals           3        0                0             100
#> 2 A     ID2            0        3              100               0
#> 3 B     Vals           1        1               50              50
#> 4 B     ID2            0        2              100               0
```

-   `get_na_counts`

This provides a convenient way to show the number of missing values
column-wise. It is relatively fast(tests done on about 400,000 rows,
took a few microseconds.)

To get the number of missing values in each column of `airquality`, we
can use the function as follows:

``` r
get_na_counts(airquality)
#>   Ozone Solar.R Wind Temp Month Day
#> 1    37       7    0    0     0   0
```

The above might be less useful if one would like to get the results by
group. In that case, one can provide a grouping vector of names in
`grouping_cols`.

``` r
test <- structure(list(Subject = structure(c(1L, 1L, 2L, 2L), .Label = c("A", 
"B"), class = "factor"), res = c(NA, 1, 2, 3), ID = structure(c(1L, 
1L, 2L, 2L), .Label = c("1", "2"), class = "factor")), class = "data.frame", row.names = c(NA, 
-4L))

get_na_counts(test, grouping_cols = "ID")
#> # A tibble: 2 x 3
#>   ID    Subject   res
#>   <fct>   <int> <int>
#> 1 1           0     1
#> 2 2           0     0
```

-   `percent_missing`

This is a very simple to use but quick way to take a look at the
percentage of data that is missing column-wise.

``` r

percent_missing(airquality)
#>      Ozone  Solar.R Wind Temp Month Day
#> 1 24.18301 4.575163    0    0     0   0
```

We can get the results by group by providing an optional `grouping_cols`
character vector.

``` r
percent_missing(test, grouping_cols = "Subject")
#> # A tibble: 2 x 3
#>   Subject   res    ID
#>   <fct>   <dbl> <dbl>
#> 1 A          50     0
#> 2 B           0     0
```

To exclude some columns from the above exploration, one can provide an
optional character vector in `exclude_cols`.

``` r
percent_missing(airquality,exclude_cols = c("Day","Temp"))
#>      Ozone  Solar.R Wind Month
#> 1 24.18301 4.575163    0     0
```

-   `sort_by_missingness`

This provides a very simple but relatively fast way to sort variables by
missingness. Unless otherwise stated, this does not currently support
arranging grouped percents.

Usage:

``` r

sort_by_missingness(airquality, sort_by = "counts")
#>   variable percent
#> 1     Wind       0
#> 2     Temp       0
#> 3    Month       0
#> 4      Day       0
#> 5  Solar.R       7
#> 6    Ozone      37
```

To sort in descending order:

``` r
sort_by_missingness(airquality, sort_by = "counts", descend = TRUE)
#>   variable percent
#> 1    Ozone      37
#> 2  Solar.R       7
#> 3     Wind       0
#> 4     Temp       0
#> 5    Month       0
#> 6      Day       0
```

To use percentages instead:

``` r
sort_by_missingness(airquality, sort_by = "percents")
#>   variable   percent
#> 1     Wind  0.000000
#> 2     Temp  0.000000
#> 3    Month  0.000000
#> 4      Day  0.000000
#> 5  Solar.R  4.575163
#> 6    Ozone 24.183007
```

## Recoding as NA

-   `recode_as_na`

As the name might imply, this converts any value or vector of values to
`NA` i.e. we take a value such as “missing” or “NA” (not a real `NA`
according to `R`) and convert it to R’s known handler for missing values
(`NA`).

To use the function out of the box (with default arguments), one simply
does something like:

``` r
dummy_test <- data.frame(ID = c("A","B","B","A"), 
                         values = c("n/a",NA,"Yes","No"))
# Convert n/a and no to NA
head(recode_as_na(dummy_test, value = c("n/a","No")))
#>   ID values
#> 1  A   <NA>
#> 2  B   <NA>
#> 3  B    Yes
#> 4  A   <NA>
```

Great, but I want to do so for specific columns not the entire dataset.
You can do this by providing column names to `subset_cols`.

``` r

another_dummy <- data.frame(ID = 1:5, Subject = 7:11, 
Change = c("missing","n/a",2:4 ))
# Only change values at the column Change
head(recode_as_na(another_dummy, subset_cols = "Change", value = c("n/a","missing")))
#>   ID Subject Change
#> 1  1       7   <NA>
#> 2  2       8   <NA>
#> 3  3       9      2
#> 4  4      10      3
#> 5  5      11      4
```

To recode columns using
[RegEx](https://en.wikipedia.org/wiki/Regular_expression),one can
provide `pattern_type` and a target `pattern`. Currently supported
`pattern_types` are `starts_with`, `ends_with`, `contains` and `regex`.
See docs for more details.:

``` r
# only change at columns that start with Solar
head(recode_as_na(airquality,value=190,pattern_type="starts_with",pattern="Solar"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41      NA  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    NA      NA 14.3   56     5   5
#> 6    28      NA 14.9   66     5   6
```

``` r
# recode at columns that start with O or S(case sensitive)
head(recode_as_na(airquality,value=c(67,118),pattern_type="starts_with",pattern="S|O"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36      NA  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    NA      NA 14.3   56     5   5
#> 6    28      NA 14.9   66     5   6
```

``` r
# use my own RegEx
head(recode_as_na(airquality,value=c(67,118),pattern_type="regex",pattern="(?i)^(s|o)"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36      NA  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    NA      NA 14.3   56     5   5
#> 6    28      NA 14.9   66     5   6
```

-   `recode_as_na_if`

This function allows one to deliberately introduce missing values if a
column meets a certain threshold of missing values. This is similar to
`amputation` but is much more basic. It is only provided here because it
is hoped it may be useful to someone for whatever reason.

``` r
head(recode_as_na_if(airquality,sign="gt", percent_na=20))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    NA     190  7.4   67     5   1
#> 2    NA     118  8.0   72     5   2
#> 3    NA     149 12.6   74     5   3
#> 4    NA     313 11.5   62     5   4
#> 5    NA      NA 14.3   56     5   5
#> 6    NA      NA 14.9   66     5   6
```

-   `recode_as_na_str`

This allows recoding as `NA` based on a string match.

``` r
partial_match <- data.frame(A=c("Hi","match_me","nope"), B=c(NA, "not_me","nah"))

recode_as_na_str(partial_match,"ends_with","ME", case_sensitive=FALSE)
#>      A    B
#> 1   Hi <NA>
#> 2 <NA> <NA>
#> 3 nope  nah
```

-   `recode_as_na_for`

For all values greater/less/less or equal/greater or equal than some
value, can I convert them to `NA`?!

**Yes You Can!** All we have to do is use `recode_as_na_for`:

``` r
head(recode_as_na_for(airquality,criteria="gt",value=25))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    NA      NA  7.4   NA     5   1
#> 2    NA      NA  8.0   NA     5   2
#> 3    12      NA 12.6   NA     5   3
#> 4    18      NA 11.5   NA     5   4
#> 5    NA      NA 14.3   NA     5   5
#> 6    NA      NA 14.9   NA     5   6
```

To do so at specific columns, pass an optional `subset_cols` character
vector:

``` r
head(recode_as_na_for(airquality, value=40,subset_cols=c("Solar.R","Ozone"), criteria="gt"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    NA      NA  7.4   67     5   1
#> 2    36      NA  8.0   72     5   2
#> 3    12      NA 12.6   74     5   3
#> 4    18      NA 11.5   62     5   4
#> 5    NA      NA 14.3   56     5   5
#> 6    28      NA 14.9   66     5   6
```

## Recoding NA as

-   `recode_na_as`

Sometimes, for whatever reason, one would like to replace `NA`s with
whatever value they would like. `recode_na_as` provides a very simple
way to do just that.

``` r
head(recode_na_as(airquality))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5     0       0 14.3   56     5   5
#> 6    28       0 14.9   66     5   6

# use NaN

head(recode_na_as(airquality, value=NaN))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5   NaN     NaN 14.3   56     5   5
#> 6    28     NaN 14.9   66     5   6
```

As a “bonus”, you can manipulate the data only at specific columns as
shown here:

``` r
head(recode_na_as(airquality, value=0, subset_cols="Ozone"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5     0      NA 14.3   56     5   5
#> 6    28      NA 14.9   66     5   6
```

The above also supports custom recoding similar to `recode_na_as`:

``` r
head(mde::recode_na_as(airquality, value=0, pattern_type="starts_with",pattern="Solar"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    NA       0 14.3   56     5   5
#> 6    28       0 14.9   66     5   6
```

-   `column_based_recode`

Ever needed to change values in a given column based on the proportions
of `NA`s in other columns(row-wise)?!. The goal of `column_based_recode`
is to achieve just that. Let’s see how we could do this with a simple
example:

``` r

head(column_based_recode(airquality, values_from = "Wind", values_to="Wind", pattern_type = "regex", pattern = "Solar|Ozone"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    NA      NA  0.0   56     5   5
#> 6    28      NA 14.9   66     5   6
```

-   `custom_na_recode`

This allows recoding `NA` values with common stats functions such as
`mean`,`max`,`min`,`sd`.

To use default values:

``` r
head(custom_na_recode(airquality))
#>      Ozone  Solar.R Wind Temp Month Day
#> 1 41.00000 190.0000  7.4   67     5   1
#> 2 36.00000 118.0000  8.0   72     5   2
#> 3 12.00000 149.0000 12.6   74     5   3
#> 4 18.00000 313.0000 11.5   62     5   4
#> 5 42.12931 185.9315 14.3   56     5   5
#> 6 28.00000 185.9315 14.9   66     5   6
```

To use select columns:

``` r


head(custom_na_recode(airquality,func="mean",across_columns=c("Solar.R","Ozone")))
#>      Ozone  Solar.R Wind Temp Month Day
#> 1 41.00000 190.0000  7.4   67     5   1
#> 2 36.00000 118.0000  8.0   72     5   2
#> 3 12.00000 149.0000 12.6   74     5   3
#> 4 18.00000 313.0000 11.5   62     5   4
#> 5 42.12931 185.9315 14.3   56     5   5
#> 6 28.00000 185.9315 14.9   66     5   6
```

To use a function from another package to perform replacements:

To perform a forward fill with `dplyr`’s `lead`:

``` r
# use lag for a backfill
head(custom_na_recode(airquality,func=dplyr::lead ))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    23      99 14.3   56     5   5
#> 6    28      19 14.9   66     5   6
```

To perform replacement by group:

``` r
some_data <- data.frame(ID=c("A1","A1","A1","A2","A2", "A2"),A=c(5,NA,0,8,3,4),B=c(10,0,0,NA,5,6),C=c(1,NA,NA,25,7,8))

head(custom_na_recode(some_data,func = "mean", grouping_cols = "ID"))
#> # A tibble: 6 x 4
#>   ID        A     B     C
#>   <chr> <dbl> <dbl> <dbl>
#> 1 A1      5    10       1
#> 2 A1      2.5   0       1
#> 3 A1      0     0       1
#> 4 A2      8     5.5    25
#> 5 A2      3     5       7
#> 6 A2      4     6       8
```

Across specific columns:

``` r
head(custom_na_recode(some_data,func = "mean", grouping_cols = "ID", across_columns = c("C", "A")))
#> # A tibble: 6 x 4
#>   ID        A     B     C
#>   <chr> <dbl> <dbl> <dbl>
#> 1 A1      5      10     1
#> 2 A1      2.5     0     1
#> 3 A1      0       0     1
#> 4 A2      8      NA    25
#> 5 A2      3       5     7
#> 6 A2      4       6     8
```

-   `recode_na_if`

Given a `data.frame` object, one can recode `NA`s as another value based
on a grouping variable. In the example below, we replace all `NA`s in
all columns with 0s if the ID is `A2` or `A3`

``` r
some_data <- data.frame(ID=c("A1","A2","A3", "A4"), 
                        A=c(5,NA,0,8), B=c(10,0,0,1),
                        C=c(1,NA,NA,25))
                        
head(recode_na_if(some_data,grouping_col="ID", target_groups=c("A2","A3"),
           replacement= 0))   
#> # A tibble: 4 x 4
#>   ID        A     B     C
#>   <chr> <dbl> <dbl> <dbl>
#> 1 A1        5    10     1
#> 2 A2        0     0     0
#> 3 A3        0     0     0
#> 4 A4        8     1    25
```

## Dropping NAs

-   `drop_na_if`

Suppose you wanted to drop any column that has a percentage of `NA`s
greater than or equal to a certain value? `drop_na_if` does just that.

We can drop any columns that have greater than or equal(gteq) to 24% of
the values missing from `airquality`:

``` r
head(drop_na_if(airquality, sign="gteq",percent_na = 24))
#>   Solar.R Wind Temp Month Day
#> 1     190  7.4   67     5   1
#> 2     118  8.0   72     5   2
#> 3     149 12.6   74     5   3
#> 4     313 11.5   62     5   4
#> 5      NA 14.3   56     5   5
#> 6      NA 14.9   66     5   6
```

The above also supports less than or equal to(`lteq`), equal to(`eq`),
greater than(`gt`) and less than(`lt`).

To keep certain columns despite fitting the target `percent_na`
criteria, one can provide an optional `keep_columns` character vector.

``` r

head(drop_na_if(airquality, percent_na = 24, keep_columns = "Ozone"))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    NA      NA 14.3   56     5   5
#> 6    28      NA 14.9   66     5   6
```

Compare the above result to the following:

``` r
head(drop_na_if(airquality, percent_na = 24))
#>   Solar.R Wind Temp Month Day
#> 1     190  7.4   67     5   1
#> 2     118  8.0   72     5   2
#> 3     149 12.6   74     5   3
#> 4     313 11.5   62     5   4
#> 5      NA 14.3   56     5   5
#> 6      NA 14.9   66     5   6
```

To drop groups that meet a set missingness criterion, we proceed as
follows.

``` r
grouped_drop <- structure(list(ID = c("A", "A", "B", "A", "B"), 
          Vals = c(4, NA,  NA, NA, NA), Values = c(5, 6, 7, 8, NA)), 
          row.names = c(NA, -5L), class = "data.frame")
# Drop all columns for groups that meet a percent missingness of greater than or
# equal to 67
drop_na_if(grouped_drop,percent_na = 67,sign="gteq",
                                    grouping_cols = "ID")
#> # A tibble: 3 x 3
#>   ID     Vals Values
#>   <chr> <dbl>  <dbl>
#> 1 A         4      5
#> 2 A        NA      6
#> 3 A        NA      8
```

-   `drop_row_if`

This is similar to `drop_na_if` but does operations rowwise not
columnwise. Compare to the example above:

``` r
# Drop rows with at least two NAs
head(drop_row_if(airquality, sign="gteq", type="count" , value = 2))
#> Dropped 2 rows.
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 6    28      NA 14.9   66     5   6
#> 7    23     299  8.6   65     5   7
```

To drop based on percentages:

``` r
# Drops 42 rows
head(drop_row_if(airquality, type="percent", value=16, sign="gteq",
                 as_percent=TRUE))
#> Dropped 42 rows.
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 7    23     299  8.6   65     5   7
#> 8    19      99 13.8   59     5   8
```

For more details, please see the documentation of `drop_row_if`.

-   `drop_na_at`

This provides a simple way to drop missing values only at specific
columns. It currently only returns those columns with their missing
values removed. See usage below. Further details are given in the
documentation. It is currently case sensitive.

``` r
head(drop_na_at(airquality,pattern_type = "starts_with","O"))
#>   Ozone
#> 1    41
#> 2    36
#> 3    12
#> 4    18
#> 5    28
#> 6    23
```

-   `drop_all_na`

This drops columns where all values are missing.

``` r
test2 <- data.frame(ID= c("A","A","B","A","B"), Vals = c(4,rep(NA, 4))) 
drop_all_na(test2, grouping_cols="ID")
#> # A tibble: 3 x 2
#>   ID     Vals
#>   <chr> <dbl>
#> 1 A         4
#> 2 A        NA
#> 3 A        NA
```

Alternatively, we can drop groups where all variables are all NA.

``` r
test2 <- data.frame(ID= c("A","A","B","A","B"), Vals = rep(NA, 5)) 

head(drop_all_na(test, grouping_cols = "ID"))
#> # A tibble: 4 x 3
#>   Subject   res ID   
#>   <fct>   <dbl> <fct>
#> 1 A          NA 1    
#> 2 A           1 1    
#> 3 B           2 2    
#> 4 B           3 2
```

-   `dict_recode`

If one would like to recode column values using a “dictionary”,
`dict_recode` provides a simple way to do that. For example, if one
would like to convert `NA` values in `Solar.R` to 520 and those in
`Ozone` to 42, one simply calls the following:

``` r
head(dict_recode(airquality, use_func="recode_na_as",
                 patterns = c("solar", "ozone"),
                 pattern_type="starts_with", values = c(520,42)))
#>   Ozone Solar.R Wind Temp Month Day
#> 1    41     190  7.4   67     5   1
#> 2    36     118  8.0   72     5   2
#> 3    12     149 12.6   74     5   3
#> 4    18     313 11.5   62     5   4
#> 5    42     520 14.3   56     5   5
#> 6    28     520 14.9   66     5   6
```

------------------------------------------------------------------------

Please note that the `mde` project is released with a [Contributor Code
of
Conduct](https://github.com/Nelson-Gon/mde/blob/master/.github/CODE_OF_CONDUCT.md).
By contributing to this project, you agree to abide by its terms.

For further exploration, please `browseVignettes("mde")`.

To raise an issue, please do so
[here](https://github.com/Nelson-Gon/mde/issues)

Thank you, feedback is always welcome :)
