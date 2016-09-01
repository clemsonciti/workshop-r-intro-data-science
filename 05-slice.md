
---
layout: lesson
title: Introduction to Data Science using R
subtitle: Addressing Data
minutes: 30
---

> ## Learning objectives {.objectives}
> * Understand the three different ways R can address data inside a data frame
> * Combine different methods for addressing data with the assignment operator to update subsets of data

R is a powerful language for data manipulation.
There are three main ways for addressing data inside R objects.

* By index (slicing)
* By logical vector
* By name (columns only)

Lets start by loading some sample data:


```R
setwd("/home/lngo/intro-data-science/")
dat <- read.csv(file = 'data/sample.csv', header = TRUE, stringsAsFactors = FALSE)
```

> ## Interpreting Rows as Headers
>
> The first row of this csv file is a list of column names.
> We used the *header=TRUE* argument to `read.csv` so that R can interpret the file correctly.
> We are using the *stringsAsFactors=FALSE* argument to override the default behaviour for R.
> Using factors in R is covered in a separate lesson.

Lets take a look at this data.


```R
class(dat)
```

R has loaded the contents of the .csv file into a variable called `dat` which is a `data frame`.


```R
dim(dat)
```

The data has 100 rows and 9 columns.


```R
head(dat)
```

The data is the results of an (not real) experiment, looking at the number of aneurysms that formed in the eyes of patients who undertook 3 different treatments.

### Addressing by Index

Data can be accessed by index. We have already seen how square brackets `[` can be used to subset (slice) data. The generic format is `dat[row_numbers,column_numbers]`.


> ## Selecting Values
>
> What will be returned by `dat[1,1]`?

If we leave out a dimension R will interpret this as a request for all values in that dimension.

> ## Selecting More Values
>
> What will be returned by `dat[,2]`?

The colon `:` can be used to create a sequence of integers.


```R
6:9
```

This creates a vector of numbers from 6 to 9.

This can be very useful for addressing data.

> ## Subsetting with Sequences
>
> Use the colon operator to index just the aneurism count data (columns 6 to 9).

Finally we can use the `c()` (combine) function to address non-sequential rows and columns.


```R
dat[c(1,5,7,9), 1:5]
```

Returns the first 5 columns for patients in rows 1,5,7 & 9

> ## Subsetting Non-Sequential Data
>
> Return the age and gender values for the first 5 patients.

### Addressing by Name

Columns in an R data frame are named.


```R
names(dat)
```

> ## Default Names
>
> If names are not specified e.g. using `headers=FALSE` in a `read.csv()` function, R assigns default names `V1,V2,...,Vn`

We usually use the `$` operator to address a column by name


```R
dat$Gender
```

Named addressing can also be used in square brackets.


```R
head(dat[,c('Age', 'Gender')])
```

> ## Best Practice
>
> Best practice is to address columns by name, often you will create or delete columns and the column position will change.

### Logical Indexing

A logical vector contains only the special values `TRUE` & `FALSE`.

c(TRUE, TRUE, FALSE, FALSE, TRUE)

> ## Truth and Its Opposite
>
> Note the values `TRUE` and `FALSE` are all capital letters and are not quoted.
{: .callout}

Logical vectors can be created using `relational operators` e.g. `<, >, ==, !=, %in%`.


```R
x <- c(1, 2, 3, 11, 12, 13)
x < 10
```


```R
x %in% 1:10
```

We can use logical vectors to select data from a data frame.


```R
index <- dat$Group == 'Control'
dat[index,]$BloodPressure
```

Often this operation is written as one line of code:


```R
plot(dat[dat$Group == 'Control',]$BloodPressure)
```

> ## Using Logical Indexes
>
> 1. Create a scatterplot showing BloodPressure for subjects not in the control group.
> 2. How many ways are there to index this set of subjects?

### Combining Indexing and Assignment

The assignment operator `<-` can be combined with indexing.


```R
x <- c(1, 2, 3, 11, 12, 13)
x[x < 10] <- 0
x
```

> ## Updating a Subset of Values
>
> In this dataset, values for Gender have been recorded as both uppercase `M, F` and lowercase `m,f`.
> Combine the indexing and assignment operations to convert all values to lowercase.
