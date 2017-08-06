---
title: "Stacked barplot using ggplot2"
date: 2017-08-06T22:26:48+02:00
draft: false
---

```r
library(ggplot2)
library(reshape2)
mt_rel <- read.delim("mt_rel_gt2_forR.txt", sep = "\t", stringsAsFactors = F)

colnames(mt_rel) <- gsub("^X", "", colnames(mt_rel))

mt_rel_melt <- melt(mt_rel,id.vars = "Species", variable.name = "samples", value.name = "Abd")

#library(RColorBrewer)
mt_rel_melt$samples <- factor(mt_rel_melt$samples, levels=rev(levels(mt_rel_melt$samples)))
ggplot(mt_rel_melt, aes(x = samples, y = Abd, fill = Species)) + 
  geom_bar(stat = "identity", width = 0.7) + ylab("% abundance") + 
  theme_bw() +
  theme(text = element_text(size=17, face = "bold")) + 
  scale_fill_ucscgb(alpha = 0.8) +coord_flip()
  
ggsave("./sample_kraken_composition_rel.pdf", width = 10, height = 8)

```

