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
xx = ggplot(df, aes(x = Temperature, y = Pelagibacteraceae.OTU_307744)) + 
geom_point(size = 4) + 
labs(y = "Pelagibacteraceae (OTU 307744) (%)", x = "Temperature (C)") + 
theme( axis.text.x = element_text(face = "bold",colour = "black", size = 12), 
axis.text.y = element_text(face = "bold", size = 11, colour = "black"), 
axis.title= element_text(face = "bold", size = 14, colour = "black"), 
panel.background = element_blank(), 
panel.border = element_rect(fill = NA, colour = "black"))
xx 
```

![useful image]({{ site.url }}/assets/Scatter_basic.png)


