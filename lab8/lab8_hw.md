---
title: "Lab 8 Homework"
author: "Please Add Your Name Here"
date: "2022-02-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <-readr:: read_csv("data/sydneybeaches.csv") %>%
  clean_names()
```

```
## Rows: 3690 Columns: 8
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(sydneybeaches);
```

```
## Rows: 3,690
## Columns: 8
## $ beach_id              <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, ~
## $ region                <chr> "Sydney City Ocean Beaches", "Sydney City Ocean ~
## $ council               <chr> "Randwick Council", "Randwick Council", "Randwic~
## $ site                  <chr> "Clovelly Beach", "Clovelly Beach", "Clovelly Be~
## $ longitude             <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.2675~
## $ latitude              <dbl> -33.91449, -33.91449, -33.91449, -33.91449, -33.~
## $ date                  <chr> "02/01/2013", "06/01/2013", "12/01/2013", "18/01~
## $ enterococci_cfu_100ml <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8, 3,~
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at C:/Users/Buzzer/Desktop/BIS15W2022_rpineda
```

The quotes show the folder structure from the root directory.

```r
#sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

This data is tidy because it fulfills all the definitions of what tidy data is. In the data frame each variable has its own column, each observation has its own rowm and each value has its own cell. The data is in a long format.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- sydneybeaches %>%
  select(site, date, enterococci_cfu_100ml)
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`


```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(names_from = (date),
              values_from = (enterococci_cfu_100ml))
```


5. Pivot the data back so that the dates are data and not column names.


```r
sydneybeaches_longagain <- sydneybeaches_wide %>%
  pivot_longer(-site,
              names_to = "date" ,
              values_to = "enterococci_cfu_100ml")
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.


```r
sydneybeaches_long_datechange <- sydneybeaches_long %>%
  separate(date, into = c("day","month","year"), sep = "/")
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.


```r
#use long with the date change because already variable for year to group with.

sydneybeaches_long_datechange %>%
  group_by((site), (year)) %>%
  filter(!is.na(enterococci_cfu_100ml)) %>%
  summarize(mean = mean(enterococci_cfu_100ml))
```

```
## `summarise()` has grouped output by '(site)'. You can override using the `.groups` argument.
```

```
## # A tibble: 66 x 3
## # Groups:   (site) [11]
##    `(site)`     `(year)`  mean
##    <chr>        <chr>    <dbl>
##  1 Bondi Beach  2013      32.2
##  2 Bondi Beach  2014      11.1
##  3 Bondi Beach  2015      14.3
##  4 Bondi Beach  2016      19.4
##  5 Bondi Beach  2017      13.2
##  6 Bondi Beach  2018      22.9
##  7 Bronte Beach 2013      26.8
##  8 Bronte Beach 2014      17.5
##  9 Bronte Beach 2015      23.6
## 10 Bronte Beach 2016      61.3
## # ... with 56 more rows
```



8. Make the output from question 7 easier to read by pivoting it to wide format.



```r
sydneybeaches_long_datechange %>%
  group_by(site, year) %>%
  filter(!is.na(enterococci_cfu_100ml)) %>%
  summarize(mean = mean(enterococci_cfu_100ml)) %>%
  pivot_wider(names_from = year,
              values_from = mean)
```

```
## `summarise()` has grouped output by 'site'. You can override using the `.groups` argument.
```

```
## # A tibble: 11 x 7
## # Groups:   site [11]
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2   22.9 
##  2 Bronte Beach             26.8   17.5   23.6    61.3  16.8   43.4 
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93  10.6 
##  4 Coogee Beach             39.7   52.6   40.3    59.5  20.7   21.6 
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7   17.6 
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2   59.1 
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8   38.0 
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6    9.21
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26  12.5 
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9  112.  
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4   15.5
```

9. What was the most polluted beach in 2018?


```r
sydneybeaches_long_datechange %>%
  group_by(site, year) %>%
  filter(!is.na(enterococci_cfu_100ml)) %>%
  filter(year == 2018) %>%
  summarize(mean = mean(enterococci_cfu_100ml)) %>%
  pivot_wider(names_from = year,
              values_from = mean) %>%
  arrange(desc(`2018`))
```

```
## `summarise()` has grouped output by 'site'. You can override using the `.groups` argument.
```

```
## # A tibble: 11 x 2
## # Groups:   site [11]
##    site                    `2018`
##    <chr>                    <dbl>
##  1 South Maroubra Rockpool 112.  
##  2 Little Bay Beach         59.1 
##  3 Bronte Beach             43.4 
##  4 Malabar Beach            38.0 
##  5 Bondi Beach              22.9 
##  6 Coogee Beach             21.6 
##  7 Gordons Bay (East)       17.6 
##  8 Tamarama Beach           15.5 
##  9 South Maroubra Beach     12.5 
## 10 Clovelly Beach           10.6 
## 11 Maroubra Beach            9.21
```


10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
