
---
layout: lesson
title: Introduction to Data Science using R
subtitle: Data Types and Structures
minutes: 45
---

> ## Learning objectives {.objectives}
> * Expose learners to the different data types in R
> * Learn how to create vectors of different types
> * Be able to check the type of vector
> * Learn about missing data and other special values
> * Getting familiar with the different data structures (lists, matrices, data frames)


### Understanding Basic Data Types in R

To make the best of the R language, you'll need a strong understanding of the
basic data types and data structures and how to operate on those.

Very important to understand because these are the objects you will manipulate
on a day-to-day basis in R. Dealing with object conversions is one of the most
common sources of frustration for beginners.

**Everything** in R is an object.

R has 6 (although we will not discuss the raw class for this workshop) atomic
vector types.

* character
* numeric (real or decimal)
* integer
* logical
* complex

By *atomic*, we mean the vector only holds data of a single type.

* **character**: `"a"`, `"swc"`
* **numeric**: `2`, `15.5`
* **integer**: `2L` (the `L` tells R to store this as an integer)
* **logical**: `TRUE`, `FALSE`
* **complex**: `1+4i` (complex numbers with real and imaginary parts)

R provides many functions to examine features of vectors and other objects, for
example

* `class()` - what kind of object is it (high-level)?
* `typeof()` - what is the object's data type (low-level)?
* `length()` - how long is it? What about two dimensional objects?
* `attributes()` - does it have any metadata?


```R
# Example
x <- "dataset"
typeof(x)
attributes(x)

y <- 1:10
y
typeof(y)
length(y)

z <- as.numeric(y)
z

typeof(z)
```

R has many __data structures__. These include

* atomic vector
* list
* matrix
* data frame
* factors

### Atomic Vectors

A vector is the most common and basic data structure in R and is pretty much the
workhorse of R. Technically, vectors can be one of two types:

* atomic vectors
* lists

although the term "vector" most commonly refers to the atomic types not to lists.

### The Different Vector Modes

A vector is a collection of elements that are most commonly of mode `character`,
`logical`, `integer` or `numeric`.

You can create an empty vector with `vector()`. (By default the mode is
`logical`. You can be more explicit as shown in the examples below.) It is more
common to use direct constructors such as `character()`, `numeric()`, etc.


```R
vector() # an empty 'logical' (the default) vector

vector("character", length = 5) # a vector of mode 'character' with 5 elements

character(5) # the same thing, but using the constructor directly

numeric(5)   # a numeric vector with 5 elements

logical(5)   # a logical vector with 5 elements
```

You can also create vectors by directly specifying their content. R will then
guess the appropriate mode of storage for the vector. For instance:


```R
x <- c(1, 2, 3)
```

will create a vector `x` of mode `numeric`. These are the most common kind, and
are treated as double precision real numbers. If you wanted to explicitly create
integers, you need to add an `L` to each element (or *coerce* to the integer
type using `as.integer()`).


```R
x1 <- c(1L, 2L, 3L)
```

Using `TRUE` and `FALSE` will create a vector of mode `logical`:


```R
y <- c(TRUE, TRUE, FALSE, FALSE)
```

While using quoted text will create a vector of mode `character`:


```R
z <- c("Sarah", "Tracy", "Jon")
```

### Examining Vectors

The functions `typeof()`, `length()`, `class()` and `str()` provide useful
information about your vectors and R objects in general.


```R
typeof(z)

length(z)

class(z)

str(z)
```




'character'






3






'character'



     chr [1:3] "Sarah" "Tracy" "Jon"


### Adding Elements

The function `c()` (for combine) can also be used to add elements to a vector.


```R
z <- c(z, "Annette")
z
z <- c("Greg", z)
z
```

### Vectors from a Sequence of Numbers

You can create vectors as a sequence of numbers.


```R
series <- 1:10
seq(10)
seq(from = 1, to = 10, by = 0.1)
```

### Missing Data

R supports missing data in vectors. They are represented as `NA` (Not Available)
and can be used for all the vector types covered in this lesson:


```R
x <- c(0.5, NA, 0.7)
x <- c(TRUE, FALSE, NA)
x <- c("a", NA, "c", "d", "e")
x <- c(1+5i, 2-3i, NA)
```

The function `is.na()` indicates the elements of the vectors that represent
missing data, and the function `anyNA()` returns `TRUE` if the vector contains
any missing values:


```R
x <- c("a", NA, "c", "d", NA)
y <- c("a", "b", "c", "d", "e")
is.na(x)
is.na(y)
anyNA(x)
anyNA(y)
```

### Other Special Values

`Inf` is infinity. You can have either positive or negative infinity.


```R
1/0
```




Inf



`NaN` means Not a Number. It's an undefined value.


```R
0/0
```




NaN



### What Happens When You Mix Types Inside a Vector?

R will create a resulting vector with a mode that can most easily accommodate
all the elements it contains. This conversion between modes of storage is called
"coercion". When R converts the mode of storage based on its content, it is
referred to as "implicit coercion". For instance, can you guess what the
following do (without running them first)?


```R
xx <- c(1.7, "a")
xx
xx <- c(TRUE, 2)
xx
xx <- c("a", TRUE)
xx
```




<ol class=list-inline>
	<li>'1.7'</li>
	<li>'a'</li>
</ol>







<ol class=list-inline>
	<li>1</li>
	<li>2</li>
</ol>







<ol class=list-inline>
	<li>'a'</li>
	<li>'TRUE'</li>
</ol>




You can also control how vectors are coerced explicitly using the
`as.<class_name>()` functions:


```R
as.numeric("1")
as.character(1:2)
```

### Objects Attributes

Objects can have __attributes__. Attributes are part of the object. These include:

* names
* dimnames
* dim
* class
* attributes (contain metadata)

You can also glean other attribute-like information such as length (works on
vectors and lists) or number of characters (for character strings).


```R
length(1:10)
nchar("Clemson Tigers")
```




10






14



### Matrix

In R matrices are an extension of the numeric or character vectors. They are not
a separate type of object but simply an atomic vector with dimensions; the
number of rows and columns.


```R
m <- matrix(nrow = 2, ncol = 2)
m
dim(m)
```

Matrices in R are filled column-wise.


```R
m <- matrix(1:6, nrow = 2, ncol = 3)
m
```

Other ways to construct a matrix


```R
m      <- 1:10
dim(m) <- c(2, 5)
m
```

This takes a vector and transforms it into a matrix with 2 rows and 5 columns.

Another way is to bind columns or rows using `cbind()` and `rbind()`.


```R
x <- 1:3
y <- 10:12
cbind(x, y)

rbind(x, y)
```




<table>
<thead><tr><th scope=col>x</th><th scope=col>y</th></tr></thead>
<tbody>
	<tr><td> 1</td><td>10</td></tr>
	<tr><td> 2</td><td>11</td></tr>
	<tr><td> 3</td><td>12</td></tr>
</tbody>
</table>







<table>
<tbody>
	<tr><th scope=row>x</th><td>1</td><td>2</td><td>3</td></tr>
	<tr><th scope=row>y</th><td>10</td><td>11</td><td>12</td></tr>
</tbody>
</table>




You can also use the `byrow` argument to specify how the matrix is filled. From R's own documentation:


```R
mdat <- matrix(c(1,2,3, 11,12,13), nrow = 2, ncol = 3, byrow = TRUE)
mdat
```




<table>
<tbody>
	<tr><td>1</td><td>2</td><td>3</td></tr>
	<tr><td>11</td><td>12</td><td>13</td></tr>
</tbody>
</table>




### List

In R lists act as containers. Unlike atomic vectors, the contents of a list are
not restricted to a single mode and can encompass any mixture of data
types. Lists are sometimes called generic vectors, because the elements of a
list can by of any type of R object, even lists containing further lists. This
property makes them fundamentally different from atomic vectors.

A list is a special type of vector. Each element can be a different type.

Create lists using `list()` or coerce other objects using `as.list()`. An empty
list of the required length can be created using `vector()`


```R
x <- list(1, "a", TRUE, 1+4i)
x
x <- vector("list", length = 5) ## empty list
length(x)
x[[1]]
x <- 1:10
x <- as.list(x)
length(x)
```

1. What is the class of `x[1]`?
2. What about `x[[1]]`?


```R
xlist <- list(a = "Karthik Ram", b = seq(1,10), data = head(iris))
xlist
```

1. What is the length of this object? What about its structure?

Lists can be extremely useful inside functions. You can “staple” together lots
of different kinds of results into a single object that a function can return.

A list does not print to the console like a vector. Instead, each element of the
list starts on a new line.

Elements are indexed by double brackets. Single brackets will still return
a(nother) list.

### Data Frame

A data frame is a very important data type in R. It's pretty much the *de facto*
data structure for most tabular data and what we use for statistics.

A data frame is a special type of list where every element of the list has same length.

Data frames can have additional attributes such as `rownames()`, which can be
useful for annotating data, like `subject_id` or `sample_id`. But most of the
time they are not used.

Some additional information on data frames:

* Usually created by `read.csv()` and `read.table()`.
* Can convert to matrix with `data.matrix()` (preferred) or `as.matrix()`
* Coercion will be forced and not always what you expect.
* Can also create with `data.frame()` function.
* Find the number of rows and columns with `nrow(dat)` and `ncol(dat)`, respectively.
* Rownames are usually 1, 2, ..., n.

### Creating Data Frames by Hand

To create data frames by hand:


```R
dat <- data.frame(id = letters[1:10], x = 1:10, y = 11:20)
dat
```

> ## Useful Data Frame Functions
>
> * `head()` - shown first 6 rows
> * `tail()` - show last 6 rows
> * `dim()` - returns the dimensions
> * `nrow()` - number of rows
> * `ncol()` - number of columns
> * `str()` - structure of each column
> * `names()` - shows the `names` attribute for a data frame, which gives the column names.

See that it is actually a special list:


```R
is.list(iris)
class(iris)
```

| Dimensions | Homogenous | Heterogeneous |
| ------- | ---- | ---- |
| 1-D | atomic vector | list |
| 2-D | matrix | data frame |


> ## Column Types in Data Frames
>
> Knowing that data frames are lists of lists, can columns be of different type?
>
> What type of structure do you expect on the iris data frame? Hint: Use `str()`.
>
> ~~~
> # The Sepal.Length, Sepal.Width, Petal.Length and Petal.Width columns are all
> # numeric types, while Species is a Factor.
> # Lists can have elements of different types.
> # Since a Data Frame is just a special type of list, it can have columns of
> # differing type (although, remember that type must be consistent within each column!).
> str(iris)
> ~~~
