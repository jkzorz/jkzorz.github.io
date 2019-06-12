---
layout: post
title: "Correlation Heatmaps"
---


Correlation is defined broadly in statistics as any association between two variables. Correlation obviously does not equal Causation, but finding two variables that are statistically correlated can be helpful in developing predictions about the relationships in your data.  

In microbial ecology, I find that using correlation analyses to determine which species/OTUs are correlated (positively or negatively) in abundance with which other species/OTUs, is a quick and easy way to gain insights into how the microbial community is assembled broadly, and also to find more intricate dynamics between species.  Alternatively, if you have numerical, environmental variables of interest (i.e. temperature, organic acid concentration, depth, etc), you can also test to see which OTUs are correlated with which variables. This type of correlation can give you insight into niche differentiation, and possibly metabolism. 

There can be hundreds of OTUs in microbial ecology data, which would make the process of testing for correlations between each possible OTU pairing very tedious. Instead of doing these analyses one at a time, you can use R to generate correlation matrices, which calculate the correlations between many OTUs, which you can then plot out in a heatmap form for an easy visualization.    

Below I will show you how to generate a correlation matrix with your OTU data, and then how to plot that matrix as a heatmap using the R packages <b>corrplot</b>, and <b>ggplot2</b>. 
