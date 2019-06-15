Data Wrangling Exercise 1: Basic Data Manipulation
================
Mohammed Ali; Marwa Mohamed
6/2/2019

## Overview

In this exercise, you’ll work with a toy data set showing product
purchases from an electronics store. While the data set is small and
simple, it still illustrates many of the challenges you have to address
in real-world data wrangling\! The data set and exercise are inspired by
this [blog
post](http://d3-media.blogspot.nl/2013/11/how-to-refine-your-data.html).

## Getting started

The data is in an Excel file here called `refine.xlsx`. Right away,
you’ll notice that the data set has a few issues:

  - There are four brands: *Philips*, *Akzo*, *Van Houten* and
    *Unilever*. However, there are many different spellings and
    capitalizations of those names\!

  - The product code and number are combined in one column, separated by
    a hyphen.

## Exercise

Using `R`, clean this data set to make it easier to visualize and
analyze. Specifically, these are the tasks you need to do:

### 0: Load the data in RStudio

Save the data set as a CSV file called `refine_original.csv` and load it
in RStudio into a data frame.

``` r
refine <- read.csv("refine_original.csv")
```

### 1: Clean up brand names

Clean up the `company` column so all of the misspellings of the brand
names are standardized. For example, you can transform the values in the
column to be: *philips, akzo, van houten* and *unileve*r (all
lowercase).

``` r
#Search for Approximate String Matching then replace the found ids with correct string
idx <- agrep(pattern = "philips", x = refine$company, ignore.case = TRUE, value =   FALSE, max.distance = 3)
 refine$company[idx] <- "philips"
 idx <- agrep(pattern = "akzo", x = refine$company, ignore.case = TRUE, value = FALSE, max.distance = 2)
 refine$company[idx] <- "akzo"
 idx <- agrep(pattern = "van houten", x = refine$company, ignore.case = TRUE, value = FALSE, max.distance = 3)
 refine$company[idx] <- "van houten"
 idx <- agrep(pattern = "unilever", x = refine$company, ignore.case = TRUE, value = FALSE, max.distance = 3)
 refine$company[idx] <- "unilever"
 #drop unused levels
 refine$company <- droplevels(refine$company)
```

### 2: Separate product code and number

Separate the product code and product number into separate columns
i.e. add two new columns called `product_code` and `product_number`,
containing the product code and number respectively

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
refine <- refine %>% separate( Product.code...number, c("product_code", "product_number"), sep = "-")
```

### 3: Add product categories

You learn that the product codes actually represent the following
product categories:

  - `p` = **Smartphone**

  - `v` = **TV**

  - `x` = **Laptop**

  - `q` = **Tablet**

In order to make the data more readable, add a column with the product
category for each
record.

``` r
# x1 <- refine %>% filter(product_code == 'p') %>%  mutate(product_category = "Smartphone")
# x2 <- refine %>% filter(product_code == 'v') %>%  mutate(product_category = "TV" )
# x3 <- refine %>% filter(product_code == 'x') %>%  mutate(product_category = "Laptop")
# x4 <- refine %>% filter(product_code == 'q') %>%  mutate(product_category = "Tablet" )
#refine <- rbind(x1, x2, x3, x4)

refine$product_category <- ifelse(refine$product_code == 'p', "Smartphone", ifelse(refine$product_code == 'v', "TV", if_else(refine$product_code == 'x', "Laptop", "Tablet")))
```

### 4: Add full address for geocoding

You’d like to view the customer information on a map. In order to do
that, the addresses need to be in a form that can be easily geocoded.
Create a new column `full_address` that concatenates the three address
fields (`address`, `city`, `country`), separated by
*commas*.

``` r
refine <- refine %>% unite("full_address", address, city, country, sep = ",")
```

### 5: Create dummy variables for company and product category

Both the company name and product category are categorical variables
i.e. they take only a fixed set of values. In order to use them in
further analysis you need to create dummy variables. Create dummy binary
variables for each of them with the prefix `company_` and `product_`
i.e.,

  - Add four binary (1 or 0) columns for company: company\_philips,
    company\_akzo, company\_van\_houten and company\_unilever.

  - Add four binary (1 or 0) columns for product category:
    product\_smartphone, product\_tv, product\_laptop and
    product\_tablet.

<!-- end list -->

``` r
library(dummies)
```

    ## dummies-1.5.6 provided by Decision Patterns

``` r
names(refine)[6] <- "product"
refine <- refine %>% dummy.data.frame(names = c("company", "product"), sep = '_')
```