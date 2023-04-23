
# TreeVisualization

### April 24, 2023

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)*   

### Click here to view the full R script: [TreeVisualization.Rmd](TreeVisualization.Rmd)

### The expected output of the commands is found here: [TreeVisualization.pdf](TreeVisualization.pdf)

```R
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install("treeio")
BiocManager::install("ggtree")
BiocManager::install("ggtreeExtra")

library("treeio")
library("ggtree")
library("ape")
library("ggtreeExtra")
library(ggplot2)
library(ggsci)


Metadata<-read.csv("/path/to/SARSCoV2_BA4.metadata.tsv",sep = "\t", header = TRUE, stringsAsFactors = T)

time_tree <- read.nexus("/path/to/annotated_tree.nexus")
TreeData<-as.data.frame(as_tibble(time_tree))
names(TreeData)[4]<-"strain"

Metadata_Tree<-merge(TreeData,Metadata,by="strain",sort = F)
rownames(Metadata_Tree) <- Metadata_Tree$strain


p<-ggtree(time_tree,layout = "circular")%<+% Metadata_Tree +
  geom_tippoint(aes(color=country))

p

p + geom_fruit(
  geom=geom_tile,
  mapping=aes(fill=Metadata_Tree$region),
  width=0.2,offset = 0.2)+scale_fill_lancet()

p + geom_fruit(
  geom=geom_tile,
  mapping=aes(fill=Metadata_Tree$sex),
  width=0.2,offset = 0.2)+scale_fill_npg()

p + geom_fruit(
  geom=geom_tile,
  mapping=aes(fill=as.numeric(as.character(Metadata_Tree$age))),
  width=0.2,offset = 0.2)+scale_fill_gradient2()


```
---

# [Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

