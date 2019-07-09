---
layout: post
title: "Scatter plots in R"
date: 2019-07-09
---


**Scatter plots** are basic plots showing the relationship between two (usually continuous) variables. Making scatter plots in R is relatively straightforward using the **ggplot2** package. There are lots of more in-depth tutorials on using the ggplot2 package online. [Here is one example](https://ggplot2.tidyverse.org/reference/).  


Start by loading the **ggplot2** library, and then loading in your data with columns as OTUs/variables, and rows as samples. 

```
library(ggplot2)
df = read.csv("Your_OTU_table.csv", header= TRUE)
```

**The data I'm using looks like this:**


![useful image]({{ site.url }}/assets/Excel_for_mantel.png)


The first column is sample name, the next 4 columns contain environmental parameters for each sample (i.e. Salinity, Temperature, etc). Then, the following 2 columns contain the latitude and longitude for each sample, and the remaining columns contain the 200+ OTU abundances that correspond to each sample.

**Basic Scatter plot**

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

This is a pretty bare-boned plot with Temperature on the x-axis and the species abundance on the y-axis. In the code:

- *ggplot* specifies the data frame I'm using and the aesthetics (aes) parameter
    - *aes* specifies how variables in the data are mapped to visual properties of the plot. *aes* can also be used within geoms 
- *geom_point* specifies that I want a scatter plot
    - *geoms* are the visual representations of observations. In other words, geoms tell ggplot how you want your data to be plotted (i.e. bar, point, line, etc).[There are many different geoms](https://ggplot2.tidyverse.org/reference/#section-layer-geoms).
- *labs* specifies the labels of my axes
- *theme* specifies all the parameters that contribute to the plot's theme (i.e. text size, plot backgrounds, etc)

*With ggplot2 figures, the best way to save is using **ggsave**:*
```
ggsave("Your_plot_name.svg")
```

**Adding colour**

The previous plot was pretty boring, so let's make things more interesting... One of the most advantageous things about ggplot2 is how easy it is to map extra variables to different aesthetics, like colour, shape, or size.  In the plot below, I want to include information about the sample's *latitude* to my plot, so I map the aesthetic *colour* within *geom_point* to latitude: 

```
xx = ggplot(df, aes(x = Temperature, y = Pelagibacteraceae.OTU_307744)) + 
geom_point(aes(colour = Latitude), size = 4) + 
labs(y = "Pelagibacteraceae (OTU 307744) (%)", x = "Temperature (C)") + 
theme( axis.text.x = element_text(face = "bold",colour = "black", size = 12), 
axis.text.y = element_text(face = "bold", size = 11, colour = "black"), 
axis.title= element_text(face = "bold", size = 14, colour = "black"), 
panel.background = element_blank(), 
panel.border = element_rect(fill = NA, colour = "black"), 
legend.title = element_text(size =12, face = "bold", colour = "black"),
legend.text = element_text(size = 10, face = "bold", colour = "black")) +
scale_colour_continuous(high = "navy", low = "salmon")

xx
```




