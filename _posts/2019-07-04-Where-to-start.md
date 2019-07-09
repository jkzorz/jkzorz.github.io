---
layout: post
title: "Where to start?"
date: 2019-07-04
---


It can be daunting to know where to start with your data analysis once you've received your (likely huge) amplicon sequencing dataset. Below I've divided the R tutorials into categories based on what you may be looking to accomplish: 


<h3>Data Visualization</h3>

  - **[Stacked bar plots](https://jkzorz.github.io/2019/06/05/stacked-bar-plots.html)**: For displaying compositional data (i.e. relative abundance of species) of discrete samples 
  
  - **[Bubble plots](https://jkzorz.github.io/2019/06/05/Bubble-plots.html)**: For displaying continuous data (i.e. relative abundance of species) of discrete samples
  
  - **[Box plots](https://jkzorz.github.io/2019/07/02/boxplots.html)**: For displaying information on the distribution of continuous data (i.e. relative abundance, diversity) across a dataset (or dataset subset) for a specific set of species or samples
  
  - **[Heatmaps](https://jkzorz.github.io/2019/06/11/Correlation-heatmaps.html)**: For displaying the continuous values (i.e. relative abundance or correlation) associated with discrete data (i.e. samples or species) 
  
  - **[Scatter plots](https://jkzorz.github.io/2019/07/09/scatter-plots.html)**: For displaying two continuous variables (i.e. abundance of species 1 vs species 2, or abundance of a species vs temperature, etc) 




<h3>Statistical Testing</h3>

Statistics can be a scary word, especially, if you're like me and it's been a long time since you've had anything to do with probability or math. For the most part, I don't think it's necessary to understand all the equations behind the statistics that you do, but I do think it's important to know that you're using the right test for your data, and that you're not making any assumptions about your data that aren't supported.  Again, I find that the [GUSTAME](https://sites.google.com/site/mb3gustame/) website is a fantastic resource for learning about the basics of whatever test you decide to do.  

  - **[ANOSIM test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html)**: Analysis of similarities test. A non-parametric test, used to determine if there are statistical differences in the microbial communities of two or more discrete groups of samples
  
  - **[Mantel test](https://jkzorz.github.io/2019/07/08/mantel-test.html)**: Used to calculate correlations between corresponding positions in two (dissimilarity/distance) matrices. Often used to test if the differences in microbial community structure across samples (i.e. Bray curtis dissimilarity) is correlated with the differences in another factor (i.e. temperature, physical separation) across samples
  
  - **[NMDS plot](https://jkzorz.github.io/2019/06/06/NMDS.html)**: Non-metric multidimensional scaling. A non-parametric ordination technique used to visually display multidimensional data (i.e. abundances of many species over many samples) in two dimensions. Points (generally samples) that are closer together have more similar microbial communities
  
  - **[Indicator Species test](https://jkzorz.github.io/2019/07/02/Indicator-species-analysis.html)**: A test used to determine "indicator species" of a particular group of samples. Identifies the species that are found statistically more abundantly in one group of samples compared to others 
  
  - **[Correlation analyses](https://jkzorz.github.io/2019/06/11/Correlation-heatmaps.html)**: A test that identifies correlations between variables in your dataset. These correlative relationships can be species/species or species/environmental factor.   
  
  
  
  
  



