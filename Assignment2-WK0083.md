INFO 3010 - Assignment 2
================

## by Lingzi Hong

### Instructions

1.  This is an R Markdown format used for publishing markdown documents
    to GitHub. When you click the **Knit** button, all R code chunks are
    run and a markdown file (.md) suitable for publishing to GitHub is
    generated.
2.  Fill in the code chunks and submit this R markdown file to the
    assignment on Canvas. Make sure when you save that you have run all
    cells, so the outputs displace between the cells.
3.  make sure to replace the directoryID in the filename with your
    student ID.

### Q1 (5pts): Download the file ‘Adults_Cleaned.csv’ from canvas. Read the file into a dataframe.

``` r
adults=read.table(file.choose(),sep = ",",header = TRUE)
```

### Q2: (5pts) Check the data type of each column.

``` r
str(adults)
```

    ## 'data.frame':    32561 obs. of  15 variables:
    ##  $ age           : int  39 50 38 53 28 37 49 52 31 42 ...
    ##  $ workclass     : chr  "State-gov" "Self-emp-not-inc" "Private" "Private" ...
    ##  $ fnlwgt        : int  77516 83311 215646 234721 338409 284582 160187 209642 45781 159449 ...
    ##  $ education     : chr  "Bachelors" "Bachelors" "HS-grad" "11th" ...
    ##  $ education.num : int  13 13 9 7 13 14 5 9 14 13 ...
    ##  $ marital.status: chr  "Never-married" "Married-civ-spouse" "Divorced" "Married-civ-spouse" ...
    ##  $ occupation    : chr  "Adm-clerical" "Exec-managerial" "Handlers-cleaners" "Handlers-cleaners" ...
    ##  $ relationship  : chr  "Not-in-family" "Husband" "Not-in-family" "Husband" ...
    ##  $ race          : chr  "White" "White" "White" "Black" ...
    ##  $ sex           : chr  "Male" "Male" "Male" "Male" ...
    ##  $ capital.gain  : int  2174 0 0 0 0 0 0 0 14084 5178 ...
    ##  $ capital.loss  : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ hours.per.week: int  40 13 40 40 40 40 16 45 50 40 ...
    ##  $ native.country: chr  "United-States" "United-States" "United-States" "United-States" ...
    ##  $ label         : chr  "<=50K" "<=50K" "<=50K" "<=50K" ...

### Q3: (5pts) How many kinds of occupations are in the dataset?

``` r
x= unique(adults$occupation)
length(x)
```

    ## [1] 15

### Q4: (5pts) Extract 3rd and 5th rows with 1st and 3rd columns from the data frame.

``` r
result =  adults[c(3,5),c(1,3)]
```

### Q5: (5pts) How many people work more than 40hrs a week?

``` r
nrow(adults[adults$hours.per.week>40,])
```

    ## [1] 9581

### Q6: (5pts) Number of people whose native country is Mexico and work more than 40hrs a week.

``` r
nrow(adults[adults$native.country=="Mexico"&adults$hours.per.week>'40',])
```

    ## [1] 133

### Q7: (5pt) For people who have got Bachelors degree, what the is percentage to have capital.gain more than 1000?

``` r
bachelors= nrow(adults[adults$education.num==13,])
capgain = nrow(adults[adults$capital.gain>"1000",])
(capgain/bachelors)*100
```

    ## [1] 50.64426

### Q8: (5pts) What is the average woking hours per week for people who are husband?

``` r
husband = nrow(adults[adults$marital.status=="Husband",])
husband
```

    ## [1] 0

### Q9: (5pts) Add a new column to the dataframe, with column name ‘health.insurance’ and all values as 0.

``` r
adults["health.insurance"]=0
head(adults)
```

    ##   age        workclass fnlwgt education education.num     marital.status
    ## 1  39        State-gov  77516 Bachelors            13      Never-married
    ## 2  50 Self-emp-not-inc  83311 Bachelors            13 Married-civ-spouse
    ## 3  38          Private 215646   HS-grad             9           Divorced
    ## 4  53          Private 234721      11th             7 Married-civ-spouse
    ## 5  28          Private 338409 Bachelors            13 Married-civ-spouse
    ## 6  37          Private 284582   Masters            14 Married-civ-spouse
    ##          occupation  relationship  race    sex capital.gain capital.loss
    ## 1      Adm-clerical Not-in-family White   Male         2174            0
    ## 2   Exec-managerial       Husband White   Male            0            0
    ## 3 Handlers-cleaners Not-in-family White   Male            0            0
    ## 4 Handlers-cleaners       Husband Black   Male            0            0
    ## 5    Prof-specialty          Wife Black Female            0            0
    ## 6   Exec-managerial          Wife White Female            0            0
    ##   hours.per.week native.country label health.insurance
    ## 1             40  United-States <=50K                0
    ## 2             13  United-States <=50K                0
    ## 3             40  United-States <=50K                0
    ## 4             40  United-States <=50K                0
    ## 5             40           Cuba <=50K                0
    ## 6             40  United-States <=50K                0

### Q10: (5pts) Drop column ‘education.num’ and ‘label’ from the data frame.

``` r
newdf= subset(adults,select = -c(education.num,label))
head(newdf)
```

    ##   age        workclass fnlwgt education     marital.status        occupation
    ## 1  39        State-gov  77516 Bachelors      Never-married      Adm-clerical
    ## 2  50 Self-emp-not-inc  83311 Bachelors Married-civ-spouse   Exec-managerial
    ## 3  38          Private 215646   HS-grad           Divorced Handlers-cleaners
    ## 4  53          Private 234721      11th Married-civ-spouse Handlers-cleaners
    ## 5  28          Private 338409 Bachelors Married-civ-spouse    Prof-specialty
    ## 6  37          Private 284582   Masters Married-civ-spouse   Exec-managerial
    ##    relationship  race    sex capital.gain capital.loss hours.per.week
    ## 1 Not-in-family White   Male         2174            0             40
    ## 2       Husband White   Male            0            0             13
    ## 3 Not-in-family White   Male            0            0             40
    ## 4       Husband Black   Male            0            0             40
    ## 5          Wife Black Female            0            0             40
    ## 6          Wife White Female            0            0             40
    ##   native.country health.insurance
    ## 1  United-States                0
    ## 2  United-States                0
    ## 3  United-States                0
    ## 4  United-States                0
    ## 5           Cuba                0
    ## 6  United-States                0
