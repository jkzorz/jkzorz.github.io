---
layout: post
title: "Indicator Species Analysis"
date: 2019-07-02
---

When analyzing microbial data, you may often want to identify microbial species that are significantly found more often in one treatment group compared to another.  There are a number of ways to find these differentially abundant species, all of which use slightly different methods. Generally however, I've found that these different methods should all reach similar conclusions. <b>[SIMPER](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/simper) </b>, and <b>[Indicator Species Analysis](https://cran.r-project.org/web/packages/indicspecies/vignettes/indicspeciesTutorial.pdf)</b> (below) are examples of statistical tests used to find differentially abundant species.     


Indicator species are:

<b> <i>"A species whose status provides information on the overall condition of the ecosystem and of other species in that ecosystem. They reflect the quality and changes in environmental conditions as well as aspects of community composition." </i></b> <i>  - United Nations Environment Programme (1996)</i> 

In order to perform indicator species analysis you need an OTU table, or something similar, that contains all the information about your species distributions across your samples. You also need corresponding data that assigns these same samples to groups. The groups you use can be habitat type, treatment, time, etc.  I often use the same groups that I used in the [ANOSIM statistical test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html). This way I can check to see which species are most responsible for the differences in microbial community composition between my groups.       


There is a specific R package to perform <b>Indicator Species Analysis</b> called <b><i>indicspecies</i></b>, developed by the authors of [this paper](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1600-0706.2010.18334.x). They also have a thorough [tutorial](https://cran.r-project.org/web/packages/indicspecies/vignettes/indicspeciesTutorial.pdf) if you want more information. To begin, load and install the <b><i>indicspecies</b></i> package. Then load in your data. I find it easiest if your data is in the format of columns as OTUs and rows as samples. 

```
install.packages("indicspecies")
library(indicspecies)
pc = read.csv("Your_OTU_table.csv", header= TRUE)
```

![useful image]({{ site.url }}/assets/NMDS_table_excel.png)
