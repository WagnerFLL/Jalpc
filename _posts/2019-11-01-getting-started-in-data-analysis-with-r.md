---
layout: post
title:  "Getting started in Data Analysis with R"
date:   2019-11-01
desc: "A gentle introduction to Data Analysis using the second most requested programming language in Data Science"
keywords: "data analytics,beginners,introduction,R"
categories: [R]
tags: [R,Data Analytics]
icon: icon-html
---

When we talk about Data Science R is the second most used programming language. It was developed in 1993 and is now widely used in data science applications as it provides support for a wide range of inference techniques, machine learning algorithms, time series analysis, data analysis and graph plotting. R was structured to reflect the way statisticians think and work, with the main focus being data manipulation. In this post, we’ll look at some basic language commands and libraries to begin exploring and manipulating data.

### Loading the Data
Data sets can be internal or loaded from external sources in R. So there are several ways to load data, here we will work with a database available in CSV from an external source.
<br>
To load the dataset into a given directory and store it in a variable the following command is used:

```R
path <- "/Users/USER_NAME/.../Datasets/"

hsb2 = read.csv(paste(path,"HSB2.csv",sep=""))
```

HSB2.csv is the name of the file containing the dataset that will be stored in the hsb2 variable after the command is executed. There are still some optional parameters in the <code>read.csv</code> method, to understand it more thoroughly go [here](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/read.table.html).

### Data Exploration
Once the data has been loaded into the workspace, it is time to explore it to get an idea about its structure.
* **str**(hsb2)
It displays the internal structure of an R object and gives a quick overview of the rows and columns of the dataset.
```
'data.frame':	200 obs. of  11 variables:
 $ id     : int  70 121 86 141 172 113 50 11 84 48 ...
 $ gender : Factor w/ 2 levels "female","male": 2 1 2 2 2 2 2 2 2 2 ...
 $ race   : Factor w/ 4 levels "african american",..: 4 4 4 4 4 4 1 3 4 1 ...
 $ ses    : Factor w/ 3 levels "high","low","middle": 2 3 1 1 3 3 3 3 3 3 ...
 $ schtyp : Factor w/ 2 levels "private","public": 2 2 2 2 2 2 2 2 2 2 ...
 $ prog   : Factor w/ 3 levels "academic","general",..: 2 3 2 3 1 1 2 1 2 1 ...
 $ read   : int  57 68 44 63 47 44 50 34 63 57 ...
 $ write  : int  52 59 33 44 52 52 59 46 57 55 ...
 $ math   : int  41 53 54 47 57 51 42 45 54 52 ...
 $ science: int  47 63 58 53 53 63 53 39 58 50 ...
 $ socst  : int  57 61 31 56 61 61 61 36 51 51 ...
```
* **glimpse**(hsb2)
This is like a transposed version of print: columns run down the page, and data runs across. This makes it possible to see every column in a data frame. It's a little like str applied to a data frame but it tries to show you as much data as possible. (And it always shows the underlying data, even when applied to a remote data source)
```
Observations: 200
Variables: 11
$ id      <int> 70, 121, 86, 141, 172, 113, 50, 11, 84, 48, 75, 60, 95, 104...
$ gender  <fct> male, female, male, male, male, male, male, male, male, mal...
$ race    <fct> white, white, white, white, white, white, african american,...
$ ses     <fct> low, middle, high, high, middle, middle, middle, middle, mi...
$ schtyp  <fct> public, public, public, public, public, public, public, pub...
$ prog    <fct> general, vocational, general, vocational, academic, academi...
$ read    <int> 57, 68, 44, 63, 47, 44, 50, 34, 63, 57, 60, 57, 73, 54, 45,...
$ write   <int> 52, 59, 33, 44, 52, 52, 59, 46, 57, 55, 46, 65, 60, 63, 57,...
$ math    <int> 41, 53, 54, 47, 57, 51, 42, 45, 54, 52, 51, 51, 71, 57, 50,...
$ science <int> 47, 63, 58, 53, 53, 63, 53, 39, 58, 50, 53, 63, 61, 55, 31,...
$ socst   <int> 57, 61, 31, 56, 61, 61, 61, 36, 51, 51, 61, 61, 71, 46, 56,...
```

* **table**(hsb2$schtyp)
It performs categorical tabulation of the data with the variable and its frequency. The function is also very useful in creating frequency tables with condition and cross tabs.
```
private  public 
     32     168
```

### Data Manipulation
R provides many different alternatives for a manipulation process. What is shown here can be done in a few different ways, but initially it is the simplest way to understand how we can prepare the data
#### Filter
Filter is used to find samples where conditions are true. Then lines where the condition is not met are discarded.
```R
hsb2_public <- filter(hsb2, schtyp == "public")

table(hsb2_public$schtyp)
>private  public 
>      0     168 

str(hsb2_public$schtyp)
> Factor w/ 2 levels "private","public": 2 2 2 2 2 2 2 2 2 2 ...
```
#### Droplevels
The droplevels R function removes unused levels of a factor.
```R
hsb2_public$schtyp <- droplevels(hsb2_public$schtyp)

str(hsb2_public$schtyp)
> Factor w/ 1 level "public": 1 1 1 1 1 1 1 1 1 1 ...
```

### Basic Plot 
```R
hsb2_private <- hsb2 %>% filter(schtyp == "private")

ggplot(data = hsb2, aes(x = science, y = math, color = prog)) + geom_point()
```
<img src="{{ site.img_path }}/R/plot.jpeg" width="75%">