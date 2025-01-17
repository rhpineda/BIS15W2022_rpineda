---
title: "Lab 6 Homework"
author: "Joel Ledford"
date: "2022-01-25"
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
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
glimpse(fisheries);
```

```
## Rows: 17,692
## Columns: 71
## $ Country                   <chr> "Albania", "Albania", "Albania", "Albania", ~
## $ `Common name`             <chr> "Angelsharks, sand devils nei", "Atlantic bo~
## $ `ISSCAAP group#`          <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, ~
## $ `ISSCAAP taxonomic group` <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, ~
## $ `ASFIS species#`          <chr> "10903XXXXX", "1750100101", "17710001XX", "2~
## $ `ASFIS species name`      <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp~
## $ `FAO major fishing area`  <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, ~
## $ Measure                   <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Q~
## $ `1950`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1951`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1952`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1953`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1954`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1955`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1956`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1957`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1958`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1959`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1960`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1961`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1962`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1963`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1964`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1965`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1966`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1967`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1968`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1969`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1970`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1971`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1972`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1973`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1974`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1975`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1976`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1977`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1978`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1979`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1980`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1981`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1982`                    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ~
## $ `1983`                    <chr> NA, NA, NA, NA, NA, NA, "559", NA, NA, NA, N~
## $ `1984`                    <chr> NA, NA, NA, NA, NA, NA, "392", NA, NA, NA, N~
## $ `1985`                    <chr> NA, NA, NA, NA, NA, NA, "406", NA, NA, NA, N~
## $ `1986`                    <chr> NA, NA, NA, NA, NA, NA, "499", NA, NA, NA, N~
## $ `1987`                    <chr> NA, NA, NA, NA, NA, NA, "564", NA, NA, NA, N~
## $ `1988`                    <chr> NA, NA, NA, NA, NA, NA, "724", NA, NA, NA, N~
## $ `1989`                    <chr> NA, NA, NA, NA, NA, NA, "583", NA, NA, NA, N~
## $ `1990`                    <chr> NA, NA, NA, NA, NA, NA, "754", NA, NA, NA, N~
## $ `1991`                    <chr> NA, NA, NA, NA, NA, NA, "283", NA, NA, NA, N~
## $ `1992`                    <chr> NA, NA, NA, NA, NA, NA, "196", NA, NA, NA, N~
## $ `1993`                    <chr> NA, NA, NA, NA, NA, NA, "150 F", NA, NA, NA,~
## $ `1994`                    <chr> NA, NA, NA, NA, NA, NA, "100 F", NA, NA, NA,~
## $ `1995`                    <chr> "0 0", "1", NA, "0 0", "0 0", NA, "52", "30"~
## $ `1996`                    <chr> "53", "2", NA, "3", "2", NA, "104", "8", NA,~
## $ `1997`                    <chr> "20", "0 0", NA, "0 0", "0 0", NA, "65", "4"~
## $ `1998`                    <chr> "31", "12", NA, NA, NA, NA, "220", "18", NA,~
## $ `1999`                    <chr> "30", "30", NA, NA, NA, NA, "220", "18", NA,~
## $ `2000`                    <chr> "30", "25", "2", NA, NA, NA, "220", "20", NA~
## $ `2001`                    <chr> "16", "30", NA, NA, NA, NA, "120", "23", NA,~
## $ `2002`                    <chr> "79", "24", NA, "34", "6", NA, "150", "84", ~
## $ `2003`                    <chr> "1", "4", NA, "22", NA, NA, "84", "178", NA,~
## $ `2004`                    <chr> "4", "2", "2", "15", "1", "2", "76", "285", ~
## $ `2005`                    <chr> "68", "23", "4", "12", "5", "6", "68", "150"~
## $ `2006`                    <chr> "55", "30", "7", "18", "8", "9", "86", "102"~
## $ `2007`                    <chr> "12", "19", NA, NA, NA, NA, "132", "18", NA,~
## $ `2008`                    <chr> "23", "27", NA, NA, NA, NA, "132", "23", NA,~
## $ `2009`                    <chr> "14", "21", NA, NA, NA, NA, "154", "20", NA,~
## $ `2010`                    <chr> "78", "23", "7", NA, NA, NA, "80", "228", NA~
## $ `2011`                    <chr> "12", "12", NA, NA, NA, NA, "88", "9", NA, "~
## $ `2012`                    <chr> "5", "5", NA, NA, NA, NA, "129", "290", NA, ~
```

```r
names(fisheries);
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

```r
#Variable names? Country, Common Name, ISSCAAP group#, etc.
#Dimensions 17692 rows x 71 columns
#Yes there are NA's
#Classes of varaibles can be seen between the carrots from the output of the
#glimpse function: <chr> and <dbl>
```
2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- clean_names(fisheries)
fisheries
```

```
## # A tibble: 17,692 x 71
##    country common_name    isscaap_group_nu~ isscaap_taxonomic_~ asfis_species_n~
##    <chr>   <chr>                      <dbl> <chr>               <chr>           
##  1 Albania Angelsharks, ~                38 Sharks, rays, chim~ 10903XXXXX      
##  2 Albania Atlantic boni~                36 Tunas, bonitos, bi~ 1750100101      
##  3 Albania Barracudas nei                37 Miscellaneous pela~ 17710001XX      
##  4 Albania Blue and red ~                45 Shrimps, prawns     2280203101      
##  5 Albania Blue whiting(~                32 Cods, hakes, haddo~ 1480403301      
##  6 Albania Bluefish                      37 Miscellaneous pela~ 1702021301      
##  7 Albania Bogue                         33 Miscellaneous coas~ 1703926101      
##  8 Albania Caramote prawn                45 Shrimps, prawns     2280100117      
##  9 Albania Catsharks, nu~                38 Sharks, rays, chim~ 10801003XX      
## 10 Albania Common cuttle~                57 Squids, cuttlefish~ 3210200202      
## # ... with 17,682 more rows, and 66 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <dbl>, measure <chr>, x1950 <chr>, x1951 <chr>,
## #   x1952 <chr>, x1953 <chr>, x1954 <chr>, x1955 <chr>, x1956 <chr>,
## #   x1957 <chr>, x1958 <chr>, x1959 <chr>, x1960 <chr>, x1961 <chr>,
## #   x1962 <chr>, x1963 <chr>, x1964 <chr>, x1965 <chr>, x1966 <chr>,
## #   x1967 <chr>, x1968 <chr>, x1969 <chr>, x1970 <chr>, x1971 <chr>,
## #   x1972 <chr>, x1973 <chr>, x1974 <chr>, x1975 <chr>, x1976 <chr>, ...
```

```r
#mutate(country = as.factor)
#fisheries$country <- as.factor(fisheries$country)
#fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
#fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
#fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)

fisheries <- fisheries %>%
mutate(country = as.factor(country)) %>%
mutate(isscaap_group_number = as.factor(isscaap_group_number)) %>%
mutate(asfis_species_number = as.factor(asfis_species_number)) %>%
mutate(fao_major_fishing_area = as.factor(fao_major_fishing_area))

fisheries;
```

```
## # A tibble: 17,692 x 71
##    country common_name    isscaap_group_nu~ isscaap_taxonomic_~ asfis_species_n~
##    <fct>   <chr>          <fct>             <chr>               <fct>           
##  1 Albania Angelsharks, ~ 38                Sharks, rays, chim~ 10903XXXXX      
##  2 Albania Atlantic boni~ 36                Tunas, bonitos, bi~ 1750100101      
##  3 Albania Barracudas nei 37                Miscellaneous pela~ 17710001XX      
##  4 Albania Blue and red ~ 45                Shrimps, prawns     2280203101      
##  5 Albania Blue whiting(~ 32                Cods, hakes, haddo~ 1480403301      
##  6 Albania Bluefish       37                Miscellaneous pela~ 1702021301      
##  7 Albania Bogue          33                Miscellaneous coas~ 1703926101      
##  8 Albania Caramote prawn 45                Shrimps, prawns     2280100117      
##  9 Albania Catsharks, nu~ 38                Sharks, rays, chim~ 10801003XX      
## 10 Albania Common cuttle~ 57                Squids, cuttlefish~ 3210200202      
## # ... with 17,682 more rows, and 66 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <fct>, measure <chr>, x1950 <chr>, x1951 <chr>,
## #   x1952 <chr>, x1953 <chr>, x1954 <chr>, x1955 <chr>, x1956 <chr>,
## #   x1957 <chr>, x1958 <chr>, x1959 <chr>, x1960 <chr>, x1961 <chr>,
## #   x1962 <chr>, x1963 <chr>, x1964 <chr>, x1965 <chr>, x1966 <chr>,
## #   x1967 <chr>, x1968 <chr>, x1969 <chr>, x1970 <chr>, x1971 <chr>,
## #   x1972 <chr>, x1973 <chr>, x1974 <chr>, x1975 <chr>, x1976 <chr>, ...
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries_tidy %>% 
count(country)
```

```
## # A tibble: 203 x 2
##    country                 n
##    <fct>               <int>
##  1 Albania               934
##  2 Algeria              1561
##  3 American Samoa        556
##  4 Angola               2119
##  5 Anguilla              129
##  6 Antigua and Barbuda   356
##  7 Argentina            3403
##  8 Aruba                 172
##  9 Australia            8183
## 10 Bahamas               423
## # ... with 193 more rows
```

```r
#203 Countries represented.
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
new_fisheries <- fisheries_tidy %>%
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)

new_fisheries
```

```
## # A tibble: 376,771 x 6
##    country isscaap_taxonomic_group asfis_species_n~ asfis_species_n~  year catch
##    <fct>   <chr>                   <chr>            <fct>            <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2004     4
## # ... with 376,761 more rows
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
new_fisheries %>%
  summarize(distinct_species = n_distinct(asfis_species_number))
```

```
## # A tibble: 1 x 1
##   distinct_species
##              <int>
## 1             1551
```

```r
#1551
```

6. Which country had the largest overall catch in the year 2000?

```r
new_fisheries %>%
  filter(year == "2000") %>%
  filter(!is.na(catch)) %>%
  group_by(country) %>%
  summarize(max_catch = max(catch)) %>%
  arrange(desc(max_catch))
```

```
## # A tibble: 187 x 2
##    country                  max_catch
##    <fct>                        <dbl>
##  1 China                         9068
##  2 Peru                          5717
##  3 Russian Federation            5065
##  4 Viet Nam                      4945
##  5 Chile                         4299
##  6 United States of America      2438
##  7 Philippines                    999
##  8 Japan                          988
##  9 Bangladesh                     977
## 10 Senegal                        970
## # ... with 177 more rows
```

```r
#China.
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
#new_fisheries %>%
#  filter(between(year, 1990,2000)) %>%
#  filter(!is.na(catch)) %>%
#  filter(asfis_species_name == "Sardina pilchardus") %>%
#  group_by(country) %>%
#  summarize(max_catch = max(catch)) %>%
#  arrange(desc(max_catch))
#I originally thought the question was asking most catches in a given year.

new_fisheries %>%
  filter(between(year, 1990,2000)) %>%
  filter(!is.na(catch)) %>%
  filter(asfis_species_name == "Sardina pilchardus") %>%
  group_by(country) %>%
  summarize(sum_catch = sum(catch)) %>%
  arrange(desc(sum_catch)) %>%
  head(sum_catch, n = 1)
```

```
## # A tibble: 1 x 2
##   country sum_catch
##   <fct>       <dbl>
## 1 Morocco      7470
```

```r
#Morocco
```

8. Which five countries caught the most cephalopods between 2008-2012?

```r
#new_fisheries %>%
#  filter(between(year, 2008,2012)) %>%
#  filter(!is.na(catch)) %>%
#  filter(asfis_species_name == "Cephalopoda") %>%
#  group_by(country) %>%
#  summarize(max_catch = max(catch)) %>%
#  arrange(desc(max_catch)) %>%
#  head(max_catch, n = 5)
#I originally thought the question was asking most catches in a given year.

new_fisheries %>%
  filter(between(year, 2008,2012)) %>%
  filter(!is.na(catch)) %>%
  filter(asfis_species_name == "Cephalopoda") %>%
  group_by(country) %>%
  summarize(sum_catch = sum(catch)) %>%
  arrange(desc(sum_catch)) %>%
  head(sum_catch, n = 5)
```

```
## # A tibble: 5 x 2
##   country sum_catch
##   <fct>       <dbl>
## 1 India         570
## 2 China         257
## 3 Spain         198
## 4 Algeria       162
## 5 France        101
```

```r
#India, China, Spain, Algeria, France
```

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
new_fisheries %>%
  filter(between(year, 2008,2012)) %>%
  filter(!is.na(catch)) %>%
  filter(asfis_species_name != "Osteichthyes") %>%
  group_by(asfis_species_name) %>%
  summarize(sum_catch = sum(catch)) %>%
  arrange(desc(sum_catch)) %>%
  head(sum_catch, n = 5)
```

```
## # A tibble: 5 x 2
##   asfis_species_name    sum_catch
##   <chr>                     <dbl>
## 1 Theragra chalcogramma     41075
## 2 Engraulis ringens         35523
## 3 Katsuwonus pelamis        32153
## 4 Trichiurus lepturus       30400
## 5 Clupea harengus           28527
```

```r
#T. lepturus.
```

10. Use the data to do at least one analysis of your choice.

```r
#Country with the largest average catch for Engraulis ringens for years when there are at least one catch?

new_fisheries %>%
  filter(!is.na(catch)) %>%
  filter(catch >= 1) %>%
  filter(asfis_species_name == "Engraulis ringens") %>%
  group_by(country) %>%
  summarize(largest_mean_catch = mean(catch)) %>%
  arrange(desc(largest_mean_catch))
```

```
## # A tibble: 3 x 2
##   country largest_mean_catch
##   <fct>                <dbl>
## 1 Peru                7714. 
## 2 Chile               1974. 
## 3 Ecuador               29.8
```

```r
#Peru
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
