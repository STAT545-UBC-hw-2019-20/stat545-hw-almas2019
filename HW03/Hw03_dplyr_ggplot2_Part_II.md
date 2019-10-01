Hw03\_dplyr\_ggplot2\_Part\_II
================
Almas K.
2019-09-28

Task Option 1:Report the absolute and/or relative abundance of countries with low life expectancy over time by continent: Compute some measure of worldwide life expectancy – you decide – a mean or median or some other quantile or perhaps your current age. Then determine how many countries on each continent have a life expectancy less than this benchmark, for each year.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

``` r
gapminder %>%
  group_by(year) %>%
  mutate(median_lifeExp=median(lifeExp)) %>%
  ungroup(year) %>%
  mutate(less_than_median= if_else(lifeExp<median_lifeExp,TRUE,FALSE)) %>%
  filter (less_than_median==TRUE) %>%
  group_by(continent,year,less_than_median) %>%
  summarize(n_less_than_median=sum(less_than_median)) %>%
  print() %>%
  ggplot(aes(x=year, y=n_less_than_median,group=continent))+
  geom_bar(aes(fill=continent),position="dodge",stat="identity")
```

    ## # A tibble: 44 x 4
    ## # Groups:   continent, year [44]
    ##    continent  year less_than_median n_less_than_median
    ##    <fct>     <int> <lgl>                         <int>
    ##  1 Africa     1952 TRUE                             47
    ##  2 Africa     1957 TRUE                             47
    ##  3 Africa     1962 TRUE                             47
    ##  4 Africa     1967 TRUE                             48
    ##  5 Africa     1972 TRUE                             50
    ##  6 Africa     1977 TRUE                             49
    ##  7 Africa     1982 TRUE                             49
    ##  8 Africa     1987 TRUE                             48
    ##  9 Africa     1992 TRUE                             47
    ## 10 Africa     1997 TRUE                             48
    ## # … with 34 more rows

![](Hw03_dplyr_ggplot2_Part_II_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
gapminder %>%
  group_by(year) %>%
  mutate(median_lifeExp=median(lifeExp)) %>%
  ungroup(year) %>%
  mutate(less_than_median= if_else(lifeExp<median_lifeExp,TRUE,FALSE)) %>%
  group_by(continent,year,less_than_median) %>%
  count()
```

    ## # A tibble: 104 x 4
    ## # Groups:   continent, year, less_than_median [104]
    ##    continent  year less_than_median     n
    ##    <fct>     <int> <lgl>            <int>
    ##  1 Africa     1952 FALSE                5
    ##  2 Africa     1952 TRUE                47
    ##  3 Africa     1957 FALSE                5
    ##  4 Africa     1957 TRUE                47
    ##  5 Africa     1962 FALSE                5
    ##  6 Africa     1962 TRUE                47
    ##  7 Africa     1967 FALSE                4
    ##  8 Africa     1967 TRUE                48
    ##  9 Africa     1972 FALSE                2
    ## 10 Africa     1972 TRUE                50
    ## # … with 94 more rows

Task Option 3 : Look at the spread of GDP per capita within the continents.
---------------------------------------------------------------------------

``` r
gp_spread <- gapminder %>%
   group_by(continent)  %>%
mutate(log_gdpPercap= log(gdpPercap)) %>%
  arrange(log_gdpPercap)
gp_spread %>%
  summarize(min(gdpPercap),
            max(gdpPercap),
            sd(gdpPercap),
            IQR(gdpPercap))
```

    ## # A tibble: 5 x 5
    ##   continent `min(gdpPercap)` `max(gdpPercap)` `sd(gdpPercap)`
    ##   <fct>                <dbl>            <dbl>           <dbl>
    ## 1 Africa                241.           21951.           2828.
    ## 2 Americas             1202.           42952.           6397.
    ## 3 Asia                  331           113523.          14045.
    ## 4 Europe                974.           49357.           9355.
    ## 5 Oceania             10040.           34435.           6359.
    ## # … with 1 more variable: `IQR(gdpPercap)` <dbl>

``` r
gp_spread %>%
  ggplot(aes(x=log_gdpPercap)) +
  geom_density() +
   facet_wrap(. ~continent) 
```

![](Hw03_dplyr_ggplot2_Part_II_files/figure-markdown_github/unnamed-chunk-2-1.png)

THe distribution is right skewed for Africa, left-skewed for Europe, mostly normal for the Americas and Oceania, and It is interesting to note that the Americas are the

Task Option 4: Compute a trimmed mean of life expectancy for different years. Or a weighted mean, weighting by population. Just try something other than the plain vanilla mean.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

``` r
gapminder %>%
   group_by(continent,year)  %>%
  summarize(wt_mean=weighted.mean(lifeExp,pop,na.rm = TRUE)) %>%
  print() %>%
  ggplot(aes(x=year,y=wt_mean)) +
  geom_point()+
  facet_wrap(. ~continent) 
```

    ## # A tibble: 60 x 3
    ## # Groups:   continent [5]
    ##    continent  year wt_mean
    ##    <fct>     <int>   <dbl>
    ##  1 Africa     1952    38.8
    ##  2 Africa     1957    40.9
    ##  3 Africa     1962    43.1
    ##  4 Africa     1967    45.2
    ##  5 Africa     1972    47.2
    ##  6 Africa     1977    49.2
    ##  7 Africa     1982    51.0
    ##  8 Africa     1987    52.8
    ##  9 Africa     1992    53.4
    ## 10 Africa     1997    53.3
    ## # … with 50 more rows

![](Hw03_dplyr_ggplot2_Part_II_files/figure-markdown_github/unnamed-chunk-3-1.png)
