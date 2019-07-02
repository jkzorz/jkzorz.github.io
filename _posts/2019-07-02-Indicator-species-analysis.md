---
layout: post
title: "Indicator Species Analysis"
date: 2019-07-02
---

When analyzing microbial data, you may often want to identify microbial species that are significantly found more often in one treatment group compared to another.  There are a number of ways to find these differentially abundant species, all of which use slightly different methods. Generally however, I've found that these different methods should all reach similar conclusions. <b>[SIMPER](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/simper) </b>, and <b>[Indicator Species Analysis](https://cran.r-project.org/web/packages/indicspecies/vignettes/indicspeciesTutorial.pdf)</b> (below) are examples of statistical tests used to find differentially abundant species.     


Indicator species are:

<b> <i>"A species whose status provides information on the overall condition of the ecosystem and of other species in that ecosystem. They reflect the quality and changes in environmental conditions as well as aspects of community composition." </i></b> <i>  - United Nations Environment Programme (1996)</i> 

In order to perform indicator species analysis you need an OTU table, or something similar, that contains all the information about your species distributions across your samples. You also need corresponding data that assigns these same samples to groups. The groups you use can be habitat type, treatment, time, etc.  I often use the same groups that I used in the [ANOSIM statistical test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html). This way I can check to see which species are most responsible for the differences in microbial community composition between my groups.       



 

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
