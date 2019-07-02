---
layout: post
title: "Indicator Species Analysis"
date: 2019-07-02
---

When analyzing microbial data, you may want to identify microbial species that are found more often in one treatment group compared to another.  There are a number of ways to find these differentially abundant species, all of which use slightly different methods. Generally however, I've found that these different methods should all reach similar conclusions. <b>[SIMPER](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/simper) </b>, <b>[mvabund](https://besjournals.onlinelibrary.wiley.com/doi/10.1111/j.2041-210X.2012.00190.x)</b> and <b>[Indicator Species Analysis](https://cran.r-project.org/web/packages/indicspecies/vignettes/indicspeciesTutorial.pdf) </b> (below) are examples of statistical tests used to find differentially abundant species.     


<b>Indicator species are:</b>

<b> <i>"A species whose status provides information on the overall condition of the ecosystem and of other species in that ecosystem. They reflect the quality and changes in environmental conditions as well as aspects of community composition." </i></b> <i>  - United Nations Environment Programme (1996)</i> 

In order to perform indicator species analysis you need an OTU table, or something similar, that contains all the information about your species distributions across your samples. You also need corresponding data that assigns these same samples to groups. The groups you use can be habitat type, treatment, time, etc.  I often use the same groups that I used in the [ANOSIM statistical test](https://jkzorz.github.io/2019/06/11/ANOSIM-test.html). This way I can check to see which species are most responsible for the differences in microbial community composition between my groups.       


There is a specific R package to perform <b>Indicator Species Analysis</b> called <b><i>indicspecies</i></b>, developed by the authors of [this paper](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1600-0706.2010.18334.x). They also have a thorough [tutorial](https://cran.r-project.org/web/packages/indicspecies/vignettes/indicspeciesTutorial.pdf) if you want more information. To begin, load and install the <b><i>indicspecies</i></b> package. Then load in your data. I find it easiest if your data is in the format of columns as OTUs and rows as samples. 

```
install.packages("indicspecies")
library(indicspecies)
pc = read.csv("Your_OTU_table.csv", header= TRUE)
```

![useful image]({{ site.url }}/assets/Wide_extra_variable_excel.png)


In this case, I have one column called "Time", that contains the information for grouping my samples into "Early" and "Late" categories. Next I want to create a data frame with all my abundance data, and a vector that contains the information from the "Time" column:

```
abund = pc[,3:ncol(pc)]
time = pc$Time
```

Because my abundance data starts in the 3rd column of my original "pc" data frame. I put a "3" in the code above. Change this number to match the column where your abundance data starts. Also change "Time" to whatever you have called your grouping variable. 

Next we can run the indicator species command: 

```
 inv = multipatt(abund, time, func = "r.g", control = how(nperm=9999))
```
<b>multipatt</b> is the name of the command from the <b><i>indicspecies</i></b> package. The mulitpatt command results in lists of species that are associated with a particular group of samples. If your group has more than 2 categories, multipatt will also identify species that are statistically more abundant in combinations of categories.   

The parameters for <b>multipatt</b> are as follows: 
<ul>
  <li>the community abundance matrix:<i>"abund"</i></li>
  <li>the vector that contains your sample grouping information: <i>"time"</i></li>
  <li>the function that multipatt is using to identify indicator species: <i> func = "r.g" </i></li>
  <li>the number of permutations used in the statistical test: <i> control = how(nperm=9999)</i></li>  
 </ul>

I almost exclusively use the <i>"r.g"</i> function because it takes abundance information, rather than solely presence/absence information, into account when calculating significance. Depending on your computing power, 9999 permutations might be too many. Feel free to decrease this number. 

To view the results: 

```
summary(inv)
```

The summary function will only return the statistically significant species (<i> p < 0.05</i>). If you are interested in all your species, add <i> alpha = 1 </i> to your command. The output of summary looks like:
 
 ![useful image]({{ site.url }}/assets/Indicspecies.png)
 
 
From this output, I can see that the significance level being reported is <b>0.05</b>, and that the function <b>"r.g"</b> was used. There were <b>39</b> species tested in total. <b>24</b> of these were significantly associated with one group.

The first list contains the species found significantly more often in the <b>"Early"</b> grouping. The <b>#sps 6</b> shows that 6 species were identified as indicators for this group.  The first column contains species names, the next column contains the <b>stat</b> value (higher means the OTU is more strongly associated). The <b>p.value</b> column contains the statistical p values for the species association (lower means stronger significance). The final column shows the significance level, which is explained by the <b>Signif. codes</b> at the bottom of the output.   

Below this first list are the species associated with the other group, <b>"Late"</b>. There are 18 species significantly associated with this group. In both cases, the species are listed in order with strongest association at the top.     

I use these results to help me get an initial overview of the species that might be more interesting to investigate. To display these results visually, I often use a <b>box plot</b> to show the differences in distribution of these identified species between my groupings.


