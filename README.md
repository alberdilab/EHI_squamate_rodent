# EHI_squamate_rodent
Repository for comparative metagenomics on EHI squamate and rodent host species.

The raw code used for data analysis are in the **.Rmd** files stored in the `code` directory of this repository, while the bookdown-rendered webbook is available at:

https://alberdilab.github.io/EHI_squamate_rodent/

For a detailed overview of the data stored in this repository, see [this readme file](https://github.com/alberdilab/EHI_squamate_rodent/tree/main/data_files_description.md).

For a summary of the samples and host species used, [see here](https://github.com/alberdilab/EHI_squamate_rodent/blob/main/code/supplemental/supp_host_species_representation.md). [Here](https://github.com/alberdilab/EHI_squamate_rodent/blob/main/code/supplemental/supp_genomes_across_hosts.md) is also a description of bacterial genomes shared across different host species.

While the webbook provides a user-friendly overview of the procedures, analyses can be directly reproduced using the .Rmd documents. Note that the code chunks that require heavy computation have been tuned off using 'eval=FALSE'. To re-render the webbook, you can use the following code:

```r
library(bookdown)
library(htmlwidgets)
library(webshot)

render_book(input = "code/", output_format = "bookdown::gitbook", output_dir = "docs/")
```

