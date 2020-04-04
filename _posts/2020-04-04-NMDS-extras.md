---
layout: post
title: "NMDS Plot extras in R"
date: 2020-04-04
---


A **[non-metric multidimensional scaling (NMDS) plot](https://jkzorz.github.io/2019/06/06/NMDS.html)** is one of the many types of ordination plots that can be used to show multidimensional data in 2 dimensions. An NMDS plot basically highlights the similarities between samples of complex multidimensional data. The closer two points (samples) are on the plot the more similar those samples are in terms of the underlying data. In my case, I'm usually looking at species abunance of microorganisms across samples. So the closer two samples are to each other, the more similar they are in terms of their microbial communities.   

The approach to generate an NMDS plot is non-metric, meaning that the algorithm doesn't assume any particular distribution of the data, which is handy when you have data that is inherently weird, and doesn't conform to bell-curve style distributions. The algorithm uses a rank based approach, that essentially ranks your samples in terms of closeness to one another based on a particular distance metric (i.e. Bray Curtis), [more about the specifics of the methods here](https://mb3is.megx.net/gustame/dissimilarity-based-methods/nmds). While this is a robust method for con-conforming data, it means that within an NMDS ordination plot the axes and coordinate system are arbitrary and don't relate to any meaningful values. 

An NMDS is just a visualization technique, and **is not a statistical assessment of sample separation or correlation**. For this you should run an **[ANOSIM test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html)** for categorical variables, or a **[Mantel test](https://jkzorz.github.io/2019/07/08/mantel-test.html)** for continuous variables. Other ordination techniques like PCA, CA, CCA, RDA, etc, may be more useful to you if you want your axes to be meaningful, or if you want to talk about variation partitioning.  

Okay, all that being said. There are ways to overlay extra information on your NMDS plots using the functions **[envfit](https://www.rdocumentation.org/packages/vegan/versions/1.4-0/topics/envfit)**, **[ordiplot](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordiplot)**, **[ordiellipse](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordihull)** and **[ordisurf](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordisurf)**, and I will explain how to use some of their features here. 


**ENVFIT**

Envfit fits environmental vectors or factors onto an ordination. The default plot for envfit, like metaMDS, isn't very aesthetically pleasing, so I will show how you can plot both using **ggplot2**.

First load your libraries and read in your data: 

```
library(vegan)
library(ggplot2)
df = read.csv("your_data_frame.csv", header = TRUE)

```

My data contains samples as rows and columns contain either environmental variables, or species abundance data. In this example, I have both categorical and continuous environmental variables.

(excel file) 

Next, subset your data so that you have a data frame with only environmental variables, and a data frame with species abundance data: 

```
env = df[,]
com = df[,]
```

Perform the NMDS ordination, as discussed more in **[this tutorial](https://jkzorz.github.io/2019/06/06/NMDS.html)**

```
m_com = as.matrix(com)

#nmds code
set.seed(123)
nmds = metaMDS(m_com, distance = "bray")
nmds
data.scores = as.data.frame(scores(nmds))
```

Now we run the envfit function with our environmental data frame, *env*.

```
en = envfit(nmds, env, permutations = 999, na.rm = TRUE)
```
The first parameter is the metaMDS object from the NMDS ordination we just performed. Next is *env* our environmental data frame. Then we state we want 999 permutations of the data, and to remove any rows with missing data. 

Let's see what the envfit function returns: 

```
> en$vectors
               NMDS1    NMDS2     r2 Pr(>r)    
Depth        0.32513 -0.94567 0.7954  0.001 ***
Salinity     0.69000 -0.72381 0.5023  0.001 ***
Temperature  0.75734  0.65303 0.7817  0.001 ***
Oxygen      -0.91051  0.41349 0.7738  0.001 ***
Chlorophyll -0.99684 -0.07940 0.4881  0.001 ***
Latitude    -0.95483 -0.29715 0.1051  0.001 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
Permutation: free
Number of permutations: 999

```

When we call the results of the envfit function, we get a table that gives our environmental variables as rows. It then gives their respective coordinates on the NMDS ordination in the NMDS1 and NMDS2 axes. 
