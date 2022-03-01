---
title: "Lab 13 Homework"
author: "Ricardo Pineda"
date: "2022-03-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
library(tidyverse)
library(naniar)
library(shiny)
library(shinydashboard)
library(janitor)
```

## Choose Your Adventure!
For this homework assignment, you have two choices of data. You only need to build an app for one of them. The first dataset is focused on UC Admissions and the second build on the Gabon data that we used for midterm 1.  

## Option 1
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

**1. Load the `UC_admit.csv` data and use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  


```r
uc_admit <- readr::read_csv("data/uc_data/UC_admit.csv")
```

```
## Rows: 2160 Columns: 6
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
uc_admit <- clean_names(uc_admit)
glimpse(uc_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ campus            <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis"~
## $ academic_yr       <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018~
## $ category          <chr> "Applicants", "Applicants", "Applicants", "Applicant~
## $ ethnicity         <chr> "International", "Unknown", "White", "Asian", "Chica~
## $ perc_fr           <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.~
## $ filtered_count_fr <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, ~
```

```r
tibble(uc_admit)
```

```
## # A tibble: 2,160 x 6
##    campus academic_yr category   ethnicity        perc_fr filtered_count_fr
##    <chr>        <dbl> <chr>      <chr>            <chr>               <dbl>
##  1 Davis         2019 Applicants International    21.16%              16522
##  2 Davis         2019 Applicants Unknown          2.51%                1959
##  3 Davis         2019 Applicants White            18.39%              14360
##  4 Davis         2019 Applicants Asian            30.76%              24024
##  5 Davis         2019 Applicants Chicano/Latino   22.44%              17526
##  6 Davis         2019 Applicants American Indian  0.35%                 277
##  7 Davis         2019 Applicants African American 4.39%                3425
##  8 Davis         2019 Applicants All              100.00%             78093
##  9 Davis         2018 Applicants International    19.87%              15507
## 10 Davis         2018 Applicants Unknown          2.83%                2208
## # ... with 2,150 more rows
```

```r
naniar::miss_var_summary(uc_admit)
```

```
## # A tibble: 6 x 3
##   variable          n_miss pct_miss
##   <chr>              <int>    <dbl>
## 1 perc_fr                1   0.0463
## 2 filtered_count_fr      1   0.0463
## 3 campus                 0   0     
## 4 academic_yr            0   0     
## 5 category               0   0     
## 6 ethnicity              0   0
```

```r
tibble(uc_admit)
```

```
## # A tibble: 2,160 x 6
##    campus academic_yr category   ethnicity        perc_fr filtered_count_fr
##    <chr>        <dbl> <chr>      <chr>            <chr>               <dbl>
##  1 Davis         2019 Applicants International    21.16%              16522
##  2 Davis         2019 Applicants Unknown          2.51%                1959
##  3 Davis         2019 Applicants White            18.39%              14360
##  4 Davis         2019 Applicants Asian            30.76%              24024
##  5 Davis         2019 Applicants Chicano/Latino   22.44%              17526
##  6 Davis         2019 Applicants American Indian  0.35%                 277
##  7 Davis         2019 Applicants African American 4.39%                3425
##  8 Davis         2019 Applicants All              100.00%             78093
##  9 Davis         2018 Applicants International    19.87%              15507
## 10 Davis         2018 Applicants Unknown          2.83%                2208
## # ... with 2,150 more rows
```

```r
#NAs as normal NA
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
uc_admit %>%
  count(academic_yr)
```

```
## # A tibble: 10 x 2
##    academic_yr     n
##          <dbl> <int>
##  1        2010   216
##  2        2011   216
##  3        2012   216
##  4        2013   216
##  5        2014   216
##  6        2015   216
##  7        2016   216
##  8        2017   216
##  9        2018   216
## 10        2019   216
```

```r
uc_admit %>%
  count(campus)
```

```
## # A tibble: 9 x 2
##   campus            n
##   <chr>         <int>
## 1 Berkeley        240
## 2 Davis           240
## 3 Irvine          240
## 4 Los_Angeles     240
## 5 Merced          240
## 6 Riverside       240
## 7 San_Diego       240
## 8 Santa_Barbara   240
## 9 Santa_Cruz      240
```

```r
uc_admit %>%
  count(category)
```

```
## # A tibble: 3 x 2
##   category       n
##   <chr>      <int>
## 1 Admits       720
## 2 Applicants   720
## 3 Enrollees    720
```

```r
uc_admit %>%
  count(ethnicity)
```

```
## # A tibble: 8 x 2
##   ethnicity            n
##   <chr>            <int>
## 1 African American   270
## 2 All                270
## 3 American Indian    270
## 4 Asian              270
## 5 Chicano/Latino     270
## 6 International      270
## 7 Unknown            270
## 8 White              270
```



```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admission by Ethnicity"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 5,
 radioButtons("a", "Select Year", choices = c("2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019"), 
              selected = "2010"),
  selectInput("b", "Select Campus", choices = c("Berkeley", "Davis", "Irvine", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"),
              selected = "Davis"),
  selectInput("c", "Select Admit Category", choices = c("Admits", "Applications", "Enrollees"),
              selected = "Admits")
  ), # close the first box
  
  box(title = "UC Admissions", width = 7,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  output$plot <- renderPlot({
  uc_admit %>% 
      filter(academic_yr == input$a & campus == input$b & category == input$c) %>% 
      ggplot(aes(x=reorder(ethnicity, filtered_count_fr), y=filtered_count_fr)) +
      geom_col() +
       labs(x = "Ethnicity", y = "n")
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}



**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**  


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Enrollment by Year and Ethnicity"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 5,
 radioButtons("a", "Select Ethnicity", choices = c("African American", "American Indian", "Asian", "Chicano/Latino", "White", "International", "Unknown", "All"), 
              selected = "All"),
  selectInput("b", "Select Campus", choices = c("Berkeley", "Davis", "Irvine", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"),
              selected = "Davis"),
  selectInput("c", "Select Admit Category", choices = c("Admits", "Applications", "Enrollees"),
              selected = "Admits")
  ), # close the first box
  
  box(title = "UC Admissions", width = 7,
  plotOutput("plot", width = "500px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  output$plot <- renderPlot({
  uc_admit %>% 
      filter(ethnicity== input$a & campus == input$b & category == input$c) %>% 
      ggplot(aes(x= academic_yr, y=filtered_count_fr)) +
      geom_col() +
       labs(x = "Year", y = "n Enrolled")
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
