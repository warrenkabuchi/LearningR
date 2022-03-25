INFO 3010 - Assignment 4
================

## by Lingzi Hong

### Instructions

1.  This is an R Markdown format used for publishing markdown documents
    to GitHub. When you click the **Knit** button, all R code chunks are
    run and a markdown file (.md) suitable for publishing to GitHub is
    generated.
2.  Please download the snowstorm.json from canvas. Fill in the code
    chunks for following question and submit this R markdown file to the
    assignment on Canvas. Make sure when you save that you have run all
    cells, so the outputs displace between the cells.
3.  make sure to replace the directoryID in the filename with your
    student ID.

### Q1. (5pts) Import the ‘snowstorm.json’ file to R

``` r
library('rjson')
library('jsonlite')
```

    ## 
    ## Attaching package: 'jsonlite'

    ## The following objects are masked from 'package:rjson':
    ## 
    ##     fromJSON, toJSON

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.2.0     v stringr 1.4.0
    ## v readr   2.1.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter()      masks stats::filter()
    ## x purrr::flatten()     masks jsonlite::flatten()
    ## x jsonlite::fromJSON() masks rjson::fromJSON()
    ## x dplyr::lag()         masks stats::lag()
    ## x jsonlite::toJSON()   masks rjson::toJSON()

``` r
json_file <- "snowstorm.json"
snowstorm <- stream_in(file(json_file))
```

    ## opening file input connection.

    ##  Found 300 records... Imported 300 records. Simplifying...

    ## closing file input connection.

### Q2. (5pts) How many records are there in the json file?

``` r
#head(snowstorm)
nrow(snowstorm)
```

    ## [1] 300

### Q3. (5pts) Extract information of ‘created_at’, ‘id_str’, ‘id’ of ‘user’, ‘name’ of ‘user’, ‘full_name’ of ‘place’, ‘place_type’ of ‘place’ from the json file and generate 6 vectors named as ‘create_at’, ‘tweet_id’, ‘user_id’,‘user_name’,‘place_name’,‘place_type’.

``` r
created_at <- snowstorm$created_at
tweet_id <- snowstorm$id_str
user_id <- snowstorm$user$id
user_name <- snowstorm$user$name
place_name <- snowstorm$place$full_name
place_type <- snowstorm$place$place_type
```

### Q4. (5pts) Generate a dataframe with the 6 vectors in Q3 and remove duplicated rows from the dataframe.

``` r
snowframe <- data.frame(created_at,tweet_id,user_name,user_id,place_name,place_type)
snowtibble <- as_tibble(snowframe)
snowtibble %>% distinct()
```

    ## # A tibble: 300 x 6
    ##    created_at                   tweet_id user_name user_id place_name place_type
    ##    <chr>                        <chr>    <chr>       <dbl> <chr>      <chr>     
    ##  1 Tue Aug 01 00:00:06 +0000 2~ 8921730~ "Miles T~  5.46e7 Southchas~ city      
    ##  2 Tue Aug 01 00:00:06 +0000 2~ 8921730~ "Florida~  4.60e8 Conway, FL city      
    ##  3 Tue Aug 01 00:00:16 +0000 2~ 8921730~ "Wendy S~  7.70e7 Birmingha~ city      
    ##  4 Tue Aug 01 00:00:27 +0000 2~ 8921731~ "Shelby ~  6.06e8 Auburn, AL city      
    ##  5 Tue Aug 01 00:00:34 +0000 2~ 8921731~ "\U0001f~  3.25e8 Gardendal~ city      
    ##  6 Tue Aug 01 00:00:43 +0000 2~ 8921732~ "Bree"     2.31e8 Pooler, GA city      
    ##  7 Tue Aug 01 00:00:55 +0000 2~ 8921732~ "Julie D~  1.03e8 <NA>       <NA>      
    ##  8 Tue Aug 01 00:00:56 +0000 2~ 8921732~ "Dana Ko~  2.32e7 Palm Beac~ city      
    ##  9 Tue Aug 01 00:00:06 +0000 2~ 8921730~ "Ben All~  2.46e7 <NA>       <NA>      
    ## 10 Tue Aug 01 00:00:09 +0000 2~ 8921730~ "Savage ~  1.01e8 Country C~ city      
    ## # ... with 290 more rows

``` r
snowtibble
```

    ## # A tibble: 300 x 6
    ##    created_at                   tweet_id user_name user_id place_name place_type
    ##    <chr>                        <chr>    <chr>       <dbl> <chr>      <chr>     
    ##  1 Tue Aug 01 00:00:06 +0000 2~ 8921730~ "Miles T~  5.46e7 Southchas~ city      
    ##  2 Tue Aug 01 00:00:06 +0000 2~ 8921730~ "Florida~  4.60e8 Conway, FL city      
    ##  3 Tue Aug 01 00:00:16 +0000 2~ 8921730~ "Wendy S~  7.70e7 Birmingha~ city      
    ##  4 Tue Aug 01 00:00:27 +0000 2~ 8921731~ "Shelby ~  6.06e8 Auburn, AL city      
    ##  5 Tue Aug 01 00:00:34 +0000 2~ 8921731~ "\U0001f~  3.25e8 Gardendal~ city      
    ##  6 Tue Aug 01 00:00:43 +0000 2~ 8921732~ "Bree"     2.31e8 Pooler, GA city      
    ##  7 Tue Aug 01 00:00:55 +0000 2~ 8921732~ "Julie D~  1.03e8 <NA>       <NA>      
    ##  8 Tue Aug 01 00:00:56 +0000 2~ 8921732~ "Dana Ko~  2.32e7 Palm Beac~ city      
    ##  9 Tue Aug 01 00:00:06 +0000 2~ 8921730~ "Ben All~  2.46e7 <NA>       <NA>      
    ## 10 Tue Aug 01 00:00:09 +0000 2~ 8921730~ "Savage ~  1.01e8 Country C~ city      
    ## # ... with 290 more rows

### Q5. (5pts) How many unique users in the dataframe?

``` r
length(unique(user_id))
```

    ## [1] 274

### Q6. (5pts) How many tweets have the full_name of place as “Charlotte, NC”?

``` r
char <- filter(snowtibble,place_name=='Charlotte, NC')
nrow(char)
```

    ## [1] 4

### Q7. (10pts) How many tweets do not have place information?

``` r
sum(is.na(snowtibble$place_name))
```

    ## [1] 48

### Q8. (10pts) How many unique places are the tweets from?

``` r
length(unique(place_type))
```

    ## [1] 5
