hw01\_gapminder
================

### A basic look at the gapminder rows and columns

``` r
gpmd <- gapminder
 ncol(gpmd) # number of columns 
```

    ## [1] 6

``` r
 nrow(gpmd) # number of rows 
```

    ## [1] 1704

``` r
colnames(gpmd) # column names 
```

    ## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"

### Let's learn more about the contents of the data

``` r
range(gpmd$year) # range of years included 
```

    ## [1] 1952 2007

``` r
str(gpmd) #basic summary of data structure
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

``` r
sum(nlevels(gpmd$country)) # number of countries
```

    ## [1] 142

``` r
aggregate(gdpPercap~continent,gpmd,mean) #average gdp per Capita for each continent of all years
```

    ##   continent gdpPercap
    ## 1    Africa  2193.755
    ## 2  Americas  7136.110
    ## 3      Asia  7902.150
    ## 4    Europe 14469.476
    ## 5   Oceania 18621.609

``` r
aggregate(lifeExp~continent,gpmd,mean) #average life expectancy for the different continents as an aggregate of years
```

    ##   continent  lifeExp
    ## 1    Africa 48.86533
    ## 2  Americas 64.65874
    ## 3      Asia 60.06490
    ## 4    Europe 71.90369
    ## 5   Oceania 74.32621

``` r
range(gpmd$lifeExp) #What are the ranges of life-expectancies?
```

    ## [1] 23.599 82.603

``` r
gpmd$country[max(gpmd$lifeExp)] #Which country had the highest life expectancy?
```

    ## [1] Austria
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

``` r
gpmd$country[max(gpmd$lifeExp)] #Which country had the lowest life expectancy?
```

    ## [1] Austria
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe
