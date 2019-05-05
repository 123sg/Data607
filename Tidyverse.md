Data 607 - Tidyverse Assignment
================
Suma Gopal

In this demonstration, we will use the following packages: forcats.
===================================================================

``` r
  library(knitr)
  library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.1.0       ✔ purrr   0.2.5  
    ## ✔ tibble  2.0.0       ✔ dplyr   0.8.0.1
    ## ✔ tidyr   0.8.2       ✔ stringr 1.3.1  
    ## ✔ readr   1.3.1       ✔ forcats 0.4.0

    ## ── Conflicts ──────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
  #library(dplyr)
  #library(magrittr)
  library(ggplot2)
  #library(tidyr)
  library(DT)
  library(readr)
  library(forcats)
```

readr package
-------------

The readr package is a fast way to read in rectangular data like a CSV file. It is useful in that it is capable of parsing many types of data.

Forcats package
---------------

The forcats package provides tools for solving problems with factors. Factors are useful for categorical data, and when there are variables with a set of fixed known values, and for when you want to show character vectors in non-alphabetical order. It can also be used to convert unknown values to NA.

Below we will apply a readr function to read in a csv file dataset of Marvel comics. Source: <https://www.kaggle.com/fivethirtyeight/fivethirtyeight-comic-characters-dataset>. Next we will apply a fo

### Data: Comic Characters

``` r
  comicsData <- read_csv("marvel-wikia-data.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   page_id = col_double(),
    ##   name = col_character(),
    ##   urlslug = col_character(),
    ##   ID = col_character(),
    ##   ALIGN = col_character(),
    ##   EYE = col_character(),
    ##   HAIR = col_character(),
    ##   SEX = col_character(),
    ##   GSM = col_character(),
    ##   ALIVE = col_character(),
    ##   APPEARANCES = col_double(),
    ##   `FIRST APPEARANCE` = col_character(),
    ##   Year = col_double()
    ## )

``` r
  datatable(comicsData, options = list(pageLength = 5))
```

    ## Warning in instance$preRenderHook(instance): It seems your data is too
    ## big for client-side DataTables. You may consider server-side processing:
    ## https://rstudio.github.io/DT/server.html

<!--html_preserve-->

<!--/html_preserve-->
Next we will apply forcats function to combine levels.

``` r
    #Forcats
  comicsData %>%
    count(EYE, sort = TRUE)
```

    ## # A tibble: 25 x 2
    ##    EYE             n
    ##    <chr>       <int>
    ##  1 <NA>         9767
    ##  2 Blue Eyes    1962
    ##  3 Brown Eyes   1924
    ##  4 Green Eyes    613
    ##  5 Black Eyes    555
    ##  6 Red Eyes      508
    ##  7 White Eyes    400
    ##  8 Yellow Eyes   256
    ##  9 Grey Eyes      95
    ## 10 Hazel Eyes     76
    ## # … with 15 more rows

We see that there are 25 different eye colors. This could be too many to display on a plot. We can reduce this down the 6 top eye colors by assigning all infrequent eye colors to "other," using the function fct\_lump(). In the function we can set the number of levels we want to keep, which in this case is 6.

``` r
  comicsData %>% mutate(EYE = fct_lump(EYE, n = 6)) %>% count(EYE, sort = TRUE)
```

    ## Warning: Factor `EYE` contains implicit NA, consider using
    ## `forcats::fct_explicit_na`

    ## # A tibble: 8 x 2
    ##   EYE            n
    ##   <fct>      <int>
    ## 1 <NA>        9767
    ## 2 Blue Eyes   1962
    ## 3 Brown Eyes  1924
    ## 4 Other        647
    ## 5 Green Eyes   613
    ## 6 Black Eyes   555
    ## 7 Red Eyes     508
    ## 8 White Eyes   400

Let's say if we want to order an unordered categorical variable by its frequencey. We can do so by applying fct\_infreq().

``` r
eyecolors <- comicsData %>% mutate(EYE = fct_lump(EYE, n = 6)) %>% count(EYE, sort = TRUE)
```

    ## Warning: Factor `EYE` contains implicit NA, consider using
    ## `forcats::fct_explicit_na`

``` r
datatable(eyecolors)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-4751114dfef231e4ea53">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8"],[null,"Blue Eyes","Brown Eyes","Other","Green Eyes","Black Eyes","Red Eyes","White Eyes"],[9767,1962,1924,647,613,555,508,400]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>EYE<\/th>\n      <th>n<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":2},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
ggplot(data = eyecolors) + geom_bar(mapping = aes(x = EYE, y = n, fill = EYE), stat = "identity")
```

![](Tidyverse_files/figure-markdown_github/unnamed-chunk-5-2.png)

``` r
#ggplot(eyecolors, mapping = aes(x = EYE)) + 
 #geom_bar()

#ggplot(comicsData, aes(x = fct_infreq(EYE))) + 
 # geom_bar() + 
  #coord_flip()
```