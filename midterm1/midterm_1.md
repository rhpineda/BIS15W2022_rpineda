---
title: "Midterm 1"
author: "Ricardo Pineda"
date: "2022-01-27"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(skimr)
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

We have used algorithms in order to extract insights from noisy structured data. An algorithm is defined as "a process or set of rules to be followed in calculations or other problem-solving operations, especially by a computer."
An example of an algorithm we have used can be seen in lab 6 HW. I used an algorithm of applying certain functions, in order to answer the questions from the structured fisheries data.

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

The most helpful thing I have learned was how the environment and the general set up of RStudio works. I used some of the knowledge I gained in this course in BIS 20Q which deals with
MATLAB. So the environment, rmd scripts, and the console are all things that I have a solid grasp on in both classes. I need to practice is not relying too much on past homework or labs to complete new homework.
I know its necessary to go back sometimes, but I feel like I struggle without any references because I always forget what functions are relevant to a problem.

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.


```r
elephants <- readr::read_csv(file = "data/ElephantsMF.csv");
```

```
## Rows: 288 Columns: 3
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(elephants);
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1~
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,~
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"~
```

```r
elephants;
```

```
## # A tibble: 288 x 3
##      Age Height Sex  
##    <dbl>  <dbl> <chr>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # ... with 278 more rows
```

```r
#Age <dbl>
#Height <dbl>
#sex <chr>
```


4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.


```r
elephants <- clean_names(elephants) %>%
mutate(sex = as.factor(sex));
elephants;
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # ... with 278 more rows
```

```r
#Age <dbl>
#Height <dbl>
#sex <fctr>
```


5. (2 points) How many male and female elephants are represented in the data?


```r
elephants %>%
  group_by(sex) %>%
  count(sex);
```

```
## # A tibble: 2 x 2
## # Groups:   sex [2]
##   sex       n
##   <fct> <int>
## 1 F       150
## 2 M       138
```


6. (2 points) What is the average age all elephants in the data?


```r
elephants %>%
  summarise(overall_age_average = mean(age));
```

```
## # A tibble: 1 x 1
##   overall_age_average
##                 <dbl>
## 1                11.0
```


7. (2 points) How does the average age and height of elephants compare by sex?


```r
elephants %>%
  group_by(sex) %>%
  summarise(overall_age_average = mean(age))
```

```
## # A tibble: 2 x 2
##   sex   overall_age_average
##   <fct>               <dbl>
## 1 F                   12.8 
## 2 M                    8.95
```

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  


```r
elephants %>%
  filter(age > 20) %>%
  group_by(sex) %>%
  summarise(overall_age_average_for_over_20 = mean(age),
            min_height = min(height),
            max_height = max(height),
            total=n())
```

```
## # A tibble: 2 x 5
##   sex   overall_age_average_for_over_20 min_height max_height total
##   <fct>                           <dbl>      <dbl>      <dbl> <int>
## 1 F                                25.6       193.       278.    37
## 2 M                                25.2       229.       304.    13
```


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.


```r
#Loading and overall structure
gabon <- readr::read_csv(file = "data/IvindoData_DryadVersion.csv");
```

```
## Rows: 24 Columns: 26
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(gabon);
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, ~
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06~
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "N~
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56~
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "Pa~
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7~
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2~
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8~
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2~
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00~
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71~
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1~
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6~
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43~
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2~
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26~
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8~
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22~
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81~
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56~
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1~
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80~
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92~
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, ~
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77~
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92~
```


```r
#Changing variables
gabon <- clean_names(gabon) %>%
  mutate(hunt_cat = as.factor(hunt_cat)) %>%
  mutate(land_use = as.factor(land_use));
gabon;
```

```
## # A tibble: 24 x 26
##    transect_id distance hunt_cat num_households land_use veg_rich veg_stems
##          <dbl>    <dbl> <fct>             <dbl> <fct>       <dbl>     <dbl>
##  1           1     7.14 Moderate             54 Park         16.7      31.2
##  2           2    17.3  None                 54 Park         15.8      37.4
##  3           2    18.3  None                 29 Park         16.9      32.3
##  4           3    20.8  None                 29 Logging      12.4      29.4
##  5           4    16.0  None                 29 Park         17.1      36  
##  6           5    17.5  None                 29 Park         16.5      29.2
##  7           6    24.1  None                 29 Park         14.8      31.2
##  8           7    19.8  None                 54 Logging      13.2      32.6
##  9           8     5.78 High                 25 Neither      12.6      23.7
## 10           9     5.13 High                 73 Logging      16        27.1
## # ... with 14 more rows, and 19 more variables: veg_liana <dbl>, veg_dbh <dbl>,
## #   veg_canopy <dbl>, veg_understory <dbl>, ra_apes <dbl>, ra_birds <dbl>,
## #   ra_elephant <dbl>, ra_monkeys <dbl>, ra_rodent <dbl>, ra_ungulate <dbl>,
## #   rich_all_species <dbl>, evenness_all_species <dbl>,
## #   diversity_all_species <dbl>, rich_bird_species <dbl>,
## #   evenness_bird_species <dbl>, diversity_bird_species <dbl>,
## #   rich_mammal_species <dbl>, evenness_mammal_species <dbl>, ...
```

10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?


```r
gabon %>%
  filter(hunt_cat == "High" | hunt_cat == "Moderate") %>%
  group_by(hunt_cat) %>%
  summarize(average_bird_diversity = mean(diversity_bird_species),
            average_mammal_diversity = mean(diversity_mammal_species))
```

```
## # A tibble: 2 x 3
##   hunt_cat average_bird_diversity average_mammal_diversity
##   <fct>                     <dbl>                    <dbl>
## 1 High                       1.66                     1.74
## 2 Moderate                   1.62                     1.68
```


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  


```r
gabon %>%
  filter(distance > 25 | distance < 3) %>%
  group_by(distance) %>%
  summarize(across(c(ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate)))  
```

```
## # A tibble: 3 x 7
##   distance ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##      <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1     2.7     0        85.0        0.29       9.09      3.74        1.86
## 2     2.92    0.24     68.2        0         25.6       4.05        1.88
## 3    26.8     4.91     31.6        0         54.1       1.29        8.12
```

```r
#Apes: Fits conclusion
#Birds: Does not fit conclusion
#Elephant: Does not fit conclusion
#Monkey: Fits conclusion
#Rodent: Does not fit conclusion
#Ungulate: Fits conclusion
```


12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`


```r
#What land use has the most vegatation?
gabon %>%
  group_by(land_use) %>%
  summarize(across(contains("veg"), mean)) %>%
  arrange(desc(veg_rich))
```

```
## # A tibble: 3 x 7
##   land_use veg_rich veg_stems veg_liana veg_dbh veg_canopy veg_understory
##   <fct>       <dbl>     <dbl>     <dbl>   <dbl>      <dbl>          <dbl>
## 1 Park         16.3      33.5      9.76    44         3.60           3.04
## 2 Logging      14.4      33.5     11.6     45.4       3.50           3.00
## 3 Neither      13.5      29.1     11.5     52.0       3.13           3.04
```

