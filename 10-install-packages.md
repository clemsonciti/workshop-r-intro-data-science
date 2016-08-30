
---
layout: lesson
title: Introduction to Data Science using R
subtitle: Installing Packages
minutes: 15
---

> ## Learning objectives {.objectives}
> * Be able to find supporting packages
> * Be able to install most of the packages for the Jupyter interface as a Palmetto user
> * Know where to find help

A significant advantage of R is the collection of community contributed packages that have been vetted through [CRAN (the Comprehensive R Archive Network)](https://cran.r-project.org/web/packages/). 

R automate the downloading and installing of CRAN packages through the `install.packages` function:


```R
?install.packages
```

By default, `install.packages` will be writing into locations that typically require administrative priviledge. This is not the case of Palmetto. It is important that you know how to set up your own customized library as a user on Palmetto. We will practice this by installing the *ggplo2* package in preparation for the next lesson. 

The process is as followed:

1. In your home directory, create a directory that is specific to storing the installed R packages. 

2. Run the `install.packages` command in a cell (you only need to do this once) with the following format:
```
install.packages(<string vector containing list of libraries to be installed>, 
                 lib=<location of the previously created directory for R packages>, 
                 repos='http://cran.us.r-project.org',
                 verbose=TRUE)


```R
install.packages("ggplot2", 
                 lib="/home/lngo/R_libs", 
                 repos='http://cran.us.r-project.org',
                 verbose=TRUE)
```

To load a customized package, you need to also specify the location of the package directory:


```R
library(ggplot2,lib="/home/lngo/R_libs")
```

There are packages that require additional supporting software. For example, the parallel `rjacgs` library requires  JAGS (Just Another Gigg Sampler) to be installed and made available via system path. If you need help installing such packages, please do not hesitate to come talk to us at ithelp@clemson.edu
