---
layout: post
title: "Bubble Plots"
date: 2019-06-05
---


Bubble plots are another way to visualize compositional microbial community data. In a bubble plot the relative abundance of a species in a sample is scaled to match the size of the corresponding point.  

The data manipulation steps for this bubble plot are the same as for the [stacked bar chart](https://jkzorz.github.io/2019/06/05/stacked-bar-plots.html). Check out the [stacked bar chart](https://jkzorz.github.io/2019/06/05/stacked-bar-plots.html) post for a more in-depth description of the steps, see below for the abridged version.  

Again, start by setting your directory, uploading a csv file with species as columns and samples as rows. 

![useful image]({{ site.url }}/assets/OTU_table_screenshot.png)

```
#set your working directory by either setwd() or manually in R studio--> Session --> Set Working Directory --> Choose Directory
#upload your data to R - exchange "Your_csv_file.csv" with the name of your csv file
pc = read.csv("Your_csv_file.csv", header = TRUE)

```

Change your data structure from a "wide" format to a "long" format. Put any variables that are not OTUs/species, into the "id" parameter


```
#convert data frame from a "wide" format to a "long" format
pcm = melt(pc, id = c("Sample"))

```



Pick your colours: 

```
colours = c( "#A54657",  "#582630", "#F7EE7F", "#4DAA57","#F1A66A","#F26157", "#F9ECCC", "#679289", "#33658A", "#F6AE2D","#86BBD8")
```

Plot it! For a bubble plot, you are using the geom_point and scaling the size to your value (relative abundance) variable. 

```
xx = ggplot(pcm, aes(x = Sample, y = variable)) + 
  geom_point(aes(size = value, fill = variable), alpha = 0.75, shape = 21) + 
  scale_size_continuous(limits = c(0.000001, 80), range = c(1,17), breaks = c(1,10,50,75)) + 
  labs( x= "", y = "", size = "Relative Abundance (%)", fill = "")  + 
  theme(legend.key=element_blank(), axis.text.x = element_text(colour = "black", size = 12, face = "bold", angle = 90, vjust = 0.3, hjust = 1), axis.text.y = element_text(colour = "black", face = "bold", size = 11), legend.text = element_text(size = 10, face ="bold", colour ="black"), legend.title = element_text(size = 12, face = "bold"), panel.background = element_blank(), panel.border = element_rect(colour = "black", fill = NA, size = 1.2), legend.position = "right") +  
  scale_fill_manual(values = colours, guide = FALSE) + 
  scale_y_discrete(limits = rev(levels(pcm$variable))) 

xx
```


![useful image]({{ site.url }}/assets/For_blog_bubble.png)



