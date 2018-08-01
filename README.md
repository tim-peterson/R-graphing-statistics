# R-graphing-statistics


```
ATRAID_OVX <- read_csv("~/data.csv")

library(ggplot2)

p <- ggplot(ATRAID_OVX, aes(`BV/TV%`, X2))

p + geom_boxplot() + geom_jitter(width = 0.2) + theme_bw() + theme(panel.border = element_blank(), panel.grid.major = element_blank(),panel.grid.minor = element_blank(), axis.line = element_line(colour = "black")) + scale_fill_manual(values=c("#d1d3d4", "#d1d3d4", "#14426f", "#14426f")) + stat_summary(geom = "crossbar", width=0.75, fatten=0, color="#da1a5e", fun.data = function(x){ return(c(y=median(x), ymin=median(x), ymax=median(x))) })
```