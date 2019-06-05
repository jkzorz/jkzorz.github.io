---
layout: post
title: "Bubble Plots"
date: 2019-06-05
---


Bubble plots are another way to visualize compositional microbial community data. In a bubble plot the relative abundance of a species in a sample is scaled to match the size of the corresponding point. 


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
