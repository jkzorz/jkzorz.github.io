---
layout: post
title: "NMDS Plot extras in R"
date: 2020-04-04
---


A **[non-metric multidimensional scaling (NMDS) plot](https://jkzorz.github.io/2019/06/06/NMDS.html)** is one of the many types of ordination plots that can be used to show multidimensional data in 2 dimensions. An NMDS plot basically highlights the similarities between samples of complex multidimensional data. The closer two points (samples) are on the plot the more similar those samples are in terms of the underlying data. In my case, I'm usually looking at species abunance of microorganisms across samples. So the closer two samples are to each other, the more similar they are in terms of their microbial communities.   

The approach to generate an NMDS plot is non-metric, meaning that the algorithm doesn't assume any particular distribution of the data, which is handy when you have data that is inherently weird, and doesn't conform to bell-curve style distributions. The algorithm uses a rank based approach, that essentially ranks your samples in terms of closeness to one another based on a particular distance metric (i.e. Bray Curtis), [more about the specifics of the methods here](https://mb3is.megx.net/gustame/dissimilarity-based-methods/nmds). While this is a robust method for con-conforming data, it means that within an NMDS ordination plot the axes and coordinate system are arbitrary and don't relate to any meaningful values. 

An NMDS is just a visualization technique, and **is not a statistical assessment of sample separation or correlation**. For this you should run an **[ANOSIM test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html)** for categorical variables, or a **[Mantel test](https://jkzorz.github.io/2019/07/08/mantel-test.html)** for continuous variables.  

Okay, all that being said. There are ways to overlay extra information on your NMDS plots using the functions **[envfit](https://www.rdocumentation.org/packages/vegan/versions/1.4-0/topics/envfit)**, **[ordiplot](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordiplot)**, **[ordiellipse](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordihull)** and **[ordisurf](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordisurf)**, and I will explain how to use some of their features here. 
