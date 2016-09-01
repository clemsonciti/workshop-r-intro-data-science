
---
layout: lesson
title: Introduction to Data Science using R
subtitle: Manipulating and analyzing data with dplyr
minutes: 30
---

> ## Learning objectives {.objectives}
> * Learn basic utilities of the dplyr package
> * Select and filter data
> * Be able to use magrittr pipes
> * Create new columns with mutate()
> * Use the split-apply-combine paradigm to summarize data
> * Export data with write.csv()


# Data Manipulation using dplyr

Bracket subsetting is handy, but it can be cumbersome and difficult to read,
especially for complicated operations. Enter `dplyr`. `dplyr` is a package for
making data manipulation easier.

Packages in R are basically sets of additional functions that let you do more
stuff. The functions we've been using so far, like `str()` or `data.frame()`,
come built into R; packages give you access to more of them. Before you use a
package for the first time you need to install it on your machine, and then you
should import it in every subsequent R session when you need it. **As part of 
the R Jupyter environment, dplyr is already installed.**


```R
install.packages("dplyr")
```


```R
library("dplyr")    ## load the package
```

## What is `dplyr`?

The package `dplyr` provides easy tools for the most common data manipulation
tasks. It is built to work directly with data frames. The thinking behind it was
largely inspired by the package `plyr` which has been in use for some time but
suffered from being slow in some cases.` dplyr` addresses this by porting much
of the computation to C++. An additional feature is the ability to work directly
with data stored in an external database. The benefits of doing this are
that the data can be managed natively in a relational database, queries can be
conducted on that database, and only the results of the query returned.

This addresses a common problem with R in that all operations are conducted in
memory and thus the amount of data you can work with is limited by available
memory. The database connections essentially remove that limitation in that you
can have a database of many 100s GB, conduct queries on it directly, and pull
back just what you need for analysis in R.

To learn more about `dplyr` after the workshop, you may want to check out this
[handy dplyr cheatsheet](http://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf).


```R
setwd("/home/lngo/intro-data-science/")
surveys = read.csv(file="data/sample.csv", header=TRUE)
```

## Selecting columns and filtering rows

We're going to learn some of the most common `dplyr` functions: `select()`,
`filter()`, `mutate()`, `group_by()`, and `summarize()`. To select columns of a
data frame, use `select()`. The first argument to this function is the data
frame (`surveys`), and the subsequent arguments are the columns to keep.


```R
select(surveys, Group, BloodPressure, Age)
```

To choose rows, use `filter()`:


```R
filter(surveys, BloodPressure > 100)
```

## Pipes

But what if you wanted to select and filter at the same time? There are three
ways to do this: use intermediate steps, nested functions, or pipes. With the
intermediate steps, you essentially create a temporary data frame and use that
as input to the next function. This can clutter up your workspace with lots of
objects. You can also nest functions (i.e. one function inside of another).
This is handy, but can be difficult to read if too many functions are nested as
the process from inside out. The last option, pipes, are a fairly recent
addition to R. Pipes let you take the output of one function and send it
directly to the next, which is useful when you need to do many things to the same
data set.  Pipes in R look like `%>%` and are made available via the `magrittr`
package installed as part of `dplyr`.


```R
surveys %>%
  filter(BloodPressure > 100) %>%
  select(Gender, Age)
```

In the above we use the pipe to send the `surveys` data set first through
`filter`, to keep rows where `BloodPressure` was greayer than 5, and then through `select`
to keep the `Gender` and `Age` columns. When the data frame is being passed to
the `filter()` and `select()` functions through a pipe, we don't need to include
it as an argument to these functions anymore.

If we wanted to create a new object with this smaller version of the data we
could do so by assigning it a new name:


```R
surveys_sml <- surveys %>%
  filter(BloodPressure > 100) %>%
  select(Gender, Age)
```


```R
surveys_sml
```

Note that the final data frame is the leftmost part of this expression.

> ### Challenge {.challenge}
>
>  Using pipes, subset the data to include individuals from the `Control` group,
>  and retain the columns `Gender`, `BloodPressure`, and `Age`

### Mutate

Frequently you'll want to create new columns based on the values in existing
columns, for example to do correct the capitalization of the gender. For this 
we'll use `mutate()`.

To create a new column of correct gender spelling:


```R
surveys %>%
  mutate(gender_capitalized = toupper(Gender))
```

If this runs off your screen and you just want to see the first few rows, you
can use a pipe to view the `head()` of the data (pipes work with non-dplyr
functions too, as long as the `dplyr` or `magrittr` packages are loaded).


```R
surveys %>%
  mutate(gender_capitalized = toupper(Gender)) %>%
  head
```

> ### Challenge {.challenge}
>
>  Create a new dataframe from the survey data that meets the following
>  criteria: contains only the `Age` column and a column `Aneurisms_avg` that 
>  contains values that are averaged of the Aneurisms colum . In this 
>  `Aneurisms_avg` column, only retain values that are greater than 150.
>
>  **Hint**: think about how the commands should be ordered to produce this data frame!

### Split-apply-combine data analysis and the summarize() function

Many data analysis tasks can be approached using the "split-apply-combine"
paradigm: split the data into groups, apply some analysis to each group, and
then combine the results. `dplyr` makes this very easy through the use of the
`group_by()` function.

#### The `summarize()` function

`group_by()` is often used together with `summarize()` which collapses each
group into a single-row summary of that group.  `group_by()` takes as argument
the column names that contain the **categorical** variables for which you want
to calculate the summary statistics. So to view mean the `Age` by sex:


```R
surveys %>%
  mutate(gender_capitalized = toupper(Gender)) %>%
  group_by(gender_capitalized) %>%
  summarize(mean_age = mean(Age, na.rm = TRUE))
```

You can group by multiple columns too:


```R
surveys %>%
  mutate(gender_capitalized = toupper(Gender)) %>%
  group_by(gender_capitalized, Group) %>%
  summarize(mean_age = mean(Age, na.rm = TRUE))
```

In this case, `dplyr` has changed our `data.frame` to a
`tbl_df`. This is a data structure that's very similar to a data frame; for our
purposes the only difference is that it won't automatically show tons of data
going off the screen, while displaying the data type for each column under its
name. If you want to display more data on the screen, you can add the `print()`
function at the end with the argument `n` specifying the number of rows to
display:


```R
surveys %>%
  mutate(gender_capitalized = toupper(Gender)) %>%
  group_by() %>%
  group_by(gender_capitalized, Group) %>%
  summarize(mean_age = mean(Age, na.rm = TRUE)) %>%
  print(n=6)
```

Once the data is grouped, you can also summarize multiple variables at the same
time (and not necessarily on the same variable). For instance, we could add a
column indicating the minimum weight for each species for each sex:


```R
surveys %>%
  mutate(gender_capitalized = toupper(Gender)) %>%
  group_by(gender_capitalized, Group) %>%
  summarize(max_age = max(Age),
            min_age = min(Age))
```

#### Tallying

When working with data, it is also common to want to know the number of
observations found for each factor or combination of factors. For this, `dplyr`
provides `tally()`. For example, if we wanted to group by the treatment methods and find the
number of rows of data for each method, we would do:


```R
surveys %>%
  group_by(Group) %>%
  tally()
```

Here, `tally()` is the action applied to the groups created by `group_by()` and
counts the total number of records for each category.

> ### Challenge {.challenge}
>
> How many males and females were surveyed?

> ### Challenge {.challenge}
>
> Use `group_by()` and `summarize()` to find the mean, min, and max aneurisms
> measurement at each quarter (`Aneurisms_q1`,`Aneurisms_q2`,`Aneurisms_q3`,`Aneurisms_q4`) 
> for each treatment group (using `Group`).
