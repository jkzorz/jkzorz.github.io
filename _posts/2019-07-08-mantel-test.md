---
layout: post
title: "Mantel Test in R"
date: 2019-07-08
---


Statistics can be a scary word, especially, if you're like me and it's been a long time since you've had anything to do with probability or math. For the most part, I don't think it's necessary to understand all the equations behind the statistics that you do, but I do think it's important to know that you're using the right test for your data, and that you're not making any assumptions about your data that aren't supported.  Again, I find that the [GUSTAME](https://sites.google.com/site/mb3gustame/) website is a fantastic resource for learning about the basics of whatever test you decide to do.  


**Mantel tests** are correlation tests that determine the correlation between two matrices (rather than two variables). When using the test for microbial ecology, the matrices are often distance/dissimilarity matrices with corresponding positions (i.e. samples in the same order in both matrices). In order to calculate the correlation, the matrix values of both matrices are 'unfolded' into long column vectors, which are then used to determine correlation. Learn more about **[Mantel tests here](https://mb3is.megx.net/gustame/hypothesis-tests/the-mantel-test)**. 

A significant Mantel test will tell you that the distances between samples in one matrix are correlated with the distances between samples in the other matrix. *Therefore, as the distance between samples increases with respect to one matrix, the distances between the same samples also increases in the other matrix*.  

I find this is a difficult concept to explain without defining what the matrices are composed of, so here are some examples: 

- **Species abundance dissimilarity matrix**: created using a distance measure, i.e. *Bray-curtis dissimilarity*. This is the same type of dissimilarity matrix used when conducting an [ANOSIM test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html) or when making an [NMDS plot](https://jkzorz.github.io/2019/06/06/NMDS.html)
- **Environmental parameter distance matrix**: generally created using *Euclidean Distance* (i.e. temperature differences between samples)
- **Geographic distance matrix**: the physical distance between sites (i.e. *Haversine distance*)

With these matrix examples, you could determine if the differences in community composition between samples are correlated, or rather "co-vary", with the differences in temperature between samples, or the physical distance between samples.  

To perform a Mantel test in R. First load/install the required packages. The mantel test function is part of the **vegan** package, which is also used for *anosim* tests and *nmds* plots. 

```
library(vegan)
install.packages("geosphere")
library(geosphere)
```
If you are interested in using physical distance as a matrix for the mantel test. The package **geosphere** contains a function for calculating Haversine distances (distance between two points on a sphere) given latitude and longitude. 


Next load in your data with columns as OTUs/variables, and rows as samples. 

```
df = read.csv("Your_OTU_table.csv", header= TRUE)
```

The data I'm using looks like this:
The first column is sample name, the next 4 columns contain environmental parameters for each sample (i.e. Salinity, Temperature, etc). Then, the following 2 columns contain the latitude and longitude for each sample, and the remaining columns contain the 200+ OTU abundances that correspond to each sample. 

![useful image]({{ site.url }}/assets/Excel_for_mantel.png)
