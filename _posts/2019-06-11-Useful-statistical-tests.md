---
layout: post
title: "Useful Statistical Tests"
date: 2019-06-11
---


Statistics can be a scary word, especially, if you're like me and it's been a long time since you've done much math and probability. For the most part, I don't think it's necessary to understand all the math behind the statistics that you do, but I do think it's important to know that you're using the right test for your data, and that you're not making any assumptions about your data that aren't supported.  Again, I find that the [GUSTAME](https://sites.google.com/site/mb3gustame/) website is a fantastic resource for learning about the basics of whatever test you decide to do.  

<h2> ANOSIM Test </h2>

The first test that I'll talk about is the [ANOSIM test](https://sites.google.com/site/mb3gustame/hypothesis-tests/anosim).  The ANOSIM test is similar to an ANOVA hypothesis test, but it's using a dissimilarity matrix as input instead of raw data. It is also non-parametric, meaning it doesn't assume things about your data (like normal distribution etc), so it's a good bet for often-skewed microbial abundance data. As a non-parametric test, ANOSIM uses ranked dissimilarities instead of actual distances, and in this way it's a very nice complement to an [NMDS plot](https://jkzorz.github.io/2019/06/06/NMDS.html), and to assigning statistical significance to the clusters of samples that you may have seen forming in your ordination.  



This test is quite straightforward to do in R. First load in your data with columns as OTUs and rows as samples, and install the package <b>vegan</b> 

```
install.packages("vegan")
library(vegan)
pc = read.csv("Your_OTU_table.csv", header= TRUE)
```

xxx excel figure xxx

As a reminder, this is the NMDS figure we made in this [tutorial](https://jkzorz.github.io/2019/06/06/NMDS.html). I will be checking statistical significance of the groupings of "Time" and "Type" below. 

xxx NMDS figure XXX

Just like in the NMDS code, we need to make a data frame with just our abundance information. The first three columns of my data have information about my samples, so my abundance data goes from column 4 until the end. I then need to turn my data frame into a matrix.  

#make community matrix - extract columns with abundance information, turn data frame into matrix
com = pc[,4:ncol(pc)]
m_com = as.matrix(com)

Now use the function "anosim" from the package vegan to test your hypothesis that..... 

```
ano = anosim(m_com, pc$Type, distance = "bray", permutations = 9999)
```


