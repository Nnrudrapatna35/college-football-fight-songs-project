Rah\! Fight\! Win\!: Analyzing the Nonsensical Nature of College Fight
Songs
================
Power Ninja Data Turtles
12/13/19

### Statistical Methods Used

For our data analysis, we used a combination of hypothesis tests and a
variety of plots (from the `ggplot` package) in order to visualize and
interpret each question.

We used hypothesis tests because finding the p-value for each instance
allowed us to conclude whether or not the data provided convincing
evidence of a trend, such as the relationship between the tempo and
content of songs. Our hypothesis tests were conducted using simulation
instead of the CLT. The CLT was not a valid option, as none of the
subsets of the data set had at least 30 observations and the majority of
our tests are for independence (difference between two groups).

Visualizing the relationships between the explanatory and response
variables with plots made it easy for viewers to understand how the
level or occurrence of certain variables changed based on differing
factors between songs. We tried to use unique but clearly structured and
labeled plot formats that would display information in a visually
appealing way without confusing the
viewer.

### Research Question 1: Predicting the Content of College Fight Songs Using Tempo and Duration

Our first research question is:

> “How does the tempo and duration of a college football team’s fight
> song predict the content of the song, specifically the number of
> clichés/tropes?”

Before delving deeper into the analysis, we will first look at the
distributions of the two explanatory variables, `bpm` and
`sec_duration`.

Starting with `bpm`, which is a measure of a fight song’s tempo, we will
create a histogram of the distribution:

![](writeup_files/figure-gfm/histogram-bpm-1.png)<!-- -->

We will also calculate the summary statistics for this distribution.
Specifically, we will use the median as a measure of center and the
interquartile range as a measure of spread (due to the bimodal nature of
the distribution). In addition, we will find the upper and lower
quartiles (Q3 and Q1, respectively), and the maximum and minimum values:

    ## # A tibble: 1 x 6
    ##     min    Q1 median    Q3   max   IQR
    ##   <dbl> <dbl>  <dbl> <dbl> <dbl> <dbl>
    ## 1    65    90    140   151   180    61

Based on the histogram, it is clear that the shape of the data is
clearly bimodal, with two distinct peaks occurring around 70 bpm and
around 150 bpm, with a split in the data at around 100 bpm. There are
more songs that are clustered around the higher bpm mode. The center
(median) occurs at 140 bpm, and the spread (IQR) is 61 bpm, indicating
that there is a moderate amount of variability in tempos. There are no
outliers in this distribution. Due to the bimodal result, it seems like
there is a natural grouping between “slow” songs and “fast” songs, so we
will use the `mutate()` function to add a new variable, `tempo`, which
is “slow” if a song’s `bpm` is less than or equal to 100 bpm and “fast”
if a song’s tempo is greater than 100 bpm.

Now, we will explore our second explanatory variable, `sec_duration`,
which is the duration of a song in seconds. In order to do this, we will
create a histogram of the distribution and find the relevant summary
statistics (Q1, median, Q3, min, max,
IQR):

![](writeup_files/figure-gfm/histogram-summary-stats-sec-duration-1.png)<!-- -->

    ## # A tibble: 1 x 6
    ##     min    Q1 median    Q3   max   IQR
    ##   <dbl> <dbl>  <dbl> <dbl> <dbl> <dbl>
    ## 1    27    58     67    85   172    27

Based on the histogram, we can see that the distribution of fight song
durations is roughly symmetric (slightly skewed to the right) and
unimodal, with a peak at around 70. There are some outliers on the
higher end of the spectrum (`sec_duration` \> 125.5), indicating that
these songs are significantly longer than the others. The center of the
distribution occurs at around 67 seconds, and the IQR of the
distribution is 27 seconds, which is relatively narrow, indicating that
fight songs do not have dramatically different lengths. Like we did with
`bpm`, let’s add a new variable, `length`, which is “short” if a song is
less than or equal to the median of 67 seconds and “long” if a song is
greater than the median of 67 seconds.

Now that we have a better understanding of our two explanatory
variables, we want to add a new variable, `classify`, which combines
`bpm` and `sec_duration` by labeling each song with one of four
classifications: “slow and short”, “slow and long”, “fast and short”,
and “fast and long”:

    ## # A tibble: 4 x 2
    ##   classify           n
    ##   <chr>          <int>
    ## 1 fast and long     23
    ## 2 fast and short    25
    ## 3 slow and long      9
    ## 4 slow and short     8

Based on the output displayed in the tibble above, 23 songs are
categorized as “fast and long”, 25 songs are categorized as “fast and
short”, 9 songs are categorized as “slow and long”, and 8 songs are
categorized as “slow and short”.

Using these classifications, we will visualize the groups and fill in
each observation with the number of tropes associated with that
particular observation. We choose to use `geom_hline()` and
`geom_vline()` to add horizontal and vertical reference lines to the
jittered plot (`geom_jitter()` adds a small amount of random variation
to the location of each point). We also choose to use
`scale_color_gradientn()` to create a rainbow-colored gradient scale,
which indicates the number of tropes for a given fight song.

![](writeup_files/figure-gfm/visualize-classify-1.png)<!-- -->

The upper left-hand region of the plot represents songs that are “fast
and short” while the lower left-hand region represents songs that are
“slow and short.” The upper right-hand region of the plot represents
songs that are “fast and long”, and the lower right-hand region
represents songs that are “slow and long”.

Finally, we will explore our response variable, `trope_count`, which is
a measure of the number of clichés/tropes in a given fight song. We will
also use a histogram and summary statistics for this univariate
analysis:

![](writeup_files/figure-gfm/histogram-summary-stats-trope-count-1.png)<!-- -->

    ## # A tibble: 1 x 7
    ##     min    Q1 median    Q3   max   IQR    sd
    ##   <dbl> <dbl>  <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1     0     3      4     5     8     2  1.67

Based on the histogram and summary statistics, we can see that the
distribution for number of tropes is unimodal with a peak at around 4
and skewed slightly to the left. There is 1 outlier at the maximum value
of our distribution (8 tropes). The center of the distribution,
described by the median, is 4 tropes, and the IQR of 2 indicates that
there is not a large amount of variability in the number of tropes for
college fight songs.

Now, we will return to our research question by examining whether the
amount of clichés varies based on a song’s classification. First, we
will create violin plots for the distribution of the number of tropes
for each classification and add jitter. Using jittered points allows us
to manage the effects of overplotting when working with smaller
datasets. We were inspired to use this type of plot after conferencing
with Dr. Eric Monson who delivered the “Tips for Effective Data
Visualization” presentation earlier this
semester.

![](writeup_files/figure-gfm/boxplots-summary-stats-number-tropes-1.png)<!-- -->

    ## # A tibble: 4 x 4
    ##   classify       median   IQR    sd
    ##   <chr>           <dbl> <dbl> <dbl>
    ## 1 fast and long       4   2.5  1.73
    ## 2 fast and short      4   1    1.56
    ## 3 slow and long       5   2    2.28
    ## 4 slow and short      4   2    1.13

Not surprisingly, all of the distributions, except for “slow and long”,
are centered at 4 tropes, which is the median number of tropes for all
songs in the dataset. However, there is one category, “slow and long,”
which has a median of 5. We would like to check whether this median is
statistically significant or not. To do this, we will conduct a
hypothesis test for the median number of tropes for songs that are
considered “slow and long”. Our null hypothesis is that the true median
number of tropes for “slow and long” songs is 4, H0: median(“slow and
long”) = 4. Our alternative hypothesis is that the true median number of
tropes is different than 4, Ha: median(“slow and long”) ≠ 4. We will use
a significance (alpha) level of 0.05. We will create and visualize the
null distribution and calculate the associated p-value:

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.038

![](writeup_files/figure-gfm/hypothesis-test-slow-and-long-1.png)<!-- -->

Based on our p-value of 0.038, which is less than 0.05 (significance
level), we reject the null hypothesis in favor of the alternative
hypothesis; in other words, there is sufficient evidence that the true
median number of tropes is different than 4. Thus, the data provide
convincing evidence that the median number of tropes for songs that are
slow and long is different than the population median of 4. However, our
sample size is very small (9 observations), so we must be wary of our
results and acknowledge the limitations of our conclusions.

We can also see from the visualization “Number of Clichés” that the
spread for “slow and short” songs is much smaller compared to all other
song classifications. We will test whether the standard deviation for
“slow and short” songs is significantly different from the population
standard deviation of 1.675. The null hypothesis is that the true
standard deviation in the number of tropes for “slow and short” songs is
1.675, H0: sigma = 1.675. The alternative hypothesis is that the true
standard deviation in the number of tropes for “slow and short” songs is
less than 1.675, Ha: sigma \< 1.675. We will use a significance level of
0.05.

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.685

![](writeup_files/figure-gfm/hypothesis-test-slow-and-short-1.png)<!-- -->

Based on the p-value of 0.685, which is greater than our alpha level of
0.05, we fail to reject the null hypothesis in favor of the alternative
hypothesis; in other words, the data do not provide convincing evidence
that the true standard deviation for slow and short songs is less than
the population standard deviation of 1.675.

Now, we will find the full linear model that predicts number of tropes
(`trope_count`) from tempo (`bpm`) and duration (`sec_duration`):

    ## # A tibble: 3 x 2
    ##   term          estimate
    ##   <chr>            <dbl>
    ## 1 (Intercept)   4.58    
    ## 2 bpm          -0.00757 
    ## 3 sec_duration  0.000208

Based on the output, the full linear model is `trope_count-hat` = 4.58 -
0.00757 \* `bpm` + 0.000208 \* `sec_duration`. The R-squared value is
0.022608, which means that approximately 2.2608044% of the variability
in trope counts can be explained by the linear model that predicts trope
count from a song’s tempo and duration. Given this R-squared value, our
model is very weak, since, in general, the closer the R-squared value is
to 1 (100% of the variability in trope counts can be explained by the
model), the more accurate and useful the final model is. As the model
only accounts for a small percent of the variability in trope counts, it
is not a great predictor of a fight song’s trope count.

The intercept tells us that for a song with 0 bpm which lasts 0 seconds,
the expected number of tropes is 4.58. But of course, this is
nonsensical, since none of the fight songs we are considering in our
analysis last 0 seconds (for that matter, no songs, by definition, last
0 seconds\!). The slope coefficient associated with `bpm` tells us that
for an increase in 1 bpm, the number of tropes is expected, on average,
to decrease by 0.00757, holding all else constant. The slope coefficient
associated with `sec_duration` tells us that for an increase in a song’s
duration by 1 second, the number of tropes is expected, on average, to
increase by 0.000208, holding all else constant.

Now, we will conduct a hypothesis test for the slopes to determine
whether the slope coefficients for `bpm` and `sec_duration` are
significantly different from 0. We will use a significance level of
0.05. Conducting a hypothesis test for the slopes will help us confirm
or deny our research question on whether either tempo or duration are
useful predictors of the content of a fight song, as determined by a
song’s number of tropes.

Let our null hypotheses be that the slope coefficients associated with
both variables are 0; H0: beta(`bpm`) = 0 and beta(`sec_duration`) = 0.
Let our alternative hypotheses be that the slope coefficients associated
with both variables are significantly different than 0, Ha: beta(`bpm`)
≠ 0 and beta(`sec_duration`) ≠ 0.

First, we will look at the slope of `bpm`, which we found to be 0.00757:

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.238

![](writeup_files/figure-gfm/hypothesis-test-bpm-slope-1.png)<!-- -->

Next, we will look at the slope of `tempo`, which we found to be
0.000208:

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.978

![](writeup_files/figure-gfm/hypothesis-test-sec-duration-slope-1.png)<!-- -->

The p-values for the slope coefficients of `bpm` and `sec_duration` are
0.238 and 0.978, respectively. Since both p-values are greater than our
alpha level of 0.05, we fail to reject the null hypotheses in favor of
the alternative hypotheses. Hence, there appears to be insufficient
evidence that the slope coefficients for `bpm` and `sec_duration` are
different than 0.

However, we must must verify that the four conditions for inference for
regression are satisfied before finalizing any conclusions: (1)
observations should be independent; (2) residuals should be randomly
distributed around 0; (3) residuals should be nearly normally
distributed, centered at 0; (4) residuals should have constant variance.

First, we can assume that the observations (fight songs) are independent
because college fight songs are unique to every team and written by
different authors.

Next, we will verify our assumption (confirm independence of
observations) by plotting the residuals in the order of data collection:

![](writeup_files/figure-gfm/testing-condition-1-1.png)<!-- -->

Next, let’s examine the normality of residuals:

![](writeup_files/figure-gfm/testing-condition-3-1.png)<!-- -->

Finally, we will check the distribution of residuals around 0 and
constant variance:

![](writeup_files/figure-gfm/testing-conditions-2-and-4-1.png)<!-- -->

Clearly, condition 4 (constant variance) is violated. Therefore,
inference for regression is invalid. This means our conclusion that
there is insufficient evidence to suggest the slope coefficients for
`bpm` and `sec_duration` are different than 0 is also not credible.

To confirm that there is no better linear model, we will use the
`step()` function and backwards selection with AIC as the selection
criterion:

    ## # A tibble: 1 x 2
    ##   term        estimate
    ##   <chr>          <dbl>
    ## 1 (Intercept)     3.62

Since backwards selection removed both `bpm` and `sec_duration` from our
model, we should conclude that these two variables are not valid
predictors for the number of tropes in a college fight song.

However, we must consider one last element before concluding that `bpm`
and `sec_duration` are poor predictors of `trope_count`: whether the
requirements for linear regression are satisfied. Amongst other things,
a linear model mandates a continuous numerical response variable\! But,
it is clear that `trope_count` does not fulfill this criterion since it
is a counting (discrete) numerical response variable. Thus, our final
conclusions are as follows: this method of analysis involving linear
regression is invalid and there is no guarantee that the explanatory
variables are poor predictors of the response variable.

### Conclusion from Research Question 1

Circling back to our research question of whether a fight song’s tempo
and duration can help predict the content of a college fight song, as
measured by the number of clichés, we hypothesized that the number of
clichés would be less for songs with slower tempos (smalle `bpm`) and
probably shorter durations (smaller `sec_duration`). We thought that
longer songs would contain more clichés because there is more time to
fill the lyrics with clichés. We thought that faster tempos, since they
are generally related to higher energies, would have included more
clichés.

From our exploratory data analysis, we noticed that there was a natural
grouping in fight songs such that all fight songs could be classified as
having a slow or fast tempo. With this information and with the results
from exploring the duration variable, we decided to classify all fight
songs into four categories: “short and fast,” “short and slow,” “long
and fast,” and “long and slow.”

After creating a violin plot for each classification and its respective
distribution of the number of tropes, we noticed two things. First,
“slow and long” songs had a greater median number of tropes. Second,
“slow and short” songs had a smaller variation in the number of
tropes. From here, we decided to conduct a hypothesis test to confirm or
deny whether what we observed in the violin plots was statistically
significant. It turned out that “slow and long” songs in our data set
did, in fact, have median number of tropes that was different than the
the population median of 4 (p = 0.038). However, the small spread of the
distribution of “slow and short” songs proved not to be different from
the population standard deviation of 1.67 (p = 0.685).

Finally, we looked at the linear model between `bpm` and `sec_duration`,
the explanatory variables, and `trope_count` to see whether `bpm` and
`sec_duration` were, indeed, good predictors of the number of tropes in
a college fight song. The original model had an R-squared value of
0.022608, which indicated that the model was very weak. After conducting
hypothesis tests for the slope of `sec_duration` and `bpm`, we found
that neither slope was significantly different than 0 (p = 0.238 and p =
0.978, respectively). However, after checking the conditions of
inference for regression, we found that the residuals did not have
constant variance; therefore, our inference for regression is invalid,
and our conclusion that there is insufficient evidence to suggest the
slope coefficients for `bpm` and `sec_duration` are different than 0 is
not credible.

Finally, we used backwards selection using AIC as the criterion to see
if there was a particularly better linear model. However, backwards
selection removed both of our explanatory variables, `bpm` and
`sec_duration`, which indicated to use that these two variables are not
valid predictors for the number of tropes in a college fight song.
Therefore, the answer to our research question is that neither tempo nor
duration can predict the content of a college fight song. The only thing
we can say is that “slow and long” fight songs have a different median
number of tropes than other fight
songs.

## Additional Analysis: Looking at Characteristics in Fight Songs in Northern and Southern Athletic Conferences

For the last part of our analysis, we are interested in examining
whether certain characteristics of fight songs are regionally unique. In
particular, we are interested in looking at the variables
`victory_win_won`, whether a song includes the words “victory”, “win”,
or "won, `men`, whether a song refers to male groups (fathers, sons,
men, etc.), and `nonsense`, whether a song uses nonsense syllables, to
see whether northern schools (conferences) are more or less likely to
have fight songs with these characteristics.

First, we will add a new variable to the `fight_songs` dataset called
`region`, which will represent the region of the United States that a
given college is located based on its conference – north or south. Since
Notre Dame is the only college we will consider in the analysis that is
independent (not in a conference), we will encode it to a region on its
own (north). It should also be noted that the PAC-12 and ACC conferences
are considered western and eastern conferences, respectively, but for
the purpose of our analysis, we will label them as northern and
southern, respectively, because we are interested in the characteristics
of fight songs for conferences in the north vs. south. Exactly half of
the teams in the PAC-12 conference are, indeed, northern schools, while
the majority of schools in the ACC conference are southern.

    ## # A tibble: 2 x 2
    ##   region     n
    ##   <chr>  <int>
    ## 1 north     27
    ## 2 south     38

There are 27 schools that we are considering to be in the northern
region and 38 in the southern region.

We will look at our first variable, `victory_win_won` to see whether
there exists a statistically significant relationship between the
proportion of teams for which `victory_win_won` is “yes” for northern or
southern conferences. First, we will find what the two proportions are.

    ## # A tibble: 4 x 4
    ## # Groups:   region [2]
    ##   region victory_win_won     n prop_victory_win_won_yes
    ##   <chr>  <chr>           <int>                    <dbl>
    ## 1 north  No                  8                    0.296
    ## 2 north  Yes                19                    0.704
    ## 3 south  No                 16                    0.421
    ## 4 south  Yes                22                    0.579

Based on the output, 70.4% of schools in northern conferences have the
words “victory,” “win,” or “won” in their fight songs. On the other
hand, only 57.9% of schools in southern conferences have the words
“victory,” “win,” or “won” in their fight songs. We would like to see
if this result is statistically significant and what the true difference
in proportions is. We have to use simulation-based hypothesis testing
because we are looking for the difference in proportions.

First, we will run a hypothesis test, calculate the p-value, and
interpret the results in order to determine whether there is a
statistically significant difference in the proportion of fight songs
that say “victory,” “win,” or “won” for schools in northern vs. southern
conferences. Our null hypothesis is that there is no difference in the
proportion of schools that say “victory,” “win,” or “won” in their fight
songs for southern and northern conferences, H0: p(southern) =
p(northern). Our alternative hypothesis is that there is a difference in
the proportion of schools that say “victory,” “win,” or “won” in their
fight songs for southern and northern conferences, Ha: p(southern) ≠
p(northern). We will use a significance level of 0.05.

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.418

![](writeup_files/figure-gfm/hyp-test-victory-win-won-1.png)<!-- -->

Based on our p-value of 0.418, which is greater than our significance
level of 0.05, we fail to reject the null hypothesis. There is
insufficient evidence that there is a difference in the proportion of
fight songs that include “victory,” “win,” or “won” for southern and
northern conferences.

Next, we will look at our second variable, `men`, which refers to
whether a fight song mentions male groups in its lyrics, to see whether
there exists a statistically significant relationship between the
proportion of teams for which `men` is “yes” for northern or southern
conferences. First, we will find what the two proportions are:

    ## # A tibble: 4 x 4
    ## # Groups:   region [2]
    ##   region men       n prop_men
    ##   <chr>  <chr> <int>    <dbl>
    ## 1 north  No       15    0.556
    ## 2 north  Yes      12    0.444
    ## 3 south  No       26    0.684
    ## 4 south  Yes      12    0.316

Based on the output, 44.4% of schools in northern conferences mention
male groups in their fight songs. On the other hand, only 31.6% of
schools in southern conferences mention male groups in their fight
songs. We would like to see if this result is statistically significant
and what the true difference in proportions is. We have to use
simulation-based hypothesis testing because we are looking for the
difference in proportions.

First, we will run a hypothesis test, calculate the p-value, and
interpret the results in order to determine whether there is a
statistically significant difference in the proportion of fight songs
that mention male groups for schools in northern vs. southern
conferences. Our null hypothesis is that there is no difference in the
proportion of schools that mention men in their fight songs for southern
and northern conferences, H0: p(southern) = p(northern). Our alternative
hypothesis is that there is a difference in the proportion of schools
that mention men in their fight songs for southern and northern
conferences, Ha: p(southern) ≠ p(northern). We will use a significance
level of 0.05.

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.454

![](writeup_files/figure-gfm/hyp-test-men-1.png)<!-- -->

Based on our p-value of 0.454, which is greater than our significance
level of 0.05, we fail to reject the null hypothesis. There is
insufficient evidence that there is a difference in the proportion of
fight songs that mention male groups for southern and northern
conferences.

Finally, we will look at our last variable, `nonsense`, which refers to
whether a fight song uses nonsense syllables in its lyrics, to see
whether there exists a statistically significant relationship between
the proportion of teams for which `nonsense` is “yes” for northern or
southern conferences. First, we will find what the two proportions are:

    ## # A tibble: 4 x 4
    ## # Groups:   region [2]
    ##   region nonsense     n prop_nonsense
    ##   <chr>  <chr>    <int>         <dbl>
    ## 1 north  No          23         0.852
    ## 2 north  Yes          4         0.148
    ## 3 south  No          32         0.842
    ## 4 south  Yes          6         0.158

Based on the output, 14.8% of schools in northern conferences use
nonsense syllables in their fight songs. On the other hand, 15.8% of
schools in southern conferences use nonsense syllables in their fight
songs. We would like to see if this result is statistically significant
and what the true difference in proportions is. We have to use
simulation-based hypothesis testing because we are looking for the
difference in proportions.

First, we will run a hypothesis test, calculate the p-value, and
interpret the results in order to determine whether there is a
statistically significant difference in the proportion of fight songs
that use nonsense syllables for schools in northern vs. southern
conferences. Our null hypothesis is that there is no difference in the
proportion of schools that use nonsense syllables in their fight songs
for southern and northern conferences, H0: p(southern) = p(northern).
Our alternative hypothesis is that there is a difference in the
proportion of schools that mention men in their fight songs for southern
and northern conferences, Ha: p(southern) ≠ p(northern). We will use a
significance level of 0.05.

    ## # A tibble: 1 x 1
    ##   p_value
    ##     <dbl>
    ## 1   0.648

![](writeup_files/figure-gfm/hyp-test-nonsense-1.png)<!-- -->

Based on our p-value of 0.648, which is greater than our significance
level of 0.05, we fail to reject the null hypothesis. There is
insufficient evidence that there is a difference in the proportion of
fight songs that use nonsense syllables for southern and northern
conferences.

### Conclusion from Additional Analysis

We hypothesized that characteristics of fight songs would very depending
on whether a fight song is from a team in a southern vs. northern
athletic conference. We decided to look at the variables
`victory_win_won`, `men`, and `nonsense` because we thought that out of
all of the variables in our data set that these three might vary based
on geographical location.

However, based on the hypothesis testing above, it turns out that the
variables `victory_win_won` (p = 0.418), `men` (p = 0.454), and
`nonsense` (p = 0.648) are NOT regionally unique. In other words,
knowing whether a school is in a southern vs. northern conference does
not tell us whether that school’s fight song is more or less likely to
include the words “victory”, “win”, or “won”, mention male
individuals/groups, or use nonsense words/phrases. The data leads us to
believe that college fight songs are truly independent and random from
one another.