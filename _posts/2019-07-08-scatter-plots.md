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

![useful image]({{ site.url }}/assets/Scatter_colour.png)


Now the plot is a bit more exciting. Samples at lower latitudes are pink in colour, while samples at higher latitudes are navy in colour. 


**Adding a regression**

It's obvious that there is a relationship between this species and temperature.  To showcase this better, we can add a regression line using *geom_smooth*. Because the relationship looks linear, I'll use *method="lm"* (for linear model)

```
xx = ggplot(df, aes(x = Temperature, y = Pelagibacteraceae.OTU_307744)) + 
 geom_smooth(method = "lm", alpha = 0.2, colour = "black") + geom_point(aes(colour = Latitude), size = 4) +
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
![useful image]({{ site.url }}/assets/Scatter_colour_lm.png)


*FYI: to statistically support this relationship, perform a linear regression in R to get the R2 and p values*:

```
lm = lm(df$Temperature~df$Pelagibacteraceae.OTU_307744)
summary(lm)
```
The output of this shows the relationship is highly statistically significant (**Adjusted R-squared value: 0.823, p value <<<< 0.05**): 

```
Call:
lm(formula = df$Temperature ~ df$Pelagibacteraceae.OTU_307744)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.2053 -0.9336 -0.5215  0.5028  3.8232 

Coefficients:
                                Estimate Std. Error t value Pr(>|t|)    
(Intercept)                       0.4082     0.4476   0.912    0.372    
df$Pelagibacteraceae.OTU_307744   1.3008     0.1280  10.165 1.45e-09 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.634 on 21 degrees of freedom
Multiple R-squared:  0.8311,	Adjusted R-squared:  0.823 
F-statistic: 103.3 on 1 and 21 DF,  p-value: 1.454e-09
```

**Faceting plots**

Cool, now we have colour and regressions in our scatter plot, but we're still only looking at the relationship of one environmental variable with one species.  What if we wanted to cover a bit more ground, and look at this relationship over multiple species? This is where **faceting** comes in handy. 


