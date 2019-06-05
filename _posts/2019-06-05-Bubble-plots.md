---
layout: post
title: "Bubble Plots"
date: 2019-06-05
---


Bubble plots are another way to visualize compositional microbial community data. In a bubble plot the relative abundance of a species in a sample is scaled to match the size of the corresponding point. 


For most analyses in R I find it easiest to upload a csv file that has your species as columns and your samples as rows. Many of the outputs for the 16S pipelines I've seen have the order the other way around, so you may have to manipulate and transpose your data to get it in the right orientation. I do a lot of my early data manipulations in excel <i>(gasp)</i>, because I find it easiest to 1) get a first glance at the data, and 2) visually observe all the changes I'm making. I can also easily add in extra descriptor columns to define my samples on treatments, or environmental conditions, or any other parameter I could be interested in testing. 


![useful image]({{ site.url }}/assets/OTU_table_screenshot.png)

```
#set your working directory by either setwd() or manually in R studio--> Session --> Set Working Directory --> Choose Directory
#upload your data to R - exchange "Your_csv_file.csv" with the name of your csv file
pc = read.csv("Your_csv_file.csv", header = TRUE)

```

I've always found the "wide" and "long" data table formats difficult to explain. The best I can do is that <i> wide </i> has a column for each variable, whereas <i> long </i> has one column for all possible variables, and a column for the corresponding values. This is important for plotting because ggplot only lets you plot one <b>x</b> variable against one <b>y</b> variable. So if you want to plot multiple species simultaneously in one figure, they need to be in one column (rather than each having its own column).  

In the following command, all variables that are not names of OTUs (or species, etc) are placed inside the "id" bracket.  



```
#convert data frame from a "wide" format to a "long" format
pcm = melt(pc, id = "Sample")

```

![useful image]({{ site.url }}/assets/OTU_table_screenshot_long.png)

<b>Pick your colours!</b> The following code allows you to define the colours that you will be using in your figure. Stacked bar charts are limited for data with many variables, because anything over 10 colours on a figure starts to look messy.  I have 11 in the figure below, and you can decide for yourself if this figure is too busy. 

Picking colours that go nicely together can also be a challenge. I find that using colour scheme generators like [this one](https://coolors.co/app), can be a good place to start. R understands six digit hex codes, and select colour names that can be found [here](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf) 


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



