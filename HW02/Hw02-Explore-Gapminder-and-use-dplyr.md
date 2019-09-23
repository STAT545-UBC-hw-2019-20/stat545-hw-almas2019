Hw02-Explore Gapminder and use dplyr
================
Almas K.
2019-09-20

    ## ── Attaching packages ──────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Excercise 1:
============

1.1 Filter for selected 3 countries in the 1970s
------------------------------------------------

``` r
gapminder %>% 
  filter(year >= 1970 & year <= 1979, country == "Canada"|country == "Sweden"|country == "Pakistan")
```

    ## # A tibble: 6 x 6
    ##   country  continent  year lifeExp      pop gdpPercap
    ##   <fct>    <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Canada   Americas   1972    72.9 22284500    18971.
    ## 2 Canada   Americas   1977    74.2 23796400    22091.
    ## 3 Pakistan Asia       1972    51.9 69325921     1050.
    ## 4 Pakistan Asia       1977    54.0 78152686     1176.
    ## 5 Sweden   Europe     1972    74.7  8122293    17832.
    ## 6 Sweden   Europe     1977    75.4  8251648    18856.

1.2 Select country and gdpPercap from 1.1 dataset
-------------------------------------------------

``` r
gapminder %>% 
  filter(year >= 1970 & year <= 1979, country == "Canada"|country == "Sweden"|country == "Pakistan") %>%
select(country, gdpPercap)
```

    ## # A tibble: 6 x 2
    ##   country  gdpPercap
    ##   <fct>        <dbl>
    ## 1 Canada      18971.
    ## 2 Canada      22091.
    ## 3 Pakistan     1050.
    ## 4 Pakistan     1176.
    ## 5 Sweden      17832.
    ## 6 Sweden      18856.

1.3 Filter gapminder to all entries that have experienced a drop in life expectancy.
------------------------------------------------------------------------------------

``` r
gapminder %>%
  group_by(country) %>%
mutate(increaseLifeExp=lifeExp-lag(lifeExp)) %>%
  filter(increaseLifeExp<0) 
```

    ## # A tibble: 102 x 7
    ## # Groups:   country [52]
    ##    country  continent  year lifeExp     pop gdpPercap increaseLifeExp
    ##    <fct>    <fct>     <int>   <dbl>   <int>     <dbl>           <dbl>
    ##  1 Albania  Europe     1992    71.6 3326498     2497.          -0.419
    ##  2 Angola   Africa     1987    39.9 7874230     2430.          -0.036
    ##  3 Benin    Africa     2002    54.4 7026113     1373.          -0.371
    ##  4 Botswana Africa     1992    62.7 1342614     7954.          -0.877
    ##  5 Botswana Africa     1997    52.6 1536536     8647.         -10.2  
    ##  6 Botswana Africa     2002    46.6 1630347    11004.          -5.92 
    ##  7 Bulgaria Europe     1977    70.8 8797022     7612.          -0.09 
    ##  8 Bulgaria Europe     1992    71.2 8658506     6303.          -0.15 
    ##  9 Bulgaria Europe     1997    70.3 8066057     5970.          -0.87 
    ## 10 Burundi  Africa     1992    44.7 5809236      632.          -3.48 
    ## # … with 92 more rows

1.4 Filter gapminder so that it shows the max GDP for each country:
-------------------------------------------------------------------

``` r
gapminder %>% 
group_by(country) %>%
  summarize(max(gdpPercap))
```

    ## # A tibble: 142 x 2
    ##    country     `max(gdpPercap)`
    ##    <fct>                  <dbl>
    ##  1 Afghanistan             978.
    ##  2 Albania                5937.
    ##  3 Algeria                6223.
    ##  4 Angola                 5523.
    ##  5 Argentina             12779.
    ##  6 Australia             34435.
    ##  7 Austria               36126.
    ##  8 Bahrain               29796.
    ##  9 Bangladesh             1391.
    ## 10 Belgium               33693.
    ## # … with 132 more rows

1.5 Scatterplot of Canada's Life Expentancy vs GDP per Capita
-------------------------------------------------------------

``` r
gapminder %>%
  filter(country == "Canada") %>%
  ggplot(aes(log(gdpPercap),lifeExp))+
  geom_point()
```

![](Hw02-Explore-Gapminder-and-use-dplyr_files/figure-markdown_github/unnamed-chunk-5-1.png) \#\#Excercise 2: : Explore individual variables with dplyr We will look at continent as our categorical and population as our quantitative in the gapminder dataset. \#\#\#2.1 : Summary Table

``` r
gapminder %>% 
  group_by(continent) %>% #This tells you the possible values for continent
 summarize(min(pop) # The minimum population by each continent
           ,max(pop) # The maximum (min and max make up range)
           ,mean(pop) #Th mean population
           ,sd(pop), #The standard deviation or the spread of the population
           IQR(pop)) #The Interquartile range (also another measure of spread)
```

    ## # A tibble: 5 x 6
    ##   continent `min(pop)` `max(pop)` `mean(pop)`  `sd(pop)` `IQR(pop)`
    ##   <fct>          <int>      <int>       <dbl>      <dbl>      <dbl>
    ## 1 Africa         60011  135031164    9916003.  15490923.   9459415.
    ## 2 Americas      662850  301139947   24504795.  50979430.  15377950.
    ## 3 Asia          120447 1318683096   77038722. 206885205.  42455955 
    ## 4 Europe        147962   82400996   17169765.  20519438.  17471367 
    ## 5 Oceania      1994794   20434176    8874672.   6506342.  11152412.

From this we can see 5 possible values for continent, as well as the range and spread. Now onto the distribution for \#2: \#\#\# 2.2 Density Plot

``` r
gapminder %>% 
  ggplot(aes(x=log(pop)))+ #Transform to log 
  geom_density()+ #Plot density
    facet_wrap(. ~continent) #Allows you to plot the density plots separately for each continent and side by side 
```

![](Hw02-Explore-Gapminder-and-use-dplyr_files/figure-markdown_github/unnamed-chunk-7-1.png) The population was log transformed. After log transformation, it is interesting to note the bimodal distribution for Oceania and Europe's population data

Excercise 3 : Produce Various Plots
-----------------------------------

I will use the
