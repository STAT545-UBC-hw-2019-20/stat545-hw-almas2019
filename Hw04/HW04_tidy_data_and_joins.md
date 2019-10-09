---
title: "Assignment 04: Tidy data and joins"
author: "Almas K."
date: '2019-10-07'
output:
  html_document:
    keep_md: TRUE
---

---


# Exercise 1: Univariate Data Reshaping

## Univariate Option 1:Life Expectancy vs Year

### 1.1 Data in Wider Format
Let's choose 2 other countries to compare to Canada 

```r
gapwide <- gapminder %>% 
  filter((country=="Algeria")|(country=="Bahrain")|(country=="Canada")) %>%
  pivot_wider(id_cols= year, 
              names_from= country,
              values_from = lifeExp) 
  gapwide %>%
    DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-d9856fc4f005dc763074" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-d9856fc4f005dc763074">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12"],[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007],[43.077,45.685,48.303,51.407,54.518,58.014,61.368,65.799,67.744,69.152,70.994,72.301],[50.939,53.832,56.923,59.923,63.3,65.593,69.052,70.75,72.601,73.925,74.795,75.635],[68.75,69.96,71.3,72.13,72.88,74.21,75.76,76.86,77.95,78.61,79.77,80.653]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>year<\/th>\n      <th>Algeria<\/th>\n      <th>Bahrain<\/th>\n      <th>Canada<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### 1.2 Scatter Plot for 2 countries against each other:

Let's choose Algeria and Bahrain to plot against each other.

```r
gapwide %>%
  ggplot(aes(x=Algeria, y=Bahrain))+
  geom_point()+
  labs(title="Scatter Plot of Bahrain's Life Expectancy vs Algeria's Life Expectancy",x="Algeria(Years)",y="Bahrain(Years)")+
  theme_bw()
```

![](HW04_tidy_data_and_joins_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

Over time, life Expectancy of Bahrain is higher compared to Algeria 

### 1.3 Relengthen Data:

```r
gapwide %>%
  pivot_longer(cols = c(-year),
               names_to = "Country",
               values_to="Life Expectancy") %>%
  DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-fc2aa2966816441eef5e" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-fc2aa2966816441eef5e">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36"],[1952,1952,1952,1957,1957,1957,1962,1962,1962,1967,1967,1967,1972,1972,1972,1977,1977,1977,1982,1982,1982,1987,1987,1987,1992,1992,1992,1997,1997,1997,2002,2002,2002,2007,2007,2007],["Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada","Algeria","Bahrain","Canada"],[43.077,50.939,68.75,45.685,53.832,69.96,48.303,56.923,71.3,51.407,59.923,72.13,54.518,63.3,72.88,58.014,65.593,74.21,61.368,69.052,75.76,65.799,70.75,76.86,67.744,72.601,77.95,69.152,73.925,78.61,70.994,74.795,79.77,72.301,75.635,80.653]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>year<\/th>\n      <th>Country<\/th>\n      <th>Life Expectancy<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,3]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

# Excercise 2: Multivariate Data Reshaping:

## Multivariate Option 1: Life Expectancy-Gdp

### 2.1 Wide Tibble

```r
gap_mulit_wide <- gapminder %>%
filter(country== "Australia"| country == "Bahrain") %>%
  pivot_wider(id_cols = year,
              names_from = country,
              values_from = c(lifeExp,gdpPercap))
gap_mulit_wide %>% knitr::kable()
```



 year   lifeExp_Australia   lifeExp_Bahrain   gdpPercap_Australia   gdpPercap_Bahrain
-----  ------------------  ----------------  --------------------  ------------------
 1952              69.120            50.939              10039.60            9867.085
 1957              70.330            53.832              10949.65           11635.799
 1962              70.930            56.923              12217.23           12753.275
 1967              71.100            59.923              14526.12           14804.673
 1972              71.930            63.300              16788.63           18268.658
 1977              73.490            65.593              18334.20           19340.102
 1982              74.740            69.052              19477.01           19211.147
 1987              76.320            70.750              21888.89           18524.024
 1992              77.560            72.601              23424.77           19035.579
 1997              78.830            73.925              26997.94           20292.017
 2002              80.370            74.795              30687.75           23403.559
 2007              81.235            75.635              34435.37           29796.048

### 2.2 Relengthen Data:

```r
gap_mulit_wide %>%
  pivot_longer(cols=c(-year),
               names_to = c(".value", "country"),
               names_sep = "_") %>%
  knitr::kable()
```



 year  country      lifeExp   gdpPercap
-----  ----------  --------  ----------
 1952  Australia     69.120   10039.596
 1952  Bahrain       50.939    9867.085
 1957  Australia     70.330   10949.650
 1957  Bahrain       53.832   11635.799
 1962  Australia     70.930   12217.227
 1962  Bahrain       56.923   12753.275
 1967  Australia     71.100   14526.125
 1967  Bahrain       59.923   14804.673
 1972  Australia     71.930   16788.629
 1972  Bahrain       63.300   18268.658
 1977  Australia     73.490   18334.198
 1977  Bahrain       65.593   19340.102
 1982  Australia     74.740   19477.009
 1982  Bahrain       69.052   19211.147
 1987  Australia     76.320   21888.889
 1987  Bahrain       70.750   18524.024
 1992  Australia     77.560   23424.767
 1992  Bahrain       72.601   19035.579
 1997  Australia     78.830   26997.937
 1997  Bahrain       73.925   20292.017
 2002  Australia     80.370   30687.755
 2002  Bahrain       74.795   23403.559
 2007  Australia     81.235   34435.367
 2007  Bahrain       75.635   29796.048

```r
## .value separates values for the measures of lifeExp and gdpPerCap
```

# Excercise 3.0 Table Joins:

### Loading the guest and email data

```r
guest <- read_csv("https://raw.githubusercontent.com/STAT545-UBC/Classroom/master/data/wedding/attend.csv")
```

```
## Parsed with column specification:
## cols(
##   party = col_double(),
##   name = col_character(),
##   meal_wedding = col_character(),
##   meal_brunch = col_character(),
##   attendance_wedding = col_character(),
##   attendance_brunch = col_character(),
##   attendance_golf = col_character()
## )
```

```r
email <- read_csv("https://raw.githubusercontent.com/STAT545-UBC/Classroom/master/data/wedding/emails.csv")
```

```
## Parsed with column specification:
## cols(
##   guest = col_character(),
##   email = col_character()
## )
```
###  3.01 First Edit email csv:
Each guest should on one line for each email and guest field changed to name to make it easier to join


```r
Email <- email %>%
  separate_rows(email,guest, sep = ",") %>%
  select(name=guest,email) %>%
 mutate(name=trimws(name, "both"))

## There is a white space in Email dataset, removed this 
```

## 3.1 Add Emails for Each Guest on List:

Assumption: Only including guests from guest list who have a corresponding email from the email tibble: 
 (no additonal emails, no additional guests):

```r
guest_emails <- Email %>%
  inner_join(guest,Email, by="name")

guest_emails %>% DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-138a496808dc4c873bf8" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-138a496808dc4c873bf8">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25"],["Sommer Medrano","Phillip Medrano","Blanka Medrano","Emaan Medrano","Blair Park","Nigel Webb","Sinead English","Ayra Marks","Jolene Welsh","Hayley Booker","Amayah Sanford","Erika Foley","Ciaron Acosta","Diana Stuart","Daisy-May Caldwell","Martin Caldwell","Violet Caldwell","Nazifa Caldwell","Eric Caldwell","Rosanna Bird","Kurtis Frost","Huma Stokes","Samuel Rutledge","Eddison Collier","Stewart Nicholls"],["sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","bpark@gmail.com","bpark@gmail.com","singlish@hotmail.ca","marksa42@gmail.com","jw1987@hotmail.com","jw1987@hotmail.com","erikaaaaaa@gmail.com","erikaaaaaa@gmail.com","shining_ciaron@gmail.com","doodledianastu@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","rosy1987b@gmail.com","rosy1987b@gmail.com","humastokes@gmail.com","humastokes@gmail.com","eddison.collier@gmail.com","eddison.collier@gmail.com"],[1,1,1,1,2,2,3,4,6,6,7,7,8,9,12,12,12,12,12,13,13,14,14,15,15],["PENDING","vegetarian","chicken","PENDING","chicken",null,"PENDING","vegetarian",null,"vegetarian",null,"PENDING","PENDING","vegetarian","chicken","PENDING","PENDING","chicken","chicken","vegetarian","PENDING",null,"chicken","PENDING","chicken"],["PENDING","Menu C","Menu A","PENDING","Menu C",null,"PENDING","Menu B",null,"Menu C","PENDING","PENDING","Menu A","Menu C","Menu B","PENDING","PENDING","PENDING","Menu B","Menu C","PENDING",null,"Menu C","PENDING","Menu B"],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","CANCELLED","CONFIRMED","CANCELLED","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED"],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED"],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>name<\/th>\n      <th>email<\/th>\n      <th>party<\/th>\n      <th>meal_wedding<\/th>\n      <th>meal_brunch<\/th>\n      <th>attendance_wedding<\/th>\n      <th>attendance_brunch<\/th>\n      <th>attendance_golf<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":3},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 3.2 Emails but not on Guest List

The names of peoples with emails but are not on the guest list:


```r
guest_emails %>%
  select(name,email) %>%
  setdiff(Email,.) %>%
  knitr::kable()
```



name              email                           
----------------  --------------------------------
Turner Jones      tjjones12@hotmail.ca            
Albert Marshall   themarshallfamily1234@gmail.com 
Vivian Marshall   themarshallfamily1234@gmail.com 

These 3 are not found in the guest dataset. 

## 3.3 Guest List for all those that we have emails for:

A new guest list with names of people on the original guest list with emails and those not on the original guest list who also have emails (found on email dataset but not guest dataset)


```r
Email %>%
  left_join(guest,Email, by="name") %>%
DT::datatable() 
```

<!--html_preserve--><div id="htmlwidget-7793a958a256477c997d" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-7793a958a256477c997d">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28"],["Sommer Medrano","Phillip Medrano","Blanka Medrano","Emaan Medrano","Blair Park","Nigel Webb","Sinead English","Ayra Marks","Jolene Welsh","Hayley Booker","Amayah Sanford","Erika Foley","Ciaron Acosta","Diana Stuart","Daisy-May Caldwell","Martin Caldwell","Violet Caldwell","Nazifa Caldwell","Eric Caldwell","Rosanna Bird","Kurtis Frost","Huma Stokes","Samuel Rutledge","Eddison Collier","Stewart Nicholls","Turner Jones","Albert Marshall","Vivian Marshall"],["sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","bpark@gmail.com","bpark@gmail.com","singlish@hotmail.ca","marksa42@gmail.com","jw1987@hotmail.com","jw1987@hotmail.com","erikaaaaaa@gmail.com","erikaaaaaa@gmail.com","shining_ciaron@gmail.com","doodledianastu@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","rosy1987b@gmail.com","rosy1987b@gmail.com","humastokes@gmail.com","humastokes@gmail.com","eddison.collier@gmail.com","eddison.collier@gmail.com","tjjones12@hotmail.ca","themarshallfamily1234@gmail.com","themarshallfamily1234@gmail.com"],[1,1,1,1,2,2,3,4,6,6,7,7,8,9,12,12,12,12,12,13,13,14,14,15,15,null,null,null],["PENDING","vegetarian","chicken","PENDING","chicken",null,"PENDING","vegetarian",null,"vegetarian",null,"PENDING","PENDING","vegetarian","chicken","PENDING","PENDING","chicken","chicken","vegetarian","PENDING",null,"chicken","PENDING","chicken",null,null,null],["PENDING","Menu C","Menu A","PENDING","Menu C",null,"PENDING","Menu B",null,"Menu C","PENDING","PENDING","Menu A","Menu C","Menu B","PENDING","PENDING","PENDING","Menu B","Menu C","PENDING",null,"Menu C","PENDING","Menu B",null,null,null],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","CANCELLED","CONFIRMED","CANCELLED","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED",null,null,null],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED",null,null,null],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED",null,null,null]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>name<\/th>\n      <th>email<\/th>\n      <th>party<\/th>\n      <th>meal_wedding<\/th>\n      <th>meal_brunch<\/th>\n      <th>attendance_wedding<\/th>\n      <th>attendance_brunch<\/th>\n      <th>attendance_golf<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":3},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

 

### 3--Extra Analysis:
There are 5 guests who don't have emails in the email dataset.  These are the ones that don't have a corresponding email in the email tibble but are found on the guest list (guest dataset)

Let's find out who the five guests on the guest list that don't have emails are : 

```r
guest_emails%>%
  select(-email) %>%
  setdiff(guest,.) %>%
  knitr::kable()
```



name                party  meal_wedding   meal_brunch   attendance_wedding   attendance_brunch   attendance_golf 
-----------------  ------  -------------  ------------  -------------------  ------------------  ----------------
Atlanta Connolly        5  PENDING        PENDING       PENDING              PENDING             PENDING         
Denzel Connolly         5  fish           Menu B        CONFIRMED            CONFIRMED           CONFIRMED       
Chanelle Shah           5  chicken        Menu C        CONFIRMED            CONFIRMED           CONFIRMED       
Cosmo Dunkley          10  PENDING        PENDING       PENDING              PENDING             PENDING         
Cai Mcdaniel           11  fish           Menu C        CONFIRMED            CONFIRMED           CONFIRMED       


