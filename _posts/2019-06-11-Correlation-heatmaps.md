---
layout: post
title: "Correlation Heatmaps"
---


Correlation is defined broadly in statistics as any association between two variables. Fundamentally, correlation does not equal causation, but finding two variables that are statistically correlated can be helpful in developing predictions about the relationships in your data.  

In microbial ecology, I find that using correlation analyses to determine which species/OTUs are correlated (positively or negatively) in abundance with which other species/OTUs, is a quick and easy way to gain insights into how the microbial community is assembled broadly, and also helps to find more intricate relationships between specific species.  Alternatively, if you have numerical, environmental variables of interest (i.e. temperature, organic acid concentration, depth, etc), you can also test to see which OTUs are correlated with which variables. This type of correlation can give you insight into niche differentiation and metabolism. 

There can be hundreds of OTUs in microbial ecology data, which would make the process of testing for correlations between each possible OTU pairing very tedious. Instead of doing these analyses one at a time, you can use R to generate correlation matrices, which calculate the correlations between many OTUs, which you can then plot out in a heatmap form for an easy visualization.    

Below I will show you how to generate a correlation matrix with your OTU data, and then how to plot that matrix as a heatmap using the R packages <b>corrplot</b>, <b> gplots</b> and <b>ggplot2</b>. 

First, install and load the appropriate packages:

```
install.packages("corrplot")
library(corrplot)
install.packages("gplots")
library(gplots)
install.packages("ggplot2")
library(ggplot2)
```

Next, read in your data as before:  
```
pc = read.csv("Autofermentation_bin_abundance_at_least_5pc_BM_transpose_larger.csv", header = TRUE)
```

Create a data frame with only your abundance data. In my case, the first two columns are sample names and groups, so my abundance data starts in column 3.  

```
com = pc[,3:ncol(pc)]
```

Now create a correlation matrix with your community composition data using the command 'cor': 

```
cc = cor(com, method = "spearman")

###if you have missing values in your data, you'll have to add the 'use' parameter. Check '?cor' for the cor command help page

```

Now you have a correlation matrix that contains correlation coefficients for every pairwise combination of OTUs in your data. The command is fairly straightforward, but you do have one statistical decision to make in deciding the method that is used to calculate the correlation coefficients. You can decide between <b>"pearson"</b>, <b>"spearman"</b>, and kendall coefficients, but I generally choose either pearson or spearman: 
<ul>
  <li><b>Pearson correlation:</b> is the linear correlation between two variables. [Read more here](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient) </li>
  <li><b>Spearman:</b></li> is a non-parametric measure of rank correlation and assesses how well a relationship between two variables can be described using a monotonic function. A monotonic function is just a fancy way of describing a relationship where for each x value, the y value increases. I usually use Spearman correlation because I'm not overly concerned that my relationships fit a linear model, and Spearman captures all types of positive or negative relationships (i.e. exponential, logarithmic). [This wikipedia page provides some visual examples of the differences between Spearman and Pearson](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient). 


The easiest way to visualize this correlation matrix 

