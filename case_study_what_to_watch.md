Case Study ‘What to watch?’
================
Luis Covarrubias
2025-09-12

According to multiple keyword-analysis reports for 2025, the most
Googled question globally is “What to watch?”

To answer this question we will refer to the authority in movie ratings,
IMDb the Internet Movie Database and make a quick analysis of two of
their non-commercial datasets.

IMDb is a omprehensive online database offering detailed information
about films, television series, video games, and streaming content,
including cast, plot summaries, trivia, ratings and reviews.

According to IMDb, the datasets we are going to use for this analysis
are updated daily, please keep in mind that we downloaded these datasets
in 2025-9-11, so you can expect to get enterely different results while
reproducing this analysis in your own computer.

### NOTE:

The first steps in this analysis will be performed in RStudio Desktop.
This is becuase one of the datasets is too large for online analysis,
about 1gb. After step 5 we will continue the analysis in this Rmd file
usig a smaller database called ‘movie_data_2025.csv’.

### Step 1: Download the data

Go to the IMDb Non-Commercial Datasets website

Link: <https://datasets.imdbws.com/>

For this analys we are only downloading two datasets: *title.basics.tsv
*title.ratings.tsv

It might take a couple of minutes becuase one of those two is large,
about 1gb. Once downloaded, decompress both datasets and save in a local
folder easy to reference.

### Step 2: Setup environment

We’ll start by opening RStudio Desktop and install and load the
‘tidyverse’ package. In the R Console enter the series of commands below
and hit Enter after every command

#### To install:

> install.packages(“tidyverse”)

#### To load:

> library(tidyverse)

### Step 3: Load the data

In the R Console enter the series of commands below and hit Enter after
every command, note that you will need to edit the location of the file
to the location where you saved your .tsv files

#### To load title.basics.tsv

> titles \<- read_tsv(“your\folder\title.basics.tsv”)

#### To load title.ratings.tsv

> ratings \<- read_tsv(“your\folder\title.ratings.tsv”)

Now you have both dataframes in your console ‘titles’ and ‘ratings’, you
could use the head() function to make sure each dataset was loaded
successfully.

### Step 3: Merge the dataframes

To answer the question ‘What to watch?’ we will use two metrics, rating
and votes, these qualities are contained in the ratings dataset, but to
see the actual name of a certain movie and to be able to filter out
boring documentaries and such, we need to merge the ratings data with
the titles data.

If you used the head() function to preview both dataframes earlier you
might have noticed that the primary key in both datasets is the variable
‘tconst’.

In the R Console enter the series of commands below and hit Enter after
every command:

#### To join both dataframes

> titles_ratings \<- titles %\>% inner_join(ratings, by = “tconst”)

#### To preview new dataframe

> head(titles_ratings)

The new dataframe must contain all columns in the titles dataframe,
plus, the ratings and votes for each title at the most right columns

### Step 3: Make it smaller

As mentioned earlier the annoying reason why we are making this analysis
in RStudio Desktop to then continue in this Rmd file is becuase the data
is too large. And one of the reasons for that is becuase it contains
decades worth of movie meta data. Lets pull a subset of data containing
only data for the current year, 2025.

To do this in the R Console enter the series of commands below and hit
Enter after every command:

#### To filter the data

> movie_data_2025 \<- titles_ratings %\>% filter(startYear == 2025)

#### To preview new data frame

> head(movie_data_2025)

The resulting dataset is much more smaller than the data we originally
downloaded.

### Step 4: Save the data

To save the data we’ll create a CSV file and save it locally. In the R
Console enter the series of commands below and hit Enter after every
command:

#### To save the file

> write_csv(movie_data_2025, “your/folder/movie_data_2025.csv”)

The resulting file is small enough to continue our analysis in any
spreadsheet app, but I imported it here so we can continue without
leaving this Rmd file. You are welcome!

### Step 5: Move to posit.cloud

Now that we have our data chunk ready, lets continue our analysis here
in Posit. Setup the environment by executing the code chunks below.

install tidyverse

``` r
install.packages('tidyverse')
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.5'
    ## (as 'lib' is unspecified)

load tidyverse

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.2
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

Nice.

### Step 6: Load the dataset

This is where we load the data chunk we created and saved in CSV format,
titled ‘movie_data_2025.csv’, we’ll also create an object here and call
it movies_2025

``` r
movies_2025_raw <- read_csv("/cloud/project/case_study/movie_data_2025.csv")
```

    ## Rows: 31352 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (7): tconst, titleType, primaryTitle, originalTitle, endYear, runtimeMin...
    ## dbl (4): isAdult, startYear, averageRating, numVotes
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

A little bit of data cleaning

``` r
movies_2025 <- movies_2025_raw %>%
  mutate(runtimeMinutes = as.integer(runtimeMinutes))
```

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `runtimeMinutes = as.integer(runtimeMinutes)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion

### Step 7: Add a few filters

For this analysis we only want to focus on movies, we don’t really have
time to watch entire series, we are too busy codding and loving so it
will only be the most rated movies at this time.

In the code chunk below we will also sort the data by most voted and
higer rated. This will hopefully unearth some modern movie gems to watch
with friends.

``` r
movies_2025 %>%
  filter(titleType == "movie") %>%
  arrange(desc(numVotes), desc(averageRating))
```

    ## # A tibble: 5,278 × 11
    ##    tconst     titleType primaryTitle     originalTitle isAdult startYear endYear
    ##    <chr>      <chr>     <chr>            <chr>           <dbl>     <dbl> <chr>  
    ##  1 tt6208148  movie     Snow White       Snow White          0      2025 "\\N"  
    ##  2 tt5950044  movie     Superman         Superman            0      2025 "\\N"  
    ##  3 tt31193180 movie     Sinners          Sinners             0      2025 "\\N"  
    ##  4 tt20969586 movie     Thunderbolts*    Thunderbolts*       0      2025 "\\N"  
    ##  5 tt16311594 movie     F1: The Movie    F1                  0      2025 "\\N"  
    ##  6 tt12299608 movie     Mickey 17        Mickey 17           0      2025 "\\N"  
    ##  7 tt14513804 movie     Captain America… Captain Amer…       0      2025 "\\N"  
    ##  8 tt9603208  movie     Mission: Imposs… Mission: Imp…       0      2025 "\\N"  
    ##  9 tt13654226 movie     The Gorge        The Gorge           0      2025 "\\N"  
    ## 10 tt26584495 movie     Companion        Companion           0      2025 "\\N"  
    ## # ℹ 5,268 more rows
    ## # ℹ 4 more variables: runtimeMinutes <int>, genres <chr>, averageRating <dbl>,
    ## #   numVotes <dbl>

What a surprise! Snow white as most voted?

### Takeaway:

## Number of Votes is not the best metric for this analysis

### Step 8: Do it right this time

This time around we will use rating as our main metric and votes as our
secondary metric. Let’s run the code below one more time, but this time
we’ll sort by averageRating first.

Filtering results by runtime over 99min and votes over 100,000 would be
helpful too.

We’ll also add a limiter so we don’t get too much homework

``` r
movies_2025 %>%
  filter(titleType == "movie") %>%
  filter(numVotes > 100000) %>%
  filter(runtimeMinutes > 99) %>%
  arrange(desc(averageRating), desc(numVotes)) %>%
  slice_head(n = 5)
```

    ## # A tibble: 5 × 11
    ##   tconst     titleType primaryTitle      originalTitle isAdult startYear endYear
    ##   <chr>      <chr>     <chr>             <chr>           <dbl>     <dbl> <chr>  
    ## 1 tt16311594 movie     F1: The Movie     F1                  0      2025 "\\N"  
    ## 2 tt26581740 movie     Weapons           Weapons             0      2025 "\\N"  
    ## 3 tt31193180 movie     Sinners           Sinners             0      2025 "\\N"  
    ## 4 tt9603208  movie     Mission: Impossi… Mission: Imp…       0      2025 "\\N"  
    ## 5 tt10676052 movie     The Fantastic Fo… The Fantasti…       0      2025 "\\N"  
    ## # ℹ 4 more variables: runtimeMinutes <int>, genres <chr>, averageRating <dbl>,
    ## #   numVotes <dbl>

Yup that’s more like it. Lets make it an objet so we can make a viz

``` r
movie_data_viz <- movies_2025 %>%
  filter(titleType == "movie") %>%
  filter(numVotes > 100000) %>%
  filter(runtimeMinutes > 99) %>%
  arrange(desc(averageRating), desc(numVotes)) %>%
  slice_head(n = 5)
```

We have a list of 5 movies with ratings ranging from 7.3 to 7.8. Note
that if we wanted to see higher ranked movies in our results we would
need to lower the min votes requirement. For me rating 7 is acceptable.

We’ll need scales

``` r
library(scales)
```

    ## 
    ## Attaching package: 'scales'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     discard

    ## The following object is masked from 'package:readr':
    ## 
    ##     col_factor

Here is a viz comparing Votes and Rating, goldbars

The further the bar, the more votes, the darker the gold, the higher
rated

``` r
ggplot(movie_data_viz, aes(x = reorder(primaryTitle, numVotes), 
                            y = numVotes, 
                            fill = averageRating)) +
  geom_col() +
  scale_fill_gradient(low = "#FFD700", high = "#B8860B") +  # light to dark gold
  coord_flip() +  # horizontal bars for better readability
  scale_y_continuous(labels = comma) +  # add commas to numVotes
  labs(title = "Top Movies of 2025",
       x = " ",
       y = "Number of Votes",
       fill = "Average Rating") +
  theme_minimal() +
  theme(legend.position = "right")
```

![](case_study_what_to_watch_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

### Takeaways

- **F1** is the higher rated and one of the most voted so defenitly a
  must watch
- By the metrics rating and votes, the **mission impossible** franchise
  did a bit better than the **marvel** franchise this summer

Let me know what you think
