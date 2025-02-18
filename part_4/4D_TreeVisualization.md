
# TreeVisualization

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


Metadata<-read.csv("~/Documents/C-PGME/Workshop2022/WorkshopMaterials_Lorenzo/Phylodynamics/SARSCoV2_BA4.metadata.tsv",sep = "\t", header = TRUE, stringsAsFactors = T)

time_tree <- read.nexus("~/Documents/C-PGME/Workshop2022/WorkshopMaterials_Lorenzo/Phylodynamics/2022-05-03_mugration/annotated_tree.nexus")
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


