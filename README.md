# R-graphing-statistics 

#### (demo'd on data from mouse bone [micro-computed tomography (uCT)](http://www.musculoskeletalcore.wustl.edu/content/Core/2976/B-Structure-and-Strength-Core/Services/Technical-Notes-and-Presentations.aspx))

Download R: [Windows](https://cran.r-project.org/bin/windows/base/R-3.5.1-win.exe) | [Mac](https://cran.r-project.org/bin/macosx/R-3.5.1.pkg)

Download R studio: [Windows](https://download1.rstudio.org/RStudio-1.1.456.exe) | [Mac](https://download1.rstudio.org/RStudio-1.1.456.dmg)

## Import data

There are two ways to import data. (1) You can pick the data file from the R Studio File menu or (2) you can use the R Studio console. I prefer to use the console because it will help new users get more comfortable working with the R language. 

*Console (preferred):* It's a good idea to start by setting the working directory. This is both so you know the relative path R Studio will be grabbing the data file from and because this is where `ggsave()` saves the graphs you'll be generating, see below).

Set working directory to some folder.

`setwd("/Users/timrpeterson/r/R-graphing-statistics/graphs")`

Read the data into a dataframe `b`.

`bvtv <- read_csv("../bvtv.csv")`

*File menu*: Click File -> Import Dataset -> From CSV -> click on the data file. This will import your data and store it locally as a data.frame. By default, the name of the data.frame will be the same as the file name.

## Statistics

Run ANOVA - analysis of variance - statistically test. [More details](https://github.com/tim-peterson/R-graphing-statistics/blob/master/Analysis%20of%20variance.pdf). You'll notice I intentionally gave the header columns bad names so that you can see a more real world scenario. `BV/TV%` needs to be escaped using backticks `` ` `` and the groups column didn't have a header name so R filled in `X2` for us. See [Notes](#notes) at the bottom to learn more about `factor()` and the tilde `~`.

``result=aov(`BV/TV%`~factor(X2),data=bvtv)``

``pairwise.t.test(bvtv$`BV/TV%`,bvtv$X2,p.adjust="holm")``

This should output something like the following with p-values for the pairwise comparisons of the X number of groups you have.

	  1       2       3       4      5     
	2 0.8364  -       -       -      -     
	3 1.4e-06 2.9e-09 -       -      -     
	4 1.0000  1.0000  1.3e-07 -      -     
	5 1.0000  1.0000  1.5e-08 1.0000 -     
	6 0.0767  0.0011  0.0119  0.0119 0.0036

This means that there is a highly statistically significant difference when comparing groups 2 and 3, `2.9e-09` (assuming a p <= 0.05 threshold). Whereas, groups 1 and 2 aren't statistically different, `0.8364`. 	

One can also try Tukey's method. 

`tt <- TukeyHSD(result,conf.level=0.95)`

`print(tt)`

	           diff        lwr        upr     p adj
	2-1  -4.0608333 -12.058594   3.936928 0.6656398
	3-1  17.7488889   9.166597  26.331180 0.0000017
	4-1  -2.4088889 -10.991180   6.173403 0.9607489
	5-1  -3.1636364 -11.324968   4.997695 0.8600899
	6-1   7.6544444  -0.927847  16.236736 0.1062980
	3-2  21.8097222  13.573170  30.046275 0.0000000
	4-2   1.6519444  -6.584608   9.888497 0.9911219
	5-2   0.8971970  -6.899752   8.694145 0.9993653
	6-2  11.7152778   3.478725  19.951830 0.0013370
	4-3 -20.1577778 -28.963023 -11.352533 0.0000001
	5-3 -20.9125253 -29.307997 -12.517053 0.0000000
	6-3 -10.0944444 -18.899690  -1.289199 0.0158559
	5-4  -0.7547475  -9.150219   7.640724 0.9998102
	6-4  10.0633333   1.258088  18.868579 0.0163264
	6-5  10.8180808   2.422609  19.213553 0.0046312


Once again you can see a highly statistically significant difference when comparing groups 2 and 3 (`3-2`), `0.0000000` (assuming a p <= 0.05 threshold). Whereas, groups 1 and 2 (`2-1`) aren't statistically different, `0.6656398`. 	You can see that's similiar to using the Holm's method. The p-values are only shown to 8 decimal points so to get the exact value you can print to whatever decimal places you need. 

`print(tt,digits=15)`

## Graphing

Load [ggplot](https://cran.r-project.org/web/packages/ggplot2/index.html) graphing library. If it's not installed, run this: `install.packages(ggplot2)`. For some examples on how to use ggplot, go [here](https://ggplot2.tidyverse.org/reference/geom_boxplot.html#examples). Interestingly enough, R comes installed with an example data.frame of data `mpg` that you can play with.

`library(ggplot2)`  

Store the graph as a variable, e.g., `p`.

``p <- ggplot(bvtv, aes(factor(X2), `BV/TV%`, fill=factor(X2)))``

This is where all the magic happens. I.e., R Studio graphs your data. See [Notes](#notes) below.


	p + geom_boxplot() + geom_point(size = 5,  fill="white", stroke=2, shape=21) + geom_jitter(width = 0.2, size = 5,  fill="white", stroke=2, shape=21) + theme_bw() + theme(panel.border = element_blank(), panel.grid.major = element_blank(),panel.grid.minor = element_blank(), axis.line = element_line(colour = "#979798")) + scale_fill_manual(values=c("#ffffff", "#d1d3d4", "#d1d3d4", "#ffffff", "#14426f", "#14426f"), name = "Groups", labels = c("WT-SHAM", "WT+OVX", "WT+OVX+ALN", "KO-SHAM", "KO+OVX", "KO+OVX+ALN")) + stat_summary(geom = "crossbar", width=0.75, fatten=0, color="#da1a5e", fun.data = function(x){ return(c(y=median(x), ymin=median(x), ymax=median(x))) }) + scale_x_discrete(name ="Group")


Save the file to your current working directory. 

`ggsave("bvtv.pdf", useDingbats=FALSE)`

### Notes

`` `BV/TV%`~X2`` the tilde `~` is to set `X2` as the dependent variable and `BV/TV%` as the independent variable. This is like telling R, what's the X and Y axis.

`factor()` is only required if your groups are numbers, "continuous variable", but you want them to be treated as a label. In our data, we've numbered our groups: `1,2,3,4,5,6` and group 1 is not smaller than group 6. It's just a different group. 

`shape=21` was required to have hollow data points. 

`scale_fill_manual()` is needed to hard code the boxplot fill colors because I wanted a specific pattern.

`stat_summary()` was used mainly to give the median line a different color than the other border colors.


If your data.frame, let's say its called `uCT`, has multiple parameters and `na` values because the row lengths are unequal, you can omit `na` values in the `uCT` data.frame and generate a `BVTV` dataframe that is a subset of `uCT` that includes just the BV/TV% data.

`BVTV <- na.omit(uCT[,c("BV/TV%","X2")])`

