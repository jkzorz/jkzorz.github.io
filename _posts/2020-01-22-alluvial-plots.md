---
layout: post
title: "Alluvial plots in R"
date: 2020-01-22
---

Alluvial plots are similar to a stacked bar plot, but look fancier and can be more aesthetically pleasing. They're useful if you want to show how a set of variables change over time, or over a set of conditions. At each time point or category along an axis, the variables are arranged in decreasing order of value, so it's easy to track how a variable changes with respect to the others in your data. 

So to make an alluvial plot, as per usual, we first need to load the required packages: 

```
library(ggplot2)
library(ggalluvial)
library(reshape2)
```

Next, load your data into R as a data frame. In this case, OTUs/species are columns, samples are rows. In addition to the first column which contains the Sample name, I have also added descriptor columns including "Origin", "Day", "Day_num", and "Time", before the OTUs/species columns. In the following code, substitute the name of your csv file for "your_data.csv"    

```
bin = read.csv("your_data.csv", header = TRUE)
```

*excel image*

Now we need to convert the data from a "wide" format to a "long" format. The difference is explained here. 

```
bmm = melt(bm, id = c("Sample", "Origin", "Day", "Day_num", "Time"))
```

Choose your colours for the figure. The following colours are written in hex code, which is a way to represent colours in computer language. You can also choose from a range of colours that are built into R, (http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf), but hex code will give you more flexibility. There are many colour scheme generators online that will also give you the hex code (i.e. https://coolors.co/app)  

I have 8 species in this data, so I need 8 colours 

```
colours = c("#418C6D", "#B11776","#F795A4", "#166BAF", "#F5D79E", "#AF794D", "#9C9CAF", "#1B1A54")
```

The next bit of code keeps the "Day" variable in the same order as the excel sheet. It's a bit of a workaround to an R default, where the levels of a variable are automatically arranged alphabetically. Alphabetically, Day10 comes before Day2, which obviously isn't what we want. 

```
bmm$Day <- factor(bmm$Day,levels=unique(bmm$Day))
```

The following code plots the alluvial figure. *geom_alluvium* specifies that we want to add the alluvium layer to the figure. The alluvium (plural: alluvia) are what is linked across the multiple time points or conditions. In this case the alluvia will be the "variable" which contain the OTUs/species from the original data. 

```
 xx = ggplot(bmm, aes( x = Day, y = value, alluvium = variable)) + 
	geom_alluvium(aes(fill = variable), colour = "black", alpha = 1, decreasing = FALSE) + 
	labs(x = "", y = "Percent of binned community (%)", fill = "", colour = "") + 
	scale_y_continuous(expand = c(0,0), limits = c(0,100)) + 
	scale_x_discrete(expand = c(0.02,0.02)) + 
	theme(legend.position = "top", axis.text.y = element_text(colour = "black", size = 12, face = "bold"), 
	axis.text.x = element_text(colour = "black", face = "bold", angle =90, size = 12, vjust = 0.5), 
	axis.title.y = element_text(colour = "black", face = "bold", size = 14), 
	panel.background = element_blank(), panel.border = element_rect(colour = "black", fill = NA, size = 1.2), 
	legend.text = element_text(size = 10, face = "bold")) + 
	scale_fill_manual(values = colours))

xx
```

Save with ggsave:

```
```

