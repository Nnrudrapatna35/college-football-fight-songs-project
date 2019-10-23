Analyzing College Fight Songs
================
Power Ninja Data Turtles
10/25/19

### Section 1. Introduction

### Section 2. Exploratory data analysis

We are interested in learning whether the number of times that a fight
song contains the word “fight” varies based on conference, as the more
times “fight” is said might predict whether a conference as a whole is
more or less aggressive than another conference. First, let’s analyze
the variable number\_fights, which counts the number of times a song
says “fight,” for all the teams. The histogram of number\_fights is
below:

``` r
ggplot(data = fight_songs, mapping = aes(x = number_fights)) + 
  geom_histogram(binwidth = 1) + 
  labs(x = "Occurrences of 'Fight' in a Song", y = "Number of Songs", title = "Number of Occurrences of 'Fight'")
```

![](proposal_files/figure-gfm/histogram-number_fights-1.png)<!-- -->

Let’s also get the summary statistics for this distribution. In
particular, we will use the median as a measure of center and IQR as a
measure of spread (due to the skewness of the distribution). In
addition, we will find Q1, Q3, the maximum, and the minimum.

``` r
fight_songs %>%
  summarise(median = median(number_fights), 
            IQR = IQR(number_fights), 
            Q1 = quantile(number_fights, 0.25), 
            Q3 = quantile(number_fights, 0.75), 
            min = min(number_fights), 
            max = max(number_fights))
```

    ## # A tibble: 1 x 6
    ##   median   IQR    Q1    Q3   min   max
    ##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1      2     5     0     5     0    17

Based on the histogram, the shape of the distribution is clearly skewed
to the right, meaning that there are more songs that say “fight” only a
few times than songs that say “fight” multiple times. The distribution
is clearly unimodal with one distinct peak at 0. The center (median) of
the distribution occurs around 2, indicating that at least 50% of the
songs contain 2 or less occurrences of the word “fight.” The data is
fairly spread out, as indicated by an IQR of 5 and a range of 17. There
are 6 outliers, which are the songs with number of fights greater than
or equal to 8.

### Section 3. Research questions

### Section 4. Data

Below is a glimpse of the data set fight\_songs:

``` r
glimpse(fight_songs)
```

    ## Observations: 65
    ## Variables: 20
    ## $ school          <chr> "Notre Dame", "Baylor", "Iowa State", "Kansas", …
    ## $ conference      <chr> "Independent", "Big 12", "Big 12", "Big 12", "Bi…
    ## $ song_name       <chr> "Victory March", "Old Fight", "Iowa State Fights…
    ## $ writers         <chr> "Michael J. Shea and John F. Shea", "Dick Baker …
    ## $ year            <chr> "1908", "1947", "1930", "1912", "1927", "1905", …
    ## $ student_writer  <chr> "No", "Yes", "Yes", "Yes", "Yes", "Yes", "No", "…
    ## $ official_song   <chr> "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes",…
    ## $ bpm             <dbl> 152, 76, 155, 137, 80, 153, 180, 81, 149, 159, 1…
    ## $ sec_duration    <dbl> 64, 99, 55, 62, 67, 37, 29, 65, 47, 54, 92, 60, …
    ## $ fight           <chr> "Yes", "Yes", "Yes", "No", "Yes", "No", "Yes", "…
    ## $ number_fights   <dbl> 1, 4, 5, 0, 6, 0, 5, 17, 2, 8, 0, 0, 1, 9, 8, 0,…
    ## $ victory_win_won <chr> "Yes", "Yes", "No", "No", "Yes", "No", "Yes", "Y…
    ## $ rah             <chr> "Yes", "No", "Yes", "No", "No", "Yes", "No", "No…
    ## $ nonsense        <chr> "No", "No", "No", "Yes", "No", "No", "No", "No",…
    ## $ colors          <chr> "Yes", "Yes", "No", "No", "Yes", "No", "No", "Ye…
    ## $ men             <chr> "Yes", "No", "Yes", "Yes", "No", "No", "Yes", "N…
    ## $ opponents       <chr> "No", "No", "No", "Yes", "No", "No", "Yes", "Yes…
    ## $ spelling        <chr> "No", "Yes", "Yes", "No", "No", "Yes", "No", "No…
    ## $ trope_count     <dbl> 6, 5, 4, 3, 3, 2, 4, 4, 6, 3, 1, 6, 3, 3, 3, 0, …
    ## $ rank            <dbl> 5, 48, 120, 64, 50, 1, 46, 7, 28, 63, 40, 48, 76…
