---
title: "Week1 Class2 - Biostatistics1"
author: "Taehoon Ha"
output:
  github_document:
    pandoc_args: --webtex
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

### 1. Data
#### 1.1 Data  
  * **Data**: consist of one or more **variables** measured / recorded on **observational units** in a **population**.
  * **Variable**: characteristics that differ among observational units.

```{r}
library(gapminder)
head(gapminder)
```

#### 1.2 Variables    
In the example above the variables are *country*, *continent*, *year*, *lifeExp*, *pop*, and *gdpPercap*.    
We usually distinguish the following two main types of variables. 

  * quantitative / numercial      
  * qualitative / categorical

#### 1.3 Quantitative Variables      

Quantitative variables can be:      

  * continuous (i.e. real numbers): any value corresponding to the points in an interval e.g., blood pressure, hegith.
  * discrete (i.e. integers): countable number of values e.g. # of events per year

In the dataset above, the continuous variables are *lifeExp* and *gdpPercap* and the discrete variable is *year*.   

#### 1.4 Qualitative Variables    

  * nomianl: names or labels, no natural order, e.g. sex, ethnicity, eye color    
  * ordinal: ordered categories, no natural numerical scale e.g. muscle tone, tumor grade, NY Heart Association Class    

In the dataset above, the qualitative variables are *country* and *continent*. They are both nominal variables.   

#### 1.5 Tidy Data     
The concept of tidy data was introduced by Hadley Wickham in his seminal paper (Wickam, 2014).      
There are three fundamental rules which make a dataset tidy:       

    1) Each variable must have its own column.    
    2) Each observation must have its own row.      
    3) Each value must have its own cell.     

The advantage of tidy data is both conceptual and practical: it allows consistency among datasets and the use of tools designed for this data structure.
```{r}
library(tidyverse)

X <- data.frame(회사 = c('A', 'A', 'A', 'B', 'B', 'B', 'B', 'C', 'C'),
매장지역 = c('경기', '서울', '제주', '경기', '서울', '부산', '인천', '경기', '제주'),
상품 = c(1, 2, 1, 1, 4, 4, 2, 4, 4))

X %>%
spread(key = 매장지역, value = 상품)


# Y to X is making data TIDY.
Y <- data.frame(회사 = c("A", "B", "C"),
경기 = c(1, 1, 4),
서울 = c(2, 4, NA), 
부산 = c(NA, 4, NA), 
인천 = c(NA, 2, NA),
제주 = c(1, NA, 4))

Y %>%
gather(key = 매장지역, value = 상품, indexs = 2:6) %>%
arrange(회사) %>%
filter(상품 != 'NA')
```

#### 1.6 Distribution

  * **Distribution**: the **frequencies** of one or more (quantitative or qualitative) **variables** recorded on units in a population or sample of interest.

  * **Distribution table** is a table of **absolute frequencies** (i.e., counts) or *relative frequencies (i.e., percentages) or both for the values taken on by the variable(s) in the group of interest.

  * **Population distribution**: distribution of variables for the units in a population.

  * **Population parameters**: summaries or measures of the population distribution.

Usually the population distribution and population parameters are unknown.

  * **Sample distribution**: distribution of variables for the units in a sample.

  * **Sample statistics**: summaries or measures computed from the sample distribution.

Usually sample statistics (sample distributions) are used to estimate the unknown population parameters (population distributions).

#### 1.7 Numerical and graphical summaries

Data summaries can be either numercial or graphical and allow us to:

  - Focus on the *main characteristics* of the data.
  - Reveal *unusual features* of the data.
  - Summarize *relationship between variables*.

#### 1.8 Exploratory Data Analysis (EDA)

**Exploratory Data Analysis (EDA)** is a key step of any data analysis and a statistical practice in general.

Before formally modeling the data, the dataset must be examined to get a 'first impression' of the data and to reveal expected and unexpected characteristics of the data. The initial examination is also useful for determining quality issues with the data such as missing values, unusual outliers, etc. It is also useful to examine the data to check the plausibility of the assumptions of candidate statistical models.

#### 1.9 Reporting of the results

Numerical and graphical summaries are also useful for displaying and reporting the results of a statistical analysis. For instance, one can report graphical displays of the distribution of the estimators, or check the properties of the residuals of the model, or compare the eﬀect sizes of several explanatory variables.



### 2. Graphical Summaries of One Variable

When dealing with a single variable, we are interested in summarizing the distribution of that variable, using univariate summaries, which are called statistics. Note that here we focus on **graphical summaries**, meaning that we do not want to display the full dataset as an image in a meaningful way. **Graphical summaries of particular utility are histograms, kernel density plots, and boxplots**.

#### 2.1 Histograms

A histogram is used to display numerical variables by aggregating similar values into "bins" to represent the distribution. The width of the bin is determined by the analyst (you) and most software distributions have a default algorithm for determining the bin width. Once the bin width is determined, the number of observations (or the proportion of observations) that fall within that interval is computed and graphed. The bin width is on the x-axis and the count or proportion of observations is on the y-axis.

This can be easily done with the following steps:

    1) Partition the real line into inetervals (**bins**).
    2) Assign each observation to its bin depending on its numercial value.
    3) Count the number of observations in each bin (or determine the proportion of values within each bin).

A **frequency histogram** reports the counts in each bin. A **relative frequency histogram** reports the proportion of observations within each bin.

**Freqeuncy histograms** are more useful when comparing histograms from sample of different sample sizes.

##### 2.1.1 Example: Life Expectancy

The following code produces is a frequency histogram.
```{r}
hist(gapminder$lifeExp, main = "Frequency Histogram", xlab = "Life Expectancy (years")
```

Using `freq = 'FASLE'` option, we can change the frequency histogram to **relative frequency histogram**.
```{r}
hist(gapminder$lifeExp, main = 'Relative Frequency Histogram', freq = F, xlab = 'Life Expectancy (years)')
```

Only difference between **frequency histogram** and **relative frequency histogram** is the **y-axis**. The shape and bins remain the same.

The proportions of the observations in each bin adds to 1 in a relative frequncy histogram.

##### 2.1.2 How to choose the bins

The histogram representation of the data depends on **2 critical choices**:

- The number of bins or the bin width (knowing one defines the other if the bin widths are equal in size)
- The location of the bin boundaries (important if the bin widths are not of equal size)

Note that the number of bins is the histogram controls the amount of information loss: ***the larger the bin width the more information we lose***. However, too many bins does not allow us to identify interesting patterns within the data because they get lost in the noise.

##### 2.1.3 Example: Life expectancy

Use `breaks() option to change this in a number of ways.

An easy way is just to give it one number that gives the number of cells for the histogram

The bins don't correspond to exactly the number we put in, because of the way R runs its algorithm to break up the data but it gives you generally what you want.
```{r}
par(mfrow = c(2, 2))
hist(gapminder$lifeExp, breaks = 5, main = "Breaks = 5", xlab = "Life Expectancy (years)")
hist(gapminder$lifeExp, breaks = 10, main = "Breaks = 10", xlab = "Life Expectancy (years)")
hist(gapminder$lifeExp, breaks = 30, main = "Breaks = 30", xlab = "Life Expectancy (years)")
hist(gapminder$lifeExp, breaks = 100, main = "Breaks = 100", xlab = "Life Expectancy (years)")
```

If you feel lazy about choosing the number of breaks, we have a solution `breaks = 'FD'`. It automatically settle your concern.

```{r}
hist(gapminder$lifeExp, breaks = 'FD', main = "Breaks = FD", xlab = "Life Expectancy (years)")
```

##### 2.1.4 How to choos the bins

A natural question is whether there is an **optimal** number of bins. R's default is to have equally spaced "breaks," in which the area of each bin is proportional to the number of points in each bin, but it's possible to provide unequally spaced breaks.

By default, R uses Sturges' formula.

$k = log_2n + 1$

This formula is derived from the binomial coefficient, and it assumes that the data are approximately normally distributed. It is known that Sturges' rule leads to over-smoothed histograms, and manually choosing the number of breaks might be beneficial in real applications. Actually, it is often informative to generate several histograms of the data with differeing numbers of bins as done in the example above.

### 2.2 Density Plots

**Density plots** are smoothed versions of histograms, obtained using **kernel density estimation** methods.

**Kernel density estimators** are defined as:   
$$
{ f }_{ n }(x)=\frac { 1 }{ nh } \sum _{ i=1 }^{ n }{ K } (\frac { x-{ x }_{ i } }{ h } )
$$

