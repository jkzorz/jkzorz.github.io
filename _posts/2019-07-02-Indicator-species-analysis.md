---
layout: post
title: "Indicator Species Analysis"
date: 2019-07-02
---

When analyzing microbial data, you may often want to identify microbial species that are significantly found more often in one treatment group compared to another.  


Indicator species are:

<b> <i>"A species whose status provides information on the overall condition of the ecosystem and of other species in that ecosystem. They reflect the quality and changes in environmental conditions as well as aspects of community composition." </i></b> <i>  - United Nations Environment Programme (1996)</i> 



<b>Purpose: </b>
  <ul>
    <li><i>To test if there is a statistical difference between the microbial communities of two or more groups of samples. </i></li>
    <li>Null Hypothesis: there is no difference between the microbial communities of your groups of samples. </li>
  </ul>
 

The first test that I'll talk about is the [ANOSIM test](https://sites.google.com/site/mb3gustame/hypothesis-tests/anosim). The ANOSIM test is similar to an ANOVA hypothesis test, but it uses a dissimilarity matrix as input instead of raw data. It is also non-parametric, meaning it doesn't assume much about your data (like normal distribution etc), so it's a good bet for often-skewed microbial abundance data. As a non-parametric test, ANOSIM uses ranked dissimilarities instead of actual distances, and in this way it's a very nice complement to an [NMDS plot](https://jkzorz.github.io/2019/06/06/NMDS.html). The main point of the ANOSIM test is to determine if the differences between two or more groups is significant. In our case, it is used to test if there is a significant difference in the microbial community composition of groups of samples. 

Some examples where you can use an ANOSIM test to test for differences in microbial communities between: 
<ul> 
  <li>Healthy vs sick individuals </li>
  <li>Different sampling environments</li>
  <li>Different seasons</li>
  <li>Treatment vs control groups </li>
  </ul>
  
  
This test is quite straightforward to do in R, and follows much of the same data manipulation as the [NMDS plot](https://jkzorz.github.io/2019/06/06/NMDS.html). First load in your data with columns as OTUs and rows as samples, and install/load the package <b>vegan</b> 

```
install.packages("vegan")
library(vegan)
pc = read.csv("Your_OTU_table.csv", header= TRUE)
```

![useful image]({{ site.url }}/assets/NMDS_table_excel.png)
