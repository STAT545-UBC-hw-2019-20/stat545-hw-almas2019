hw01\_gapminder
================

### A basic look at the gapminder rows and columns

``` r
gpmd <- gapminder
 ncol(gpmd) # The number of columns of the gapminder data
```

    ## [1] 6

``` r
 nrow(gpmd) # The number of rows of the gapminder data
```

    ## [1] 1704

``` r
colnames(gpmd) # The column names of the gapminderdata
```

    ## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
