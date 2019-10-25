Analyzing College Fight Songs
================
Power Ninja Data Turtles
10/25/19

### Section 1: Introduction

In our research project, we will be analyzing the fight songs of various
college football teams to discover whether a song’s tempo or duration
can tell us anything about the content of the song and whether a team’s
fight song is any indicator of the team’s success. More specifically, we
will be examining the fight songs of all 65 teams located across the
Power 5 sports conferences (Big 10, Big 12, ACC, Pac-12 and SEC) plus
Notre Dame (Independent conference). Our dataset, which is fittingly
titled `fight-songs`. Each observation in the set represents a distinct
Power 5 college football team. For each team (observation), the dataset
features 23 variables (of which we are using 19, plus one of our own,
for a total of 20), which contain a wide range of information, primarily
concerning the school’s fight song.

The variables are as follows: `school`, `conference`, `song_name`,
`writers`, `year`, `student_writer`, `official_song`, `bpm`,
`sec_duration`, `fight`, `number_fights`, `victory_win_won`, `rah`,
`nonsense`, `colors`, `men`, `opponents`, `spelling`, `trope_count`, and
finally, `rank`. Detailed explanations of each variable are located in
the codebook.

The data was collected by looking at the lyrics of each song (as
published by the school), metadata about each fight song on Spotify,
history about each song (as stated by the school), and information about
each school’s conference, which is easily accessible on the Internet. We
added the `rank` variable to the dataset using Microsoft Excel, and we
found this information from the Associated Press’s historic rankings of
every college football team in the country, which was released last
year.

As as note, some schools may have more than one fight song, and some of
the songs sanctioned as “official” by their schools aren’t the ones that
fans most commonly sing out. The songs that seemed best-known and
best-loved were chosen as the “official” fight song. Additionally,
fivethirtyeight has chosen their lyrics to those sung most regularly and
published by the school, so some verses won’t appear.

### Section 2: Exploratory Data Analysis

We are interested in figuring out whether a team’s tempo (measured in
bpm, or beats per minute) has anything to do with the number of clichés
that a song has. First, we will create a histogram and calculate the
summary statistics to visualize the distribution of tempos across fight
songs for all teams.

``` r
ggplot(data = fight_songs, mapping = aes(x = bpm)) + 
  geom_histogram(binwidth = 10) + 
  labs(x = "Tempo (bpm)", y = "Number of Songs", title = "Tempos of Fight Songs")
```

![](proposal_files/figure-gfm/histogram_bpm-1.png)<!-- -->

Let’s also calculate the summary statistics for this distribution.
Specifically, we will use the median as a measure of center and the
interquartile range as a measure of spread (due to the bimodal nature of
the distribution). In addition, we will find the upper and lower
quartiles (Q3 and Q1 respectively), and the maximum and minimum values:

``` r
fight_songs %>%
  summarise(median = median(bpm), 
            IQR = IQR(bpm), 
            Q1 = quantile(bpm, 0.25), 
            Q3 = quantile(bpm, 0.75), 
            min = min(bpm), 
            max = max(bpm))
```

    ## # A tibble: 1 x 6
    ##   median   IQR    Q1    Q3   min   max
    ##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1    140    61    90   151    65   180

Based on the histogram, it is clear that the shape of the data is
clearly bimodal, with two distinct peaks occurring around 40 bpm and
around 150 bpm. There are more songs that are clustered around the
higher bpm mode. The center (median) occurs at around 140 bpm, and the
spread (IQR) is 61 bpm, indicating that there is a moderate amount of
variability in tempos. There are no outliers.

Now, let’s see whether there appears to be a relationship between a
song’s tempo (bpm) and the number of clichés (tropes) that a song has.
We will do this by creating a scatterplot and fitting a linear model.

``` r
ggplot(data = fight_songs, mapping = aes(x = bpm, y = trope_count)) +
  geom_jitter() +
  geom_smooth(method = "lm", se = FALSE) +
  labs(x = "Tempo (bpm)", y = "Number of Clichés", title = "Number of Clichés vs. Tempo")
```

![](proposal_files/figure-gfm/scatterplot-bpm-tropes-1.png)<!-- -->

There seems to be a weak, negative linear relationship between tempo and
number of clichés. Let’s find the linear model associated with this
scatterplot:

``` r
(m_bpm <- lm(data = fight_songs, trope_count ~ bpm))
```

    ## 
    ## Call:
    ## lm(formula = trope_count ~ bpm, data = fight_songs)
    ## 
    ## Coefficients:
    ## (Intercept)          bpm  
    ##    4.593162    -0.007591

Based on the output, the linear model that predicts number of tropes
based on tempo is: trope\_count-hat = 4.593162 - 0.007591 \* bpm. The
intercept of 4.593162 tells us that if a song has 0 bpm (nonsensical),
it is expected to have 4.593162 clichés (tropes), on average. The slope
of -0.007591 tells us that for an increase in 1 bpm, the expected number
of clichés is predicted to decrease by 0.007591. The R-squared value of
this model is 0.0225985, meaning that approximately 2.2598527 percent of
the variability in clichés is accounted for by the linear model. This
means that the linear model is relatively weak.

Now, let’s see whether trope count is any indicator as to the rank
(success) of a team. We will do this by creating another scatterplot and
fitting a model.

``` r
ggplot(data = fight_songs, mapping = aes(x = trope_count, y = rank)) +
  geom_jitter() +
  geom_smooth(method = "lm", se = FALSE) +
  labs(x = "Number of Clichés in Fight Song", y = "Team Rank", title = "Team Rank vs. Number of Clichés")
```

![](proposal_files/figure-gfm/scatterplot-tropes-rank-1.png)<!-- -->

There seems to be a weak positive linear relationship between number of
clichés in a fight song and team rank. Let’s find the linear model
associated with this scatterplot:

``` r
(m_tropes_rank <- lm(data = fight_songs, rank ~ trope_count))
```

    ## 
    ## Call:
    ## lm(formula = rank ~ trope_count, data = fight_songs)
    ## 
    ## Coefficients:
    ## (Intercept)  trope_count  
    ##     35.5818       0.5455

Based on the output, the linear model that predicts rank based on number
of clichés is: rank-hat = 35.5818 + 0.5455 \* trope\_count. The
intercept of 35.5818 tells us that if a song has 0 clichés, it is
expected to have a ranking of 35.5818, on average. The slope of 0.5455
tells us that for an increase in 1 cliché, the expected team ranking is
predicted to increase by 0.5455 points. The R-squared value of this
model is 0.0012119, meaning that approximately 0.1211865 percent of the
variability in ranks is accounted for by the linear model. This means
that the linear model is relatively weak.

### Section 3: Research Questions

Our first research question is: how does the tempo and duration of a
college football team’s fight song predict the content of the song
(i.e. the number of clichés/tropes)? We will utilize the `trope_count`
variable, which counts the number of clichés in a fight song (a cliché
is whether a song contains the word “fight”, the word “victory”, the
word “won”, the word “rah”, nonsense syllables, school colors, a
reference to “men”/“boys”/sons“, opponent name, or spells something out)
to form connections between the a song’s duration and tempo and its
content. The predictor variables we are interested in are `bpm`, which
we started to explore in Section 2, and `sec_duration`. We aim to
describe the relationship, if any exists, between the two numerical
predictor (X) variables and the numerical response (Y) variable,
`trope_count`. Based on the exploratory data analysis from Section 2, we
hypothesize that the number of clichés will be less for songs with
slower tempos (`bpm`) and probably shorter durations (`sec_duration`).
Combining tempo and duration, we hope to separate songs into 4
categories,”short and fast," “short and slow”, “long and fast,” and
“long and slow”, and thus compare the number of tropes across these
four categories.

Our second research question is: how do characteristics of fight songs
of college football teams correspond to their respective historical
levels of success? We will utilize the `rank` variable to form
connections between the performances of college football teams and their
respective fight songs. The predictor variables we are interested in
are: `victory_win_won`, `opponents`, `nonsense`, and `rah`. We aim to
describe the relationship, if any exists, between these four categorical
predictor (X) variables and the numerical response (Y) variable, `rank`.
We hypothesize that a statistically significant relationship exists
between `victory_win_won` and `rank` because it is reasonable to asssume
historically successful college football teams would incorporate
symbolic elements of their dominance into their iconic fight songs. In
this case, we believe the words “victory”, “win”, and “won” are symbolic
elements of dominance, giving us reason to believe higher-ranked teams
are more likely to have their fights songs include these words. We also
hypothesize that a statistically significant relationship exists between
`opponents` and `rank` since higher-ranked college football teams are
likely to have developed more emotionally-charged rivalries with their
highly successful peers. With such long-standing, emotional rivalries,
it is reasonable to believe fight songs associated with these
highly-ranked college football programs allude to these rivals by name.
Moreover, we hypothesize that a statistically significant relationship
exists between `nonsense` and `rank` because there have been a plethora
of articles written about how, in the past, highly successful teams
included nonsensical phrases in their fight songs to distract the
players on the opposing college football teams. Since the fight songs
included in our dataset were written decades ago (as evidenced by the
`year` variable), it is reasonable to assume the songs associated with
the historically-best college teams will have a higher likelihood of
including nonsense (e.g “Hooperay”). Finally, we hypothesize there is no
statistically significant relationship between `rah` and `rank` since
“rah” seems like it would be a common word in a fight song,
irrespective of the quality (how successful) of a college football team.

### Section 4: Data

Below, we use the glimpse() function to display a preview of the dataset
`fight_songs`:

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
