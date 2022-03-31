INFO 3010 - Assignment 6
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
4.  You can get 5 bonus points for using ggplot2 package.

### Q1. (5pts) Download the ‘HousePrice.csv’ dataset. Read the dataset to a dataframe. Check the structure of the dataframe.

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.2.0     v stringr 1.4.0
    ## v readr   2.1.2     v forcats 0.5.1

    ## Warning: package 'ggplot2' was built under R version 4.1.3

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
df=read.csv("HousePrice.csv")
str(df)
```

    ## 'data.frame':    6383 obs. of  30 variables:
    ##  $ Latitude         : num  47.5 47.5 47.5 47.5 47.5 ...
    ##  $ Longitude        : num  -122 -122 -122 -122 -122 ...
    ##  $ Bath             : num  1.5 3 3 2.75 3 3 1 2.25 2 1.75 ...
    ##  $ Bed              : chr  "4" "3" "6" "4" ...
    ##  $ Price            : int  287000 210000 975000 335000 372500 278000 169000 218400 230000 187500 ...
    ##  $ Postal           : int  98178 98178 98146 98178 98178 98178 98178 98178 98178 98178 ...
    ##  $ Crime            : int  220 219 181 236 235 252 239 258 258 260 ...
    ##  $ MedIncome        : int  60190 60190 53346 60190 60190 60190 60190 60190 60190 60190 ...
    ##  $ School           : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Sqft_Area        : int  2190 1790 2520 2960 2480 2540 890 1690 1910 1760 ...
    ##  $ Lot_Area         : num  11761 13504 54014 23087 10062 ...
    ##  $ SchoolDist       : num  0.887 0.712 0.723 0.981 0.812 ...
    ##  $ Property_Address : chr  "5364 S Juniper St" "5546 S Juniper St" "11825 Seola Beach Dr SW" "11836 54th Ave S" ...
    ##  $ Zestimate        : chr  "278K" "261K" "967K" "438K" ...
    ##  $ Date_Sold        : chr  "2/14/2014" "3/14/2014" "7/14/2014" "8/23/2013" ...
    ##  $ Locality         : chr  "Seattle" "Seattle" "Seattle" "Seattle" ...
    ##  $ Region           : chr  "WA" "WA" "WA" "WA" ...
    ##  $ Built_Year       : int  1955 1988 1954 1957 2004 1960 1957 1982 1951 1964 ...
    ##  $ Price_Sqft       : int  131 117 386 113 150 109 189 200 120 106 ...
    ##  $ Envi             : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ Age              : int  59 26 60 57 10 54 57 32 63 50 ...
    ##  $ TranEnvi         : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ TransDist        : num  Inf Inf Inf Inf Inf ...
    ##  $ SchoolType       : int  8 8 8 8 8 8 8 8 8 8 ...
    ##  $ SchoolTSRatio    : int  20 20 19 20 20 20 20 20 20 20 ...
    ##  $ SchoolRating     : int  3 3 4 3 3 3 3 3 3 3 ...
    ##  $ MedAge           : num  38 38 38.3 38 38 38 38 38 38 38 ...
    ##  $ Population       : int  21860 21860 25574 21860 21860 21860 21860 21860 21860 21860 ...
    ##  $ College.Graduates: num  23.1 23.1 20.6 23.1 23.1 ...
    ##  $ Rank             : int  7537 7537 9082 7537 7537 7537 7537 7537 7537 7537 ...

### Q2. (10pts) Draw a density plot and a histogram to check the distribution of house price. What’s the difference between the histogram and density plot?

``` r
hist(df$Price)
```

![](Assignment6-directoryID_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
d <- density(df$Price)
plot(d)
```

![](Assignment6-directoryID_files/figure-gfm/unnamed-chunk-2-2.png)<!-- -->

### Q3. 1) (5pts) Get a subset of data with postal code as “98178”,“98146” and “98118”. 2) (10pts) Draw a scatter plot with the subset data. It has Sqft_Area in x-axis, Price in y-axis. The dots are colored by postal code.

``` r
subset <- df[!is.na(df$Postal)&(df$Postal==98178)|(df$Postal==98146)|(df$Postal==98118),]
ggplot(data=subset)+
  geom_point(mapping = aes(x=Sqft_Area,y=Price),color=subset$Postal)
```

![](Assignment6-directoryID_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

### Q4. (10pts) Randomly choose a dataset you are interested. You may also keep using the hourseprice dataset. Draw a scatter plot matrix. What do you find? Write 2-3 sentences about your finding. Remember to write the sentences outside of code chunk.

``` r
ggplot(data=df)+
  geom_point(mapping = aes(x=Lot_Area,y=Price))
```

    ## Warning: Removed 82 rows containing missing values (geom_point).

![](Assignment6-directoryID_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#I was expecting that lot area would rise with price but, there are some properties with large lot area and low prices
```

### Q5. (10pts) Choose any three variables from Q4 and draw a plot to explore the relations of the three variables.

``` r
ggplot(df, aes( x = SchoolRating,y = Price, color = Age)) +
  geom_point()
```

    ## Warning: Removed 2589 rows containing missing values (geom_point).

![](Assignment6-directoryID_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
