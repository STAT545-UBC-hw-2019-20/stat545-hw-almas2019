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

### Let's learn more about the structure of the data

``` r
str(gpmd,strict.width="wrap") #basic summary of data structure
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ## $ country : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ## $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3
    ##    3 ...
    ## $ year : int 1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ## $ lifeExp : num 28.8 30.3 32 34 36.1 ...
    ## $ pop : int 8425333 9240934 10267083 11537966 13079460 14880372 12881816
    ##    13867957 16317921 22227415 ...
    ## $ gdpPercap: num 779 821 853 836 740 ...

### The range of years included in the data are:

``` r
range(gpmd$year) 
```

    ## [1] 1952 2007

### How many countries are there in the dataset:

``` r
sum(nlevels(gpmd$country))
```

    ## [1] 142

Let's explore life-expectancies from the dataset a bit:
-------------------------------------------------------

### What are the ranges of life expectancies?

``` r
range(gpmd$lifeExp)
```

    ## [1] 23.599 82.603

### Which countries have the highest and lowest life-expectancies?

``` r
gpmd$country[max(gpmd$lifeExp)] #highest
```

    ## [1] Austria
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

``` r
gpmd$country[min(gpmd$lifeExp)] #lowest
```

    ## [1] Albania
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

### Average Life expectancy per continent:

``` r
aggregate(lifeExp~continent,gpmd,mean) #includes all years
```

    ##   continent  lifeExp
    ## 1    Africa 48.86533
    ## 2  Americas 64.65874
    ## 3      Asia 60.06490
    ## 4    Europe 71.90369
    ## 5   Oceania 74.32621

Let's learn about the median GDP per capita for each continent:
---------------------------------------------------------------

``` r
aggregate(gdpPercap~continent,gpmd,median) #includes all years
```

    ##   continent gdpPercap
    ## 1    Africa  1192.138
    ## 2  Americas  5465.510
    ## 3      Asia  2646.787
    ## 4    Europe 12081.749
    ## 5   Oceania 17983.304
