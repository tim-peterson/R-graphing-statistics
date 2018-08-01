# R-graphing-statistics


```

setwd("/some_dir")


ATRAID_OVX <- read_csv("~/data.csv")

library(ggplot2)

OCS <- na.omit(ATRAID_OVX[,c("Oc.S/BS","X22")])

p <- ggplot(OCS, aes(factor(X22), `Oc.S/BS`, fill=factor(X22), width = 2)

p + geom_boxplot() + geom_point(size = 5,  fill="white", stroke=2, shape=21) + theme_classic() + theme(axis.line = element_line(colour = "#979798")) + scale_fill_manual(values=c("#d1d3d4", "#d1d3d4", "#14426f", "#14426f"), name = "Groups", labels = c("WT-ALN", "WT+ALN", "KO-ALN", "KO+ALN")) + stat_summary(geom = "crossbar", width=0.75, fatten=0, color="#da1a5e", fun.data = function(x){ return(c(y=median(x), ymin=median(x), ymax=median(x))) }) + scale_x_discrete(name ="Groups") 



File <- "img/OCS_v6.pdf"
if (file.exists(File)) stop(File, " already exists")
dir.create(dirname(File), showWarnings = FALSE)

ggsave("OCS_v6.pdf", width = 5, height = 5)

```