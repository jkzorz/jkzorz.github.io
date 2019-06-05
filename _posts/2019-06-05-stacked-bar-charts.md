---
layout: post
title: "Stacked Bar Charts"
date: 2019-06-05
---

Well. Finally got around to putting this old website together. Neat thing about it - powered by [Jekyll](http://jekyllrb.com) and I can use Markdown to author my posts. It actually is a lot easier than I thought it was going to be.

```

pcm = melt(pc, id = c("Bin", "Phylum", "Taxonomy", "Cluster"))

colours = c( "#A54657",  "#582630", "#F7EE7F", "#4DAA57","#F1A66A","#F26157", "#F9ECCC", "#679289", "#33658A", "#F6AE2D","#86BBD8", "steelblue4", "grey30", "yellow", "aquamarine", "moccasin", "blue", "darkolivegreen")

mx = ggplot(pcm, aes(x = variable, fill = Cluster, y = value)) + geom_bar(stat = "identity", colour = "black") + theme(axis.text.x = element_text(angle = 90, size = 14, colour = "black", vjust = 0.5, hjust = 1, face= "bold"), axis.title.y = element_text(size = 16, face = "bold"), legend.title = element_text(size = 16, face = "bold"), legend.text = element_text(size = 12, face = "bold", colour = "black"), axis.text.y = element_text(colour = "black", size = 12, face = "bold")) + scale_y_continuous(expand = c(0,0))  + labs(x = "", y = "Relative Abundance (%)", fill = "Class") + geom_vline(xintercept = 6.5, size = 1.2, colour = "black") + scale_fill_manual(values = colours)

```


![useful image]({{ site.url }}/assets/Autofermentation_bar_at_least_5pc_BM.png)
