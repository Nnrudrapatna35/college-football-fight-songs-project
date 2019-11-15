Data Analysis
================
Power Ninja Data Turtles
11.15.2019

### Load data and packages

``` r
library(tidyverse)
library(infer)
library(broom)

fight_songs <- read_csv("/cloud/project/data/fight-songs.csv")
```

    ## Warning: Missing column names filled in: 'X21' [21]

``` r
fight_songs <- fight_songs %>%
  select(-X21)
```

### Research Question 1

First, we will create a linear model for
