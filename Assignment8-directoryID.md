INFO 3010 - Assignment8
================

## by Lingzi Hong

### Instructions

1.  This is an R Markdown format used for publishing markdown documents
    to GitHub. When you click the **Knit** button, all R code chunks are
    run and a markdown file (.md) suitable for publishing to GitHub is
    generated.
2.  Fill in the code chunks for following question and submit this R
    markdown file to the assignment on Canvas. Make sure when you save
    that you have run all cells, so the outputs displace between the
    cells.
3.  make sure to replace the directoryID in the filename with your
    student ID.
4.  Solo work, open book & open notes - This part of the exam is open
    book, open notes. You may use the textbook, your own notes, and any
    pre-existing resources. You must not use communicate with others -
    in person, using online forums, social media, Slack, WeChat,
    GroupMe, etc. If you have concerns, contact me directly so I can
    advise you.
5.  Partial credit - If you can’t get your code to work 100% correctly,
    you can often earn partial credit. To get partial credit you need to
    show that you know there is a problem and describe it. The better
    you can describe the problem, the more partial credit you can earn,
    because it helps demonstrate your knowledge and skill.

### Q1. (5pts) Read the file ‘2016.csv’ to a dataframe. Check the first 5 lines of the data.

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
df <- read.csv("2016.csv")
head(df)
```

    ##       Country         Region Happiness_Rank Happiness_Score
    ## 1     Denmark Western Europe              1           7.526
    ## 2 Switzerland Western Europe              2           7.509
    ## 3     Iceland Western Europe              3           7.501
    ## 4      Norway Western Europe              4           7.498
    ## 5     Finland Western Europe              5           7.413
    ## 6      Canada  North America              6           7.404
    ##   Lower_Confidence_Interval Upper_Confidence_Interval GDP_per_Capita  Family
    ## 1                     7.460                     7.592        1.44178 1.16374
    ## 2                     7.428                     7.590        1.52733 1.14524
    ## 3                     7.333                     7.669        1.42666 1.18326
    ## 4                     7.421                     7.575        1.57744 1.12690
    ## 5                     7.351                     7.475        1.40598 1.13464
    ## 6                     7.335                     7.473        1.44015 1.09610
    ##   Life_Expectancy Freedom Government_Corruption Generosity Dystopia_Residual
    ## 1         0.79504 0.57941               0.44453    0.36171           2.73939
    ## 2         0.86303 0.58557               0.41203    0.28083           2.69463
    ## 3         0.86733 0.56624               0.14975    0.47678           2.83137
    ## 4         0.79579 0.59609               0.35776    0.37895           2.66465
    ## 5         0.81091 0.57104               0.41004    0.25492           2.82596
    ## 6         0.82760 0.57370               0.31329    0.44834           2.70485

### Q2. (5pts) Use the dataframe in Q1. Compute the average Hapiness_Score for countries in North America.

``` r
N_america <- filter(df,Region=="North America")
mean(N_america$Happiness_Score)
```

    ## [1] 7.254

### Q3. (5pts) Use the dataframe in Q1. Write code to compute the difference in Generosity between United States and Mexico.

``` r
unitedStates <- filter(df,Country =="United States")
mexico <- filter(df,Country=="Mexico")
unitedStates$Generosity-mexico$Generosity
```

    ## [1] 0.29342

### Q4. (5pts) Use the dataframe in Q1. Plot the dataframe columns Happiness_Rank and Happiness_Score as a line graph

``` r
library(ggplot2)
ggplot(df,aes(x=Happiness_Rank,y=Happiness_Score))+geom_line()
```

![](Assignment8-directoryID_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

### Q5. (5pts) Use the dataframe in Q1. Plot the scatter plot matrix for the four variables: Family, Life_Expectancy,Freedom, Generosity

``` r
variables <- select(df,Family,Life_Expectancy,Freedom,Generosity)
pairs(variables)
```

![](Assignment8-directoryID_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### Q6. (5pts) Get a subset of data with countries from Region of “Western Europe” and “North America” and “Eastern Asia”.

``` r
WE <- subset(df,Region=="Western Europe")
EA <- subset(df,Region=="Eastern Asia")
subset <- rbind(WE,EA,N_america)
```

### Q7. (5pts) Use the subset data in Q6. Draw a scatter plot with Government_Corruption in the x-axis, Freedom in the y-axis, and the points are colored by region.

``` r
ggplot(subset,aes(x=Government_Corruption,y=Freedom,color=Region))+geom_point()
```

![](Assignment8-directoryID_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

### Q8. (5pts) Build a linear regression model: predict Happiness_Score with variables GDP_per_Capita, Family, Life_Expectancy and Freedom.

``` r
sample_size <- floor(0.8*nrow(df))
train_ind <- sample(seq_len(nrow(df)),size=sample_size)
train <- df[train_ind,]
test <- df[-train_ind,]
lm.fit <- lm(Happiness_Score~ GDP_per_Capita+ Family+ Life_Expectancy + Freedom,data=df)
```

### Q9. (10pts) Get a summary of the model. Answer the following questions out of the code chunk: what is the adjusted R-squared? What is the coefficient of GDP_per_Capita?

``` r
summary(lm.fit)
```

    ## 
    ## Call:
    ## lm(formula = Happiness_Score ~ GDP_per_Capita + Family + Life_Expectancy + 
    ##     Freedom, data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.46090 -0.27833 -0.00541  0.38392  1.58309 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       2.2068     0.1518  14.535  < 2e-16 ***
    ## GDP_per_Capita    0.7615     0.2095   3.634 0.000381 ***
    ## Family            1.1730     0.2296   5.109 9.59e-07 ***
    ## Life_Expectancy   1.4451     0.3468   4.167 5.15e-05 ***
    ## Freedom           1.9200     0.3355   5.723 5.41e-08 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5413 on 152 degrees of freedom
    ## Multiple R-squared:  0.781,  Adjusted R-squared:  0.7752 
    ## F-statistic: 135.5 on 4 and 152 DF,  p-value: < 2.2e-16

``` r
#Adjusted Rsquare is 0.7752
#Coefficient of GDP per capita 0.7615
```
