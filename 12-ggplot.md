
---
layout: lesson
title: Introduction to Data Science using R
subtitle: Manipulating and analyzing data with dplyr
minutes: 60
---

> ## Learning objectives {.objectives}
> * Visualize some of the [mammals data](https://dx.doi.org/10.6084/m9.figshare.1314459.v5)
> from Figshare [surveys.csv](https://ndownloader.figshare.com/files/2292172)
> * Understand how to plot these data using R ggplot2 package. For more details
> on using ggplot2 see [official documentation](http://docs.ggplot2.org/current/).
> * Building step by step complex plots with the ggplot2 package


```R
setwd("/home/lngo/intro-data-science/")
surveys = read.csv(file="data/sample.csv", header=TRUE)

# plotting package
library(ggplot2)
# modern data frame manipulations
library(dplyr)
```

## Plotting with ggplot2

We will make the same plot using the `ggplot2` package.

`ggplot2` is a plotting package that makes it simple to create complex plots
from data in a dataframe. It uses default settings, which help creating
publication quality plots with a minimal amount of settings and tweaking.

ggplot graphics are built step by step by adding new elements.

To build a ggplot we need to:

- bind the plot to a specific data frame using the `data` argument


```R
ggplot(data = surveys)
```

- define aesthetics (`aes`), that maps variables in the data to axes on the plot
     or to plotting size, shape color, etc.,


```R
ggplot(data = surveys, aes(x = Age, y = BloodPressure))
```

- add `geoms` -- graphical representation of the data in the plot (points,
     lines, bars). To add a geom to the plot use `+` operator:


```R
ggplot(data = surveys, aes(x = Age, y = BloodPressure)) + geom_point()
```

The `+` in the `ggplot2` package is particularly useful because it allows you
to modify existing `ggplot` objects. This means you can easily set up plot
"templates" and conveniently explore different types of plots, so the above
plot can also be generated with code like this:


```R
# Create
surveys_plot <- ggplot(data = surveys, aes(x = Age, y = BloodPressure))

# Draw the plot
surveys_plot + geom_point()
```

Notes:

- Anything you put in the `ggplot()` function can be seen by any geom layers
  that you add.  i.e. these are universal plot settings. This includes the x and
  y axis you set up in `aes()`.
- You can also specify aesthetics for a given geom independently of the
  aesthetics defined globally in the `ggplot()` function.


## Building your plots iteratively

Building plots with ggplot is typically an iterative process. We start by
defining the dataset we'll use, lay the axes, and choose a geom.


```R
ggplot(data = surveys, aes(x = Age, y = BloodPressure)) +
    geom_point()
```

Then, we start modifying this plot to extract more information from it. For
instance, we can add transparency (alpha) to avoid overplotting.


```R
ggplot(data = surveys, aes(x = Age, y = BloodPressure))  +
    geom_point(alpha = 0.5)
```

We can also add colors for all the points


```R
ggplot(data = surveys, aes(x = Age, y = BloodPressure))  +
    geom_point(alpha = 0.5, color = "blue")
```

Or to color each treatment group in the plot differently:


```R
ggplot(data = surveys, aes(x = Age, y = BloodPressure))  +
    geom_point(alpha = 0.5, aes(color=Group))
```

## Boxplot

Visualising the distribution of blood pressure within each treatment group.


```R
ggplot(data = surveys, aes(x = Group, y = BloodPressure))  +
    geom_boxplot()
```

By adding points to boxplot, we can have a better idea of the number of
measurements and of their distribution:


```R
ggplot(data = surveys, aes(x = Group, y = BloodPressure))  +
    geom_boxplot(alpha = 0) +
    geom_jitter(alpha = 0.3, color = "tomato")
```

Notice how the boxplot layer is behind the jitter layer? What do you need to
change in the code to put the boxplot in front of the points such that it's not
hidden.

> ### Challenges
>
> Boxplots are useful summaries, but hide the *shape* of the distribution. For
> example, if there is a bimodal distribution, this would not be observed with a
> boxplot. An alternative to the boxplot is the violin plot (sometimes known as a
> beanplot), where the shape (of the density of points) is drawn.
>
> - Replace the box plot with a violin plot; see `geom_violin()`
>
> In many types of data, it is important to consider the *scale* of the
> observations.  For example, it may be worth changing the scale of the axis to
> better distribute the observations in the space of the plot.  Changing the scale
> of the axes is done similarly to adding/modifying other components (i.e., by
> incrementally adding commands).
>
> - Represent blood pressure on the log10 scale; see `scale_y_log10()`
>
> - Create boxplot for `BloodPressure`.


## Plotting data series



```R
survey_counts <- surveys %>%
                  mutate(gender_capitalized = toupper(Gender)) %>%
                  group_by(gender_capitalized, Group) %>%
                  tally
```


```R
survey_counts
```


```R
ggplot(data = survey_counts, aes(x = Group, y = n)) +
     geom_line()
```

Unfortunately this does not work, because we plot data for all the species
together. We need to tell ggplot to draw a line for each species by modifying
the aesthetic function to include `group = gender_capitalized`.


```R
ggplot(data = survey_counts, aes(x = Group, y = n, group = gender_capitalized)) +
    geom_line()

```

We will be able to distinguish species in the plot if we add colors.


```R
ggplot(data = survey_counts, aes(x = Group, y = n, group = gender_capitalized, colour = gender_capitalized)) +
    geom_line()
```

## Faceting

ggplot has a special technique called *faceting* that allows to split one plot
into multiple plots based on a factor included in the dataset. We will use it to
make one plot for a time series for each species.


```R
ggplot(data = survey_counts, aes(x = Group, y = n, group = gender_capitalized, colour = gender_capitalized)) +
    geom_line() +
    facet_wrap(~ gender_capitalized)

```

Now we would like to split line in each plot by the blood pressure of each individual, which 
to be converted into scales of low (0-90), medium (90-120), and high (>120). 


```R
survey_counts <- surveys %>%
                  mutate(gender_capitalized = toupper(Gender)) %>%
                  mutate(blood_pressure_rank = cut(BloodPressure,
                                                   breaks = c(0,90,120,300), 
                                                   labels=c("low","medium","high"))) %>%
                  group_by(gender_capitalized, Group, blood_pressure_rank) %>%
                  tally
```


```R
survey_counts
```

We can now make the faceted plot splitting further by blood pressure rank (within a single plot):


```R
ggplot(data = survey_counts, aes(x = Group, y = n, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_wrap(~ gender_capitalized)
```

Usually plots with white background look more readable when printed.  We can set
the background to white using the function `theme_bw()`.


```R
ggplot(data = survey_counts, aes(x = Group, y = n, color = gender_capitalized, group = blood_pressure_rank)) +
     geom_line() +
     facet_wrap(~ gender_capitalized) + 
     theme_bw()
```

To make the plot easier to read, we can color by sex instead of species (species
are already in separate plots, so we don't need to distinguish them further).


```R
ggplot(data = survey_counts, aes(x = Group, y = n, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_wrap(~ gender_capitalized) + 
     theme_bw()
```

The `facet_wrap` geometry extracts plots into an arbitrary number of dimensions
to allow them to cleanly fit on one page. On the other hand, the `facet_grid`
geometry allows you to explicitly specify how you want your plots to be
arranged via formula notation (`rows ~ columns`; a `.` can be used as
a placeholder that indicates only one row or column).

Let's modify the previous plot to compare how the weights of male and females
has changed through time.


```R
anm_avg <- surveys %>%
                  mutate(gender_capitalized = toupper(Gender)) %>%
                  mutate(blood_pressure_rank = cut(BloodPressure,
                                                   breaks = c(0,90,120,300), 
                                                   labels=c("low","medium","high"))) %>%
                  group_by(gender_capitalized, Group, blood_pressure_rank) %>%
                  summarize(avg_anm_q1 = mean(Aneurisms_q1))
```


```R
# one column, facet by row
ggplot(data = anm_avg, aes(x = Group, y = avg_anm_q1, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_grid(gender_capitalized ~ .) 
```


```R
# one row, facet by column
ggplot(data = anm_avg, aes(x = Group, y = avg_anm_q1, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_grid(. ~ gender_capitalized) 
```

## Customization

Take a look at the ggplot2 cheat sheet
(https://www.rstudio.com/wp-content/uploads/2015/08/ggplot2-cheatsheet.pdf), and
think of ways to improve the plot. 

Now, let's change names of axes to something more informative than 'year'
and 'n' and add a title to this figure:


```R
ggplot(data = survey_counts, aes(x = Group, y = n, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_wrap(~ gender_capitalized) + 
     labs(title = 'Patient Demographic' ,
         x = 'Treatment Group',
         y = 'Count') +
     theme_bw()
```

The axes have more informative names, but their readibility can be improved by
increasing the font size. While we are at it, we'll also change the font family:


```R
ggplot(data = survey_counts, aes(x = Group, y = n, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_wrap(~ gender_capitalized) + 
     labs(title = 'Patient Demographic' ,
         x = 'Treatment Group',
         y = 'Count') +
     theme_bw() +
     theme(text=element_text(size=16, family="Courier"))
```

After our manipulations we notice that the values on the x-axis are still not
properly readable. Let's change the orientation of the labels and adjust them
vertically and horizontally so they don't overlap. You can use a 90 degree
angle, or experiment to find the appropriate angle for diagonally oriented
labels.


```R
ggplot(data = survey_counts, aes(x = Group, y = n, color = blood_pressure_rank, group = blood_pressure_rank)) +
     geom_line() +
     facet_wrap(~ gender_capitalized) + 
     labs(title = 'Patient Demographic' ,
         x = 'Treatment Group',
         y = 'Count') +
     theme_bw() +
     theme(axis.text.x = element_text(colour="grey20", size=12, angle=90, hjust=.5, vjust=.5),
           axis.text.y = element_text(colour="grey20", size=12),
           text=element_text(size=16, family="Courier"))
```

If you like the changes you created to the default theme, you can save them as
an object to easily apply them to other plots you may create:


```R
courier_grey_theme <- theme(axis.text.x = element_text(colour="grey20", size=12, angle=90, hjust=.5, vjust=.5),
                            axis.text.y = element_text(colour="grey20", size=12),
                            text=element_text(size=16, family="Courier"))
ggplot(data = surveys, aes(x = Group, y = BloodPressure))  +
    geom_boxplot() + 
    courier_grey_theme
```

With all of this information in hand, please take another five minutes to either
improve one of the plots generated in this exercise or create a beautiful graph
of your own. Use the RStudio ggplot2 cheat sheet, which we linked earlier for
inspiration.

Here are some ideas:

* See if you can change thickness of the lines.
* Can you find a way to change the name of the legend? What about its labels?
* Use a different color palette (see http://www.cookbook-r.com/Graphs/Colors_(ggplot2)/)

After creating your plot, you can save it to a file in your favourite format.
You can easily change the dimension (and its resolution) of your plot by
adjusting the appropriate arguments (`width`, `height` and `dpi`):


```R
my_plot <- ggplot(data = survey_counts, 
                  aes(x = Group, y = n, color = blood_pressure_rank, group = blood_pressure_rank)) +
            geom_line() +
            facet_wrap(~ gender_capitalized) + 
            labs(title = 'Patient Demographic' ,
                 x = 'Treatment Group',
                 y = 'Count') +
            theme_bw() +
            theme(axis.text.x = element_text(colour="grey20", size=12, angle=90, hjust=.5, vjust=.5),
                  axis.text.y = element_text(colour="grey20", size=12),
                  text=element_text(size=16, family="Courier"))

ggsave("name_of_file.png", my_plot, width=15, height=10)
```
