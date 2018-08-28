
# get intersection of multiple data frame columns (lists of genes)

	intersect <- Reduce(intersect, list(tolower(aging_mouse_liver$gene_name),tolower(aging_rat_kidney$X__1)))

# write this data to file
write.csv(intersect, file = "/Users/timrpeterson/OneDrive - Washington University in St. Louis/Data/Aging/intersect_liver_kidney.csv")