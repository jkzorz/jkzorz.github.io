---
layout: post
title: "Stacked Bar Charts"
date: 2019-06-05
---

The stacked bar chart has been the poster child <i>(literally on every single microbial ecology poster in the past 5 years)</i> of compositional microbial community data visualizations. particularly if you are using 16S amplicon sequencing 

<b> Disclaimer: </b> I am in no way an R code expert, I wouldn't even really consider myself to be at the amateur level. Everything I use has basically worked for me once, and so I keep adapting it over time as needed. Sometimes I try new things, but mainly I stick to what I know. That being said, there are probably 100s of better/different ways to do things in R than what I do, but all the code I'm sharing is what has worked for me and is what I've become comparatively comfortable with.  


Install and load ggplot2 (for plotting) and reshape2 (for data manipulation) packages. 
<i> Alternatively, packages like tidyr and dplyr are also standards for data manipulation (see disclaimer above)</i>

```
install.packages("ggplot2")
install.packages("reshape2")
library(ggplot2)
library(reshape2)

```





```
#convert data frame from a "wide" format to a "long" format
pcm = melt(pc, id = c("Bin", "Phylum", "Taxonomy", "Cluster"))

```

<b>Pick your colours!</b> The following code allows you to define the colours that you will be using in your figure. Stacked bar charts are limited for data with many variables, because anything over 10 colours on a figure starts to look messy.  I have 11 in the figure below, and you can decide for yourself if this figure is too busy. 

Picking colours that go nicely together can also be a challenge. I find that using colour scheme generators like [this one](https://coolors.co/app), can be a good place to start. R understands six digit hex codes, and select colour names that can be found [here](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf) 


P.S. A better way to display abundance data for many (10+) variables/species/OTUs is a [Bubble plot](http://blog/Bubble).   

```
#define the colours to use in the figure
colours = c( "#A54657",  "#582630", "#F7EE7F", "#4DAA57","#F1A66A","#F26157", "#F9ECCC", "#679289", "#33658A", "#F6AE2D","#86BBD8")

```

mx = ggplot(pcm, aes(x = variable, fill = Cluster, y = value)) + 
    geom_bar(stat = "identity", colour = "black") + 
    theme(axis.text.x = element_text(angle = 90, size = 14, colour = "black", vjust = 0.5, hjust = 1, face= "bold"), axis.title.y = element_text(size = 16, face = "bold"), legend.title = element_text(size = 16, face = "bold"), legend.text = element_text(size = 12, face = "bold", colour = "black"), axis.text.y = element_text(colour = "black", size = 12, face = "bold")) + 
    scale_y_continuous(expand = c(0,0))  + labs(x = "", y = "Relative Abundance (%)", fill = "Class") + 
    geom_vline(xintercept = 6.5, size = 1.2, colour = "black") + 
    scale_fill_manual(values = colours)
    
mx

```


![useful image]({{ site.url }}/assets/Autofermentation_bar_at_least_5pc_BM.png)


powered by [Jekyll](http://jekyllrb.com) 
