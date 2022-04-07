INFO 3010 - Assignment 7
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

### Q1. (5pts) Read the file “2015.csv” to a dataframe. Have a statistical summary of the dataset.

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
df <-  read.csv("2015.csv")
summary(df)
```

    ##    Country             Region          Happiness_Rank   Happiness_Score
    ##  Length:158         Length:158         Min.   :  1.00   Min.   :2.839  
    ##  Class :character   Class :character   1st Qu.: 40.25   1st Qu.:4.526  
    ##  Mode  :character   Mode  :character   Median : 79.50   Median :5.232  
    ##                                        Mean   : 79.49   Mean   :5.376  
    ##                                        3rd Qu.:118.75   3rd Qu.:6.244  
    ##                                        Max.   :158.00   Max.   :7.587  
    ##  Lower_Confidence_Interval Upper_Confidence_Interval GDP_per_Capita  
    ##  Min.   :0.01848           Min.   :0.0000            Min.   :0.0000  
    ##  1st Qu.:0.03727           1st Qu.:0.5458            1st Qu.:0.8568  
    ##  Median :0.04394           Median :0.9102            Median :1.0295  
    ##  Mean   :0.04788           Mean   :0.8461            Mean   :0.9910  
    ##  3rd Qu.:0.05230           3rd Qu.:1.1584            3rd Qu.:1.2144  
    ##  Max.   :0.13693           Max.   :1.6904            Max.   :1.4022  
    ##      Family       Life_Expectancy     Freedom        Government_Corruption
    ##  Min.   :0.0000   Min.   :0.0000   Min.   :0.00000   Min.   :0.0000       
    ##  1st Qu.:0.4392   1st Qu.:0.3283   1st Qu.:0.06168   1st Qu.:0.1506       
    ##  Median :0.6967   Median :0.4355   Median :0.10722   Median :0.2161       
    ##  Mean   :0.6303   Mean   :0.4286   Mean   :0.14342   Mean   :0.2373       
    ##  3rd Qu.:0.8110   3rd Qu.:0.5491   3rd Qu.:0.18025   3rd Qu.:0.3099       
    ##  Max.   :1.0252   Max.   :0.6697   Max.   :0.55191   Max.   :0.7959       
    ##    Generosity    
    ##  Min.   :0.3286  
    ##  1st Qu.:1.7594  
    ##  Median :2.0954  
    ##  Mean   :2.0990  
    ##  3rd Qu.:2.4624  
    ##  Max.   :3.6021

### Q2. (5pts) Draw a plot with box plot of Happiness_Score for countries in each Region group. Write 2-3 sentences to describe your findings.

``` r
ggplot(df, aes(x = Region, y = Happiness_Score, fill = Region)) + 
  geom_boxplot() +
  stat_summary(fun = "mean", geom = "point", shape = 8,
               size = 10, color = "white")
```

![](Assignment7-WK0083_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

### Q3. (10pts) Draw a scatter plot matrix for variables: Happiness_Score, Family, Freedom, Government_Corruption. What is the relation between Happiness_Score and Government_Corruption? What is the relation between Happiness_Score and Freedom?

``` r
pairs(~Happiness_Score+ Family+Freedom+Government_Corruption,data = df,lowe.panel=panel.smooth)
```

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

    ## Warning in axis(side = side, at = at, labels = labels, ...): "lowe.panel" is not
    ## a graphical parameter

    ## Warning in plot.xy(xy.coords(x, y), type = type, ...): "lowe.panel" is not a
    ## graphical parameter

    ## Warning in plot.window(...): "lowe.panel" is not a graphical parameter

    ## Warning in plot.xy(xy, type, ...): "lowe.panel" is not a graphical parameter

    ## Warning in title(...): "lowe.panel" is not a graphical parameter

![](Assignment7-WK0083_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

### Q4. (10pts) Build a linear regression model to predict Life_Expectancy with Hapiness_Score, GDP_per_Capita, Family and Government_Corruption.

``` r
df.regression <- lm(Life_Expectancy~Happiness_Score+ GDP_per_Capita+ Family+ Government_Corruption,data=df)
summary(df.regression)
```

    ## 
    ## Call:
    ## lm(formula = Life_Expectancy ~ Happiness_Score + GDP_per_Capita + 
    ##     Family + Government_Corruption, data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.35494 -0.08702  0.00841  0.07806  0.30047 
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           -0.03073    0.04757  -0.646    0.519    
    ## Happiness_Score        0.06982    0.01520   4.594 9.01e-06 ***
    ## GDP_per_Capita         0.04067    0.05145   0.790    0.430    
    ## Family                -0.05690    0.05516  -1.032    0.304    
    ## Government_Corruption  0.33536    0.07560   4.436 1.74e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.1177 on 153 degrees of freedom
    ## Multiple R-squared:  0.4057, Adjusted R-squared:  0.3901 
    ## F-statistic: 26.11 on 4 and 153 DF,  p-value: < 2.2e-16

### Q5. (10pts) Check model details. Answer the following questions out of code box: What is the adjusted R-squared value? Is the linear relation significant?

``` r
#The adjusted R square value is 0.3901, There is 39% less variation around the regression line than the mean. GDP,Happiness Score, Family and Government Corruption accounts for 39% of the variation. 
```

### Q6. (10pts) Draw diagnostic plots for the model in Q6.

``` r
par(mfrow = c(2, 2))
plot(df.regression)
```

![](Assignment7-WK0083_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
