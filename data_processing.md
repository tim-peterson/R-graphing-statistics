
## Import data

Find list of genes that are differentially expressed. They are often in supplemental tables, such as Excel files. R Studio can work on many types of files including Excel files, CSV, TSV files.

[Mouse liver, PMID: 26541291](https://www.ncbi.nlm.nih.gov/pubmed/26541291)

	aging_mouse_liver <- read_excel("~/Downloads/12864_2015_2061_MOESM2_ESM.xlsx")

[Rat kidney, PMID: 27153548](https://www.ncbi.nlm.nih.gov/pubmed/27153548)

	aging_rat_kidney <- read_excel("~/Downloads/oncotarget-07-30037-s003.xls")

## Get intersection of multiple data frame columns (lists of genes)

	gene_intersection <- Reduce(intersect, list(tolower(aging_mouse_liver$gene_name),tolower(aging_rat_kidney$X__1)))

`list()` can take more than two gene lists.

`tolower()` puts all gene names as lowercase to ensure that the intersection doesn't fail due to capitalization.

## Write this data to file

	write.csv(gene_intersection, file = "/Users/timrpeterson/OneDrive - Washington University in St. Louis/Data/Aging/intersect_liver_kidney.csv")

## To-do	

1. get gene homolog tables (from BioMart?) to convert non-mammalian species gene names to their mammalian homologs. This will enable us to leverage experiments done in worms, yeast, fish, etc.

2. Identify a list of transcription factors that control aging-associated genes common to multiple datasets.

