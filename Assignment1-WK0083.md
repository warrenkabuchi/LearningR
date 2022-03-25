INFO 3010 - Assignment 1
================

## by Lingzi Hong

### Instructions

1.  This is an R Markdown format used for publishing markdown documents
    to GitHub. When you click the **Knit** button all R code chunks are
    run and a markdown file (.md) suitable for publishing to GitHub is
    generated.
2.  Submit this downloaded completed R markdown file in the assignment
    on Canvas. Make sure when you save that you have run all cells, so
    the outputs displace between the cells.
3.  make sure to replace the directoryID in the filename with your
    student ID.

### Q1: Compute the following values. Write code in the below chunks. (5pts)

1.  Square root of 28 (2pts)  
a. sqrt(28) 


2.  Round to two decimal places for pi, i.e, 3.1415926… (3pts)

b. round(pi, 2)

### Q2: Create the following vectors. a = (5, 10, 15, 20, 70, 120), b = (-7, 6, 18, 3, 58, -56). (10pts)

1.  create a vector c, which is add results of vector a and b (3pts)
 a = c(5,10,15,20,70,120) b = c(-7,6,18,3,58,-56)
c =- a + b

2.  get the 3rd and 5th element of c (3pts)
c[ c(3,5) ]

3.  how many elements of c are greater than 30 (4pts)

length(c[c > 30])

### Q3: Create the following vectors with seq or rep (10pts)

1.  c(1,2,3,4,1,2,3,4) (5pts)
1. c = rep(seq(1, 4, by=1), times = 2)

2.  c(1,1,3,3,5,5,7,7) (5pts)
2. c = c(rep(1,2), rep(3,2), rep(5,2), rep(7,2))

### Q4: Create a vector which contains 10 random integer values between -20 and +30. (5pts)
v = sample(-20:30, 10, replace=TRUE)

### Q5: Get the maximum and minumum value of a vector c(3,-5,8,12,4,2,1,7). (5pts)

x= c(3,-5,8,12,4,2,1,7)
# find the minimum
min(x)
# find the maximum
max(x)


### Q6: Write a R program to print the numbers from 1 to 50 and print “Fizz” for multiples of 3, print “Buzz” for multiples of 5, and print “FizzBuzz” for multiples of both. (5pts)

for (x in 1:50) {
  if(x%%3==0){
    print(paste(x,' Fizz'))
  }
  if(x%%5==0){
    print(paste(x,' Buzz'))
  }
  if(x%%5==0 && x%%3==0){
    print(paste(x,' FizzBuzz'))
  }

### Q7. Write a R program to get the unique characters of the string “This is a challenging question”. (10pts)
str = "This is a challenging question."
print(unique(tolower(str)))

