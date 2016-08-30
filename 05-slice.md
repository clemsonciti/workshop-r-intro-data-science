
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




'data.frame'



R has loaded the contents of the .csv file into a variable called `dat` which is a `data frame`.


```R
dim(dat)
```




<ol class=list-inline>
	<li>100</li>
	<li>9</li>
</ol>




The data has 100 rows and 9 columns.


```R
head(dat)
```




<table>
<thead><tr><th></th><th scope=col>ID</th><th scope=col>Gender</th><th scope=col>Group</th><th scope=col>BloodPressure</th><th scope=col>Age</th><th scope=col>Aneurisms_q1</th><th scope=col>Aneurisms_q2</th><th scope=col>Aneurisms_q3</th><th scope=col>Aneurisms_q4</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>Sub001</td><td>m</td><td>Control</td><td>132</td><td>16</td><td>114</td><td>140</td><td>202</td><td>237</td></tr>
	<tr><th scope=row>2</th><td>Sub002</td><td>m</td><td>Treatment2</td><td>139</td><td>17.2</td><td>148</td><td>209</td><td>248</td><td>248</td></tr>
	<tr><th scope=row>3</th><td>Sub003</td><td>m</td><td>Treatment2</td><td>130</td><td>19.5</td><td>196</td><td>251</td><td>122</td><td>177</td></tr>
	<tr><th scope=row>4</th><td>Sub004</td><td>f</td><td>Treatment1</td><td>105</td><td>15.7</td><td>199</td><td>140</td><td>233</td><td>220</td></tr>
	<tr><th scope=row>5</th><td>Sub005</td><td>m</td><td>Treatment1</td><td>125</td><td>19.9</td><td>188</td><td>120</td><td>222</td><td>228</td></tr>
	<tr><th scope=row>6</th><td>Sub006</td><td>M</td><td>Treatment2</td><td>112</td><td>14.3</td><td>260</td><td>266</td><td>320</td><td>294</td></tr>
</tbody>
</table>




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




<ol class=list-inline>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
</ol>




This creates a vector of numbers from 6 to 9.

This can be very useful for addressing data.

> ## Subsetting with Sequences
>
> Use the colon operator to index just the aneurism count data (columns 6 to 9).

Finally we can use the `c()` (combine) function to address non-sequential rows and columns.


```R
dat[c(1,5,7,9), 1:5]
```




<table>
<thead><tr><th></th><th scope=col>ID</th><th scope=col>Gender</th><th scope=col>Group</th><th scope=col>BloodPressure</th><th scope=col>Age</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>Sub001</td><td>m</td><td>Control</td><td>132</td><td>16</td></tr>
	<tr><th scope=row>5</th><td>Sub005</td><td>m</td><td>Treatment1</td><td>125</td><td>19.9</td></tr>
	<tr><th scope=row>7</th><td>Sub007</td><td>f</td><td>Control</td><td>173</td><td>17.7</td></tr>
	<tr><th scope=row>9</th><td>Sub009</td><td>m</td><td>Treatment2</td><td>131</td><td>19.4</td></tr>
</tbody>
</table>




Returns the first 5 columns for patients in rows 1,5,7 & 9

> ## Subsetting Non-Sequential Data
>
> Return the age and gender values for the first 5 patients.

### Addressing by Name

Columns in an R data frame are named.


```R
names(dat)
```




<ol class=list-inline>
	<li>'ID'</li>
	<li>'Gender'</li>
	<li>'Group'</li>
	<li>'BloodPressure'</li>
	<li>'Age'</li>
	<li>'Aneurisms_q1'</li>
	<li>'Aneurisms_q2'</li>
	<li>'Aneurisms_q3'</li>
	<li>'Aneurisms_q4'</li>
</ol>




> ## Default Names
>
> If names are not specified e.g. using `headers=FALSE` in a `read.csv()` function, R assigns default names `V1,V2,...,Vn`

We usually use the `$` operator to address a column by name


```R
dat$Gender
```




<ol class=list-inline>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'M'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'F'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'M'</li>
	<li>'M'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'M'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'M'</li>
	<li>'M'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'M'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'M'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'F'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'M'</li>
	<li>'M'</li>
	<li>'M'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'M'</li>
	<li>'M'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'F'</li>
	<li>'f'</li>
	<li>'m'</li>
	<li>'m'</li>
	<li>'F'</li>
	<li>'m'</li>
	<li>'M'</li>
	<li>'M'</li>
</ol>




Named addressing can also be used in square brackets.


```R
head(dat[,c('Age', 'Gender')])
```




<table>
<thead><tr><th></th><th scope=col>Age</th><th scope=col>Gender</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>16</td><td>m</td></tr>
	<tr><th scope=row>2</th><td>17.2</td><td>m</td></tr>
	<tr><th scope=row>3</th><td>19.5</td><td>m</td></tr>
	<tr><th scope=row>4</th><td>15.7</td><td>f</td></tr>
	<tr><th scope=row>5</th><td>19.9</td><td>m</td></tr>
	<tr><th scope=row>6</th><td>14.3</td><td>M</td></tr>
</tbody>
</table>




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




<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>





```R
x %in% 1:10
```




<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>




We can use logical vectors to select data from a data frame.


```R
index <- dat$Group == 'Control'
dat[index,]$BloodPressure
```




<ol class=list-inline>
	<li>132</li>
	<li>173</li>
	<li>129</li>
	<li>77</li>
	<li>158</li>
	<li>81</li>
	<li>137</li>
	<li>111</li>
	<li>135</li>
	<li>108</li>
	<li>133</li>
	<li>139</li>
	<li>126</li>
	<li>125</li>
	<li>99</li>
	<li>122</li>
	<li>155</li>
	<li>133</li>
	<li>94</li>
	<li>98</li>
	<li>74</li>
	<li>116</li>
	<li>97</li>
	<li>104</li>
	<li>117</li>
	<li>90</li>
	<li>150</li>
	<li>116</li>
	<li>108</li>
	<li>102</li>
</ol>




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
