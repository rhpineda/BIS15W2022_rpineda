---
title: "dplyr Superhero"
date: "2022-01-20"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Load the tidyverse

```r
library("tidyverse")
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
library("janitor")
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

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
superhero_info <- rename(superhero_info, gender = "Gender", eye_color="Eye color", race= "Race", hair_color="Hair color", height="Height",publisher = "Publisher", skin_color = "Skin color", alignment = "Alignment", weight = "Weight")
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names  Agility `Accelerated Healing` `Lantern Power Ri~ `Dimensional Awa~
##   <chr>       <lgl>   <lgl>                 <lgl>              <lgl>            
## 1 3-D Man     TRUE    FALSE                 FALSE              FALSE            
## 2 A-Bomb      FALSE   TRUE                  FALSE              FALSE            
## 3 Abe Sapien  TRUE    TRUE                  FALSE              FALSE            
## 4 Abin Sur    FALSE   FALSE                 TRUE               FALSE            
## 5 Abomination FALSE   TRUE                  FALSE              FALSE            
## 6 Abraxas     FALSE   FALSE                 FALSE              TRUE             
## # ... with 163 more variables: Cold Resistance <lgl>, Durability <lgl>,
## #   Stealth <lgl>, Energy Absorption <lgl>, Flight <lgl>, Danger Sense <lgl>,
## #   Underwater breathing <lgl>, Marksmanship <lgl>, Weapons Master <lgl>,
## #   Power Augmentation <lgl>, Animal Attributes <lgl>, Longevity <lgl>,
## #   Intelligence <lgl>, Super Strength <lgl>, Cryokinesis <lgl>,
## #   Telepathy <lgl>, Energy Armor <lgl>, Energy Blasts <lgl>,
## #   Duplication <lgl>, Size Changing <lgl>, Density Control <lgl>, ...
```

```r
superhero_powers <- clean_names(superhero_powers);
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  


```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
filter(superhero_info, superhero_info$alignment == "neutral")
```

```
## # A tibble: 24 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Biza~ Male   black     Biza~ Black         191 DC Comics white      neutral  
##  2 Blac~ Male   <NA>      God ~ <NA>           NA DC Comics <NA>       neutral  
##  3 Capt~ Male   brown     Human Brown          NA DC Comics <NA>       neutral  
##  4 Copy~ Female red       Muta~ White         183 Marvel C~ blue       neutral  
##  5 Dead~ Male   brown     Muta~ No Hair       188 Marvel C~ <NA>       neutral  
##  6 Deat~ Male   blue      Human White         193 DC Comics <NA>       neutral  
##  7 Etri~ Male   red       Demon No Hair       193 DC Comics yellow     neutral  
##  8 Gala~ Male   black     Cosm~ Black         876 Marvel C~ <NA>       neutral  
##  9 Glad~ Male   blue      Stro~ Blue          198 Marvel C~ purple     neutral  
## 10 Indi~ Female <NA>      Alien Purple         NA DC Comics <NA>       neutral  
## # ... with 14 more rows, and 1 more variable: weight <dbl>
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>%
  select(name, alignment, race);
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ... with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info %>%
  filter(race != "Human")
```

```
## # A tibble: 222 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  2 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  3 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  4 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  7 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  8 Angel Male   <NA>      Vamp~ <NA>           NA Dark Hor~ <NA>       good     
##  9 Ange~ Female yellow    Muta~ Black         165 Marvel C~ <NA>       good     
## 10 Anti~ Male   yellow    God ~ No Hair        61 DC Comics <NA>       bad      
## # ... with 212 more rows, and 1 more variable: weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
goodguys <- filter(superhero_info, alignment == "good");
```

```r
badguys <- filter(superhero_info, alignment == "bad");
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(goodguys$race);
```

```
##      goodguys$race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
goodguys %>%
  filter(race == "Asgardian")
```

```
## # A tibble: 3 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sif    Female blue      Asga~ Black         188 Marvel C~ <NA>       good     
## 2 Thor   Male   blue      Asga~ Blond         198 Marvel C~ <NA>       good     
## 3 Thor ~ Female blue      Asga~ Blond         175 Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
badguys %>%
  filter(height > 200)
```

```
## # A tibble: 25 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  2 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  3 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  4 Apoc~ Male   red       Muta~ Black         213 Marvel C~ grey       bad      
##  5 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  6 Bloo~ Female blue      Human Brown         218 Marvel C~ <NA>       bad      
##  7 Dark~ Male   red       New ~ No Hair       267 DC Comics grey       bad      
##  8 Doct~ Male   brown     Human Brown         201 Marvel C~ <NA>       bad      
##  9 Doct~ Male   brown     <NA>  Brown         201 Marvel C~ <NA>       bad      
## 10 Doom~ Male   red       Alien White         244 DC Comics <NA>       bad      
## # ... with 15 more rows, and 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
goodguys %>%
  filter(hair_color == "No Hair");
```

```
## # A tibble: 37 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  3 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  4 Beta~ Male   <NA>      <NA>  No Hair       201 Marvel C~ <NA>       good     
##  5 Bish~ Male   brown     Muta~ No Hair       198 Marvel C~ <NA>       good     
##  6 Blac~ Male   brown     <NA>  No Hair       185 DC Comics <NA>       good     
##  7 Blaq~ <NA>   black     <NA>  No Hair        NA Marvel C~ <NA>       good     
##  8 Bloo~ Male   black     Muta~ No Hair        NA Marvel C~ <NA>       good     
##  9 Crim~ Male   brown     <NA>  No Hair       180 Marvel C~ <NA>       good     
## 10 Dona~ Male   green     Muta~ No Hair        NA IDW Publ~ green      good     
## # ... with 27 more rows, and 1 more variable: weight <dbl>
```

```r
badguys %>%
  filter(hair_color == "No Hair");
```

```
## # A tibble: 35 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abom~ Male   green     Huma~ No Hair     203   Marvel C~ <NA>       bad      
##  2 Abso~ Male   blue      Human No Hair     193   Marvel C~ <NA>       bad      
##  3 Alien Male   <NA>      Xeno~ No Hair     244   Dark Hor~ black      bad      
##  4 Anni~ Male   green     <NA>  No Hair     180   Marvel C~ <NA>       bad      
##  5 Anti~ Male   yellow    God ~ No Hair      61   DC Comics <NA>       bad      
##  6 Blac~ Male   black     Human No Hair     188   DC Comics <NA>       bad      
##  7 Bloo~ Male   white     <NA>  No Hair      30.5 Marvel C~ <NA>       bad      
##  8 Brai~ Male   green     Andr~ No Hair     198   DC Comics green      bad      
##  9 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 10 Dart~ Male   yellow    Cybo~ No Hair     198   George L~ <NA>       bad      
## # ... with 25 more rows, and 1 more variable: weight <dbl>
```

```r
#more good guys with no hair
```

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight greater than or equal to 450?

```r
superhero_info %>%
  filter(height > 200 | weight >= 450);
```

```
## # A tibble: 60 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  3 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  4 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  5 Ant-~ Male   blue      Human Blond         211 Marvel C~ <NA>       good     
##  6 Anti~ Male   blue      Symb~ Blond         229 Marvel C~ <NA>       <NA>     
##  7 Apoc~ Male   red       Muta~ Black         213 Marvel C~ grey       bad      
##  8 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
##  9 Beta~ Male   <NA>      <NA>  No Hair       201 Marvel C~ <NA>       good     
## 10 Bloo~ Female blue      Human Brown         218 Marvel C~ <NA>       bad      
## # ... with 50 more rows, and 1 more variable: weight <dbl>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>%
  filter(height > 300);
```

```
## # A tibble: 8 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin F~ Male   red       Kaka~ No Hair      975  Marvel C~ green      good     
## 2 Galac~ Male   black     Cosm~ Black        876  Marvel C~ <NA>       neutral  
## 3 Groot  Male   yellow    Flor~ <NA>         701  Marvel C~ <NA>       good     
## 4 MODOK  Male   white     Cybo~ Brownn       366  Marvel C~ <NA>       bad      
## 5 Onsla~ Male   red       Muta~ No Hair      305  Marvel C~ <NA>       bad      
## 6 Sasqu~ Male   red       <NA>  Orange       305  Marvel C~ <NA>       good     
## 7 Wolfs~ Female green     <NA>  Auburn       366  Marvel C~ <NA>       good     
## 8 Ymir   Male   white     Fros~ No Hair      305. Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>%
  filter(weight >= 450);
```

```
## # A tibble: 8 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Blood~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
## 2 Darks~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 3 Gigan~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
## 4 Hulk   Male   green     Huma~ Green       244   Marvel C~ green      good     
## 5 Jugge~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
## 6 Red H~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 7 Sasqu~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 8 Wolfs~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

```r
#because tall heroes but not heavy superheroes can be included in Q10 while
#they can't be included here. Only one condition needs to be satisfied in Q10.
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
superhero_info %>%
  filter(weight >= 450 & height >300) %>%
  arrange(desc(height,weight))
```

```
## # A tibble: 2 x 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Wolfs~ Female green     <NA>  Auburn        366 Marvel C~ <NA>       good     
## 2 Sasqu~ Male   red       <NA>  Orange        305 Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```

```r
#Wolfsbane with the highest height:weight ratio
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abin ~
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, F~
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, FA~
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, ~
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, T~
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, F~
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, FA~
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TRUE~
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, FAL~
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, FA~
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE, F~
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, ~
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, T~
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, ~
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, ~
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, ~
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, ~
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, ~
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,~
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>%
  filter(accelerated_healing) %>%
  filter(durability) %>%
  filter(super_strength);
```

```
## # A tibble: 97 x 168
##    hero_names   agility accelerated_healing lantern_power_ring dimensional_awar~
##    <chr>        <lgl>   <lgl>               <lgl>              <lgl>            
##  1 A-Bomb       FALSE   TRUE                FALSE              FALSE            
##  2 Abe Sapien   TRUE    TRUE                FALSE              FALSE            
##  3 Angel        TRUE    TRUE                FALSE              FALSE            
##  4 Anti-Monitor FALSE   TRUE                FALSE              TRUE             
##  5 Anti-Venom   FALSE   TRUE                FALSE              FALSE            
##  6 Aquaman      TRUE    TRUE                FALSE              FALSE            
##  7 Arachne      TRUE    TRUE                FALSE              FALSE            
##  8 Archangel    TRUE    TRUE                FALSE              FALSE            
##  9 Ardina       TRUE    TRUE                FALSE              FALSE            
## 10 Ares         TRUE    TRUE                FALSE              FALSE            
## # ... with 87 more rows, and 163 more variables: cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>, ...
```

```r
#97
```

## Your Favorite
15. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>%
  filter(hero_names == "Indiana Jones") %>%
  glimpse();
```

```
## Rows: 1
## Columns: 168
## $ hero_names                   <chr> "Indiana Jones"
## $ agility                      <lgl> FALSE
## $ accelerated_healing          <lgl> FALSE
## $ lantern_power_ring           <lgl> FALSE
## $ dimensional_awareness        <lgl> FALSE
## $ cold_resistance              <lgl> FALSE
## $ durability                   <lgl> FALSE
## $ stealth                      <lgl> FALSE
## $ energy_absorption            <lgl> FALSE
## $ flight                       <lgl> FALSE
## $ danger_sense                 <lgl> FALSE
## $ underwater_breathing         <lgl> FALSE
## $ marksmanship                 <lgl> FALSE
## $ weapons_master               <lgl> FALSE
## $ power_augmentation           <lgl> FALSE
## $ animal_attributes            <lgl> FALSE
## $ longevity                    <lgl> FALSE
## $ intelligence                 <lgl> TRUE
## $ super_strength               <lgl> FALSE
## $ cryokinesis                  <lgl> FALSE
## $ telepathy                    <lgl> FALSE
## $ energy_armor                 <lgl> FALSE
## $ energy_blasts                <lgl> FALSE
## $ duplication                  <lgl> FALSE
## $ size_changing                <lgl> FALSE
## $ density_control              <lgl> FALSE
## $ stamina                      <lgl> TRUE
## $ astral_travel                <lgl> FALSE
## $ audio_control                <lgl> FALSE
## $ dexterity                    <lgl> FALSE
## $ omnitrix                     <lgl> FALSE
## $ super_speed                  <lgl> FALSE
## $ possession                   <lgl> FALSE
## $ animal_oriented_powers       <lgl> FALSE
## $ weapon_based_powers          <lgl> FALSE
## $ electrokinesis               <lgl> FALSE
## $ darkforce_manipulation       <lgl> FALSE
## $ death_touch                  <lgl> FALSE
## $ teleportation                <lgl> FALSE
## $ enhanced_senses              <lgl> FALSE
## $ telekinesis                  <lgl> FALSE
## $ energy_beams                 <lgl> FALSE
## $ magic                        <lgl> FALSE
## $ hyperkinesis                 <lgl> FALSE
## $ jump                         <lgl> FALSE
## $ clairvoyance                 <lgl> FALSE
## $ dimensional_travel           <lgl> FALSE
## $ power_sense                  <lgl> FALSE
## $ shapeshifting                <lgl> FALSE
## $ peak_human_condition         <lgl> FALSE
## $ immortality                  <lgl> FALSE
## $ camouflage                   <lgl> FALSE
## $ element_control              <lgl> FALSE
## $ phasing                      <lgl> FALSE
## $ astral_projection            <lgl> FALSE
## $ electrical_transport         <lgl> FALSE
## $ fire_control                 <lgl> FALSE
## $ projection                   <lgl> FALSE
## $ summoning                    <lgl> FALSE
## $ enhanced_memory              <lgl> FALSE
## $ reflexes                     <lgl> TRUE
## $ invulnerability              <lgl> FALSE
## $ energy_constructs            <lgl> FALSE
## $ force_fields                 <lgl> FALSE
## $ self_sustenance              <lgl> FALSE
## $ anti_gravity                 <lgl> FALSE
## $ empathy                      <lgl> FALSE
## $ power_nullifier              <lgl> FALSE
## $ radiation_control            <lgl> FALSE
## $ psionic_powers               <lgl> FALSE
## $ elasticity                   <lgl> FALSE
## $ substance_secretion          <lgl> FALSE
## $ elemental_transmogrification <lgl> FALSE
## $ technopath_cyberpath         <lgl> FALSE
## $ photographic_reflexes        <lgl> FALSE
## $ seismic_power                <lgl> FALSE
## $ animation                    <lgl> FALSE
## $ precognition                 <lgl> FALSE
## $ mind_control                 <lgl> FALSE
## $ fire_resistance              <lgl> FALSE
## $ power_absorption             <lgl> FALSE
## $ enhanced_hearing             <lgl> FALSE
## $ nova_force                   <lgl> FALSE
## $ insanity                     <lgl> FALSE
## $ hypnokinesis                 <lgl> FALSE
## $ animal_control               <lgl> FALSE
## $ natural_armor                <lgl> FALSE
## $ intangibility                <lgl> FALSE
## $ enhanced_sight               <lgl> FALSE
## $ molecular_manipulation       <lgl> FALSE
## $ heat_generation              <lgl> FALSE
## $ adaptation                   <lgl> FALSE
## $ gliding                      <lgl> FALSE
## $ power_suit                   <lgl> FALSE
## $ mind_blast                   <lgl> FALSE
## $ probability_manipulation     <lgl> FALSE
## $ gravity_control              <lgl> FALSE
## $ regeneration                 <lgl> FALSE
## $ light_control                <lgl> FALSE
## $ echolocation                 <lgl> FALSE
## $ levitation                   <lgl> FALSE
## $ toxin_and_disease_control    <lgl> FALSE
## $ banish                       <lgl> FALSE
## $ energy_manipulation          <lgl> FALSE
## $ heat_resistance              <lgl> FALSE
## $ natural_weapons              <lgl> FALSE
## $ time_travel                  <lgl> FALSE
## $ enhanced_smell               <lgl> FALSE
## $ illusions                    <lgl> FALSE
## $ thirstokinesis               <lgl> FALSE
## $ hair_manipulation            <lgl> FALSE
## $ illumination                 <lgl> FALSE
## $ omnipotent                   <lgl> FALSE
## $ cloaking                     <lgl> FALSE
## $ changing_armor               <lgl> FALSE
## $ power_cosmic                 <lgl> FALSE
## $ biokinesis                   <lgl> FALSE
## $ water_control                <lgl> FALSE
## $ radiation_immunity           <lgl> FALSE
## $ vision_telescopic            <lgl> FALSE
## $ toxin_and_disease_resistance <lgl> FALSE
## $ spatial_awareness            <lgl> FALSE
## $ energy_resistance            <lgl> FALSE
## $ telepathy_resistance         <lgl> FALSE
## $ molecular_combustion         <lgl> FALSE
## $ omnilingualism               <lgl> FALSE
## $ portal_creation              <lgl> FALSE
## $ magnetism                    <lgl> FALSE
## $ mind_control_resistance      <lgl> FALSE
## $ plant_control                <lgl> FALSE
## $ sonar                        <lgl> FALSE
## $ sonic_scream                 <lgl> FALSE
## $ time_manipulation            <lgl> FALSE
## $ enhanced_touch               <lgl> FALSE
## $ magic_resistance             <lgl> FALSE
## $ invisibility                 <lgl> FALSE
## $ sub_mariner                  <lgl> FALSE
## $ radiation_absorption         <lgl> FALSE
## $ intuitive_aptitude           <lgl> FALSE
## $ vision_microscopic           <lgl> FALSE
## $ melting                      <lgl> FALSE
## $ wind_control                 <lgl> FALSE
## $ super_breath                 <lgl> FALSE
## $ wallcrawling                 <lgl> FALSE
## $ vision_night                 <lgl> FALSE
## $ vision_infrared              <lgl> FALSE
## $ grim_reaping                 <lgl> FALSE
## $ matter_absorption            <lgl> FALSE
## $ the_force                    <lgl> FALSE
## $ resurrection                 <lgl> FALSE
## $ terrakinesis                 <lgl> FALSE
## $ vision_heat                  <lgl> FALSE
## $ vitakinesis                  <lgl> FALSE
## $ radar_sense                  <lgl> FALSE
## $ qwardian_power_ring          <lgl> FALSE
## $ weather_control              <lgl> FALSE
## $ vision_x_ray                 <lgl> FALSE
## $ vision_thermal               <lgl> FALSE
## $ web_creation                 <lgl> FALSE
## $ reality_warping              <lgl> FALSE
## $ odin_force                   <lgl> FALSE
## $ symbiote_costume             <lgl> FALSE
## $ speed_force                  <lgl> FALSE
## $ phoenix_force                <lgl> FALSE
## $ molecular_dissipation        <lgl> FALSE
## $ vision_cryo                  <lgl> FALSE
## $ omnipresent                  <lgl> FALSE
## $ omniscient                   <lgl> FALSE
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
