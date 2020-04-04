---
layout: post
title: "NMDS Plot extras in R: Envfit"
date: 2020-04-04
---


A **[non-metric multidimensional scaling (NMDS) plot](https://jkzorz.github.io/2019/06/06/NMDS.html)** is one of the many types of ordination plots that can be used to show multidimensional data in 2 dimensions. An NMDS plot basically highlights the similarities between samples of complex multidimensional data. The closer two points (samples) are on the plot the more similar those samples are in terms of the underlying data. In my case, I'm usually looking at species abunance of microorganisms across samples. So the closer two samples are to each other, the more similar they are in terms of their microbial communities.   

The approach to generate an NMDS plot is non-metric, meaning that the algorithm doesn't assume any particular distribution of the data, which is handy when you have data that is inherently weird, and doesn't conform to bell-curve style distributions. The algorithm uses a rank based approach, that essentially ranks your samples in terms of closeness to one another based on a particular distance metric (i.e. Bray Curtis), [more about the specifics of the methods here](https://mb3is.megx.net/gustame/dissimilarity-based-methods/nmds). While this is a robust method for con-conforming data, it means that within an NMDS ordination plot the axes and coordinate system are arbitrary and don't relate to any meaningful values. 

An NMDS is just a visualization technique, and **is not a statistical assessment of sample separation or correlation**. For this you should run an **[ANOSIM test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html)** for categorical variables, or a **[Mantel test](https://jkzorz.github.io/2019/07/08/mantel-test.html)** for continuous variables. Other ordination techniques like PCA, CA, CCA, RDA, etc, may be more useful to you if you want your axes to be meaningful, or if you want to talk about variation partitioning.  

Okay, all that being said. There are ways to overlay extra information on your NMDS plots using the functions **[envfit](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/envfit)**, **[ordiplot](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordiplot)**, **[ordiellipse](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordihull)** and **[ordisurf](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordisurf)**, and I will explain how to use some of their features here. 


**ENVFIT**

Envfit fits environmental vectors or factors onto an ordination. The default plot for envfit, like metaMDS, isn't very aesthetically pleasing, so I will show how you can plot both using **ggplot2**.

First load your libraries and read in your data: 

```
library(vegan)
library(ggplot2)
df = read.csv("your_data_frame.csv", header = TRUE)

```

My data contains samples as rows and columns contain either environmental variables, or species abundance data. In this example, I have both categorical and continuous environmental variables.

![useful image]({{ site.url }}/assets/envfit_csv.png)


Next, subset your data so that you have a data frame with only environmental variables (env), and a data frame with only species abundance data (com). In the code below I'm naming the range of columns that contains the respective information.  

```
com = df[,9:32]
env = df[,2:8]
```

Perform the NMDS ordination, as discussed more in **[this tutorial](https://jkzorz.github.io/2019/06/06/NMDS.html)**

```
m_com = as.matrix(com)

#nmds code
set.seed(123)
nmds = metaMDS(m_com, distance = "bray")
nmds
```

Now we run the envfit function with our environmental data frame, *env*.

```
en = envfit(nmds, env, permutations = 999, na.rm = TRUE)
```
The first parameter is the metaMDS object from the NMDS ordination we just performed. Next is *env* our environmental data frame. Then we state we want 999 permutations of the data, and to remove any rows with missing data. 

Let's see what the envfit function returns: 

```
> en$vectors
               NMDS1    NMDS2     r2 Pr(>r)    
Depth        0.32513 -0.94567 0.7954  0.001 ***
Salinity     0.69000 -0.72381 0.5023  0.001 ***
Temperature  0.75734  0.65303 0.7817  0.001 ***
Oxygen      -0.91051  0.41349 0.7738  0.001 ***
Chlorophyll -0.99684 -0.07940 0.4881  0.001 ***
Latitude    -0.95483 -0.29715 0.1051  0.001 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
Permutation: free
Number of permutations: 999

```

We can also plot both our NMDS and the envfit results using base R: 

```
plot(nmds)
plot(en)
```

![useful image]({{ site.url }}/assets/envfit_base.png)


When we call the results of the envfit function, we get a table that gives our environmental variables as rows. It then gives their respective coordinates on the NMDS ordination in the NMDS1 and NMDS2 axes. 

The printed output of continuous variables (vectors) gives the direction cosines which are the coordinates of the heads of unit length vectors. In plot these are scaled by their correlation (square root of the column r2) so that “weak” predictors have shorter arrows than “strong” predictors

Scores gives scaled relative lengths 
The plotted (and scaled) arrows are further adjusted to the current graph using a constant multiplier: this will keep the relative r2-scaled lengths of the arrows but tries to fill the current plot. You can see the multiplier using ordiArrowMul(result_of_envfit), and set it with the argument arrow.mul. 
Function vectorfit finds directions in the ordination space towards which the environmental vectors change most rapidly and to which they have maximal correlations with the ordination configuration. Function factorfit finds averages of ordination scores for factor levels. Function factorfit treats ordered and unordered factors similarly.
If permutations > 0, the significance of fitted vectors or factors is assessed using permutation of environmental variables.


In order to plot using *ggplot2*, you need to extract the appropriate information from the nmds and envfit results. 

For the NMDS output, use the following code to extract the sample coordinates in the NMDS ordination space. Then add columns from your original data set that contain information that you would like to include in your plot. In this case I'm including the column "season" 

```
data.scores = as.data.frame(scores(nmds))
data.scores$season = df$Season
```

Extracting the required information from the envfit result is a bit less straightforward. The envfit output contains information on the length of the segments for each variable. The segments are scaled to the r2 value, so that the environmental variables with a longer segment are more strongly correlated with the data than those with a shorter segment. You can extract this information with scores. Then these lengths are further scaled to fit the plot. This is done with a multiplier that is analysis specific, and can be accessed using the command *ordiArrowMul(en)*. Below I multiply the scores by this multiplier to keep the coordinates relative to each other. 

Also because my data contained continuous and categorical environmental variables, I'm extracting the information from both separately using the "vectors" and "factors" options respectively. 

```
en_coord_cont = as.data.frame(scores(en, "vectors")) * ordiArrowMul(en)
en_coord_cat = as.data.frame(scores(en, "factors")) * ordiArrowMul(en)
```

Now the data is in the appropriate format to plot in R using ggplot2. 

First we will make a figure of just the nmds plot without the envfit variables: 

```
gg = ggplot(data = data.scores, aes(x = NMDS1, y = NMDS2)) + geom_point(data = data.scores, aes(colour = season), size = 3, alpha = 0.5) + scale_colour_manual(values = c("orange", "steelblue")) + theme(axis.title = element_text(size = 10, face = "bold", colour = "grey30"), panel.background = element_blank(), panel.border = element_rect(fill = NA, colour = "grey30"), axis.ticks = element_blank(), axis.text = element_blank(), legend.key = element_blank(), legend.title = element_text(size = 10, face = "bold", colour = "grey30"), legend.text = element_text(size = 9, colour = "grey30")) + labs(colour = "Season")
```

![useful image]({{ site.url }}/assets/nmds_only.png)


Now we will add the envfit data: 

```
gg = ggplot(data = data.scores, aes(x = NMDS1, y = NMDS2)) + geom_point(data = data.scores, aes(colour = season), size = 3, alpha = 0.5) + scale_colour_manual(values = c("orange", "steelblue"))  + geom_segment(aes(x = 0, y = 0, xend = NMDS1, yend = NMDS2), data = en_coord, size =1, alpha = 0.5, colour = "grey30") + geom_point(data = en_coord_cat, aes(x = NMDS1, y = NMDS2), shape = "diamond", size = 4, alpha = 0.6, colour = "navy") + geom_text(data = en_coord_cat, aes(x = NMDS1, y = NMDS2+0.04), label = row.names(en_coord_cat), colour = "navy", fontface = "bold") + geom_text(data = en_coord_cont, aes(x = NMDS1, y = NMDS2), colour = "grey30", fontface = "bold", label = row.names(en_coord_cont)) + theme(axis.title = element_text(size = 10, face = "bold", colour = "grey30"), panel.background = element_blank(), panel.border = element_rect(fill = NA, colour = "grey30"), axis.ticks = element_blank(), axis.text = element_blank(), legend.key = element_blank(), legend.title = element_text(size = 10, face = "bold", colour = "grey30"), legend.text = element_text(size = 9, colour = "grey30")) + labs(colour = "Season")
```

![useful image]({{ site.url }}/assets/envfit.png)


It's a bit busy still, but looks much nicer than the original base R example. 

Remember you can also use the **[Mantel Test](https://jkzorz.github.io/2019/07/08/mantel-test.html)** or an **[ANOSIM test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html)** (or equivalent) for alternate methods to test the significance of continuous and categorical associations with your data.  


