---
layout: post
title: "ANOSIM Test"
date: 2019-06-11
---


Statistics can be a scary word, especially, if you're like me and it's been a long time since you've had anything to do with probability or math. For the most part, I don't think it's necessary to understand all the equations behind the statistics that you do, but I do think it's important to know that you're using the right test for your data, and that you're not making any assumptions about your data that aren't supported.  Again, I find that the [GUSTAME](https://sites.google.com/site/mb3gustame/) website is a fantastic resource for learning about the basics of whatever test you decide to do.  

<h2> ANOSIM Test </h2>

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
