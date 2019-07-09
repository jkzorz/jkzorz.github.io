---
layout: post
title: "Scatter plots in R"
date: 2019-07-09
---


**Scatter plots** are basic plots showing the relationship between two (usually continuous) variables. Making scatter plots in R is relatively straightforward using the **ggplot2** package. 


Start by loading the **ggplot2** library, and then loading in your data with columns as OTUs/variables, and rows as samples. 

```
library(ggplot2)
df = read.csv("Your_OTU_table.csv", header= TRUE)
```

**The data I'm using looks like this:**


![useful image]({{ site.url }}/assets/Excel_for_mantel.png)


The first column is sample name, the next 4 columns contain environmental parameters for each sample (i.e. Salinity, Temperature, etc). Then, the following 2 columns contain the latitude and longitude for each sample, and the remaining columns contain the 200+ OTU abundances that correspond to each sample.


For my first plot, I am interested in observing the relationship between temperature and a certain species (Pelagibacteraceae.OTU_307744):

```
```

