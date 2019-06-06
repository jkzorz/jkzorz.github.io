---
layout: post
title: "NMDS plots"
date: 2019-06-06
---


Non-metric Multi-dimensional Scaling (NMDS) plots are a way to condense information from multidimensional data (multiple variables/species/OTUs), into a 2D representation or ordination. In this ordination, the closer together two points are, the more similar the corresponding samples are with respect to the variables that went into making the NMDS plot.  

NMDS are great tools for microbial ecologists because you can condense the overwhelming amount of information from the distribution of multiple species/OTUs across your samples into something you can actually look at. In an NMDS plot generated using an OTU table, the points are samples, and the closer the points are together in the ordination space, the more similar their microbial communities.  

I'm not even going to pretend to be a statistical expert, but I will briefly try to give an overview of how the NMDS plot is generated.  If you want to learn more, check out the [GUSTAME](https://mb3is.megx.net/gustame/dissimilarity-based-methods/nmds) page dedicated to explaining NMDS.  

NMDS plots are non-metric, meaning they use data that is not required to fit a normal distribution. This is handy for microbial ecologists because the majority of our data has a skewed distribution with a long tail. In other words, there are only a few abundant species, and many, many species with low abundance (the long tail).  

NMDS is an iterative algorithm, meaning that it repeats the same series of steps over and over again until it finds the best solution. This is important to know because it means that each time you produce an NMDS plot from scratch it may look slightly different, even when starting with exactly the same data.  

What makes an NMDS plot non-metric, is that it is rank-based. This means that instead of using the actual values to calculate distances, it uses ranks. So for example, object A is the "1st" most close object to object B, and object C is the "2nd" most close. Learn more from: [GUSTAME](https://mb3is.megx.net/gustame/dissimilarity-based-methods/nmds)

The last basic thing to know about NMDS is that it uses a distance matrix as an input. Read more about distance matrices [here](https://sites.google.com/site/mb3gustame/reference/dissimilarity).  There are many different distance measures to choose from, however as a default, I tend to use the <b>Bray-Curtis</b> distance measure when dealing with relative abundance data. Bray-Curtis takes into account species presence/absence, as well as abundance, whereas other measures (like Jaccard) only take into account presence/absence. 

<b> Okay now to make one: </b>
Read in your OTU table that has species/OTUs as columns, and samples as rows, and install or load the package <b>"vegan"</b> into R. If you want to include a descriptor column(s) for your samples, it's easiest to place them next to the first column with all your sample names.   

```
install.packages("vegan")
library(vegan)
pc = read.csv("Your_OTU_table.csv", header= TRUE)

```

Now you want to make a data frame that has only your species/OTU abundance data. So a data frame that only includes your OTU columns. In my case, my column 1 is sample names, and my column 2 is the treatment variable.  Therefore my abundance data goes from my 3rd column, until the end. 

```
#make community matrix - extract columns with abundance information
com = pc[,3:ncol(pc)]

```

Often in R you will get errors because your data is not in the right format. Right now our data is in a "data frame" format, but it needs to be in a "matrix" format for use in the <i>vegan</i> package. The following code is how to convert it: 

```
#turn abundance data frame into a matrix
m_com = as.matrix(com)
```




