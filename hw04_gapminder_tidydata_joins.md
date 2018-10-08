Alejandra\_hw04\_tidy\_data\_joins
================

## Tidy data and joins

This is an R Markdown document that has the objective of practicing and
strengthen data wrangling skills by working with some realistic problems
in the grey area between data aggregation and data reshaping.

According to Hadley Wickham, **Data Tidying** is structuring datasets to
facilitate analysis.

## Loading data and required libraries

``` r
library(gapminder)
library(dplyr)
library(tidyr)
library(kableExtra)
library(gridExtra)
library(ggplot2)
```

**Data Reshaping Prompts (and relationship to aggregation)**

Using `gather()` and `spread()` functions to reshape data can be very
useful to present tables, figures or doing aggregations and statistical
analysis.

\#\# Activity 2:

  - Make a tibble with one row per year and columns for life expectancy
    for two or more countries.

\-Use knitr::kable() to make this table look pretty in your rendered
homework. -Take advantage of this new data shape to scatterplot life
expectancy for one country against that of another.

For this activity, I will select the countries from North America:
Mexico, United States and Canada.

First let’s only look at the subset of life expectancy in these
countries:

``` r
#subset data from North America
NAmerica = gapminder %>%
  filter(country %in% c("Mexico", "United States", "Canada")) %>% 
  select(country, year, lifeExp)
  
#Put the subset in a table
  kable (NAmerica) %>%
  kable_styling(full_width = FALSE, position = "center")
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

country

</th>

<th style="text-align:right;">

year

</th>

<th style="text-align:right;">

lifeExp

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

68.750

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

69.960

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

71.300

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

72.130

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

72.880

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

74.210

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

75.760

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

76.860

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

77.950

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

78.610

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

79.770

</td>

</tr>

<tr>

<td style="text-align:left;">

Canada

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

80.653

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

50.789

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

55.190

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

58.299

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

60.110

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

62.361

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

65.032

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

67.405

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

69.498

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

71.455

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

73.670

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

74.902

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

76.195

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

68.440

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

69.490

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

70.210

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

70.760

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

71.340

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

73.380

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

74.650

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

75.020

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

76.090

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

76.810

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

77.310

</td>

</tr>

<tr>

<td style="text-align:left;">

United States

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

78.242

</td>

</tr>

</tbody>

</table>

Now, let’s make the data tidy by using `spread()` which turns a pair of
key-values into tidy columns. I will put the outpu in a table and I will
also make a plot with this new tidy data.

``` r
NAtable = NAmerica %>%
spread(key = country, value = lifeExp) #define key value pairs for new columns

NAplot = NAmerica %>%
  ggplot(aes(year, lifeExp, color = country)) +
  geom_point() +
  geom_line() +
  labs(x="Year", y="Life Expectancy", title= "Life Expectancy in North America") #add axis labels and title

grid.arrange(tableGrob(NAtable), NAplot, nrow = 1) #put table next to plot
```

<img src="hw04_gapminder_tidydata_joins_files/figure-gfm/unnamed-chunk-3-1.png" style="display: block; margin: auto auto auto 0;" />

`spread()` allowed tidying data by making a column for each conytry and
each observation in a row. This made it possible to do the above plot
easily. We can see that in early years, there was a huge gap between the
life expectancy of Mexico and the rest of North America. However, over
the years, life expectancy in Mexico increased and in 2007 it was not
too far behind from United States and Canada.

Now let’s compare life expectancy between US and Canada by making a
scatterplot:

``` r
NAtable %>%
  ggplot(aes(x=Canada, y=Mexico)) +
  geom_point(aes(color=year)) +
  geom_smooth(method="lm", color="lightpink")
```

![](hw04_gapminder_tidydata_joins_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
