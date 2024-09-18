# Creating the R environment

Make sure you are in the folder with the code:

- `analyze_correlation_results.Rmd`
- `correlation_workflow.Rmd`

# R environment

This was tested on R=4.4.1

It is optional to use Posit Package Manager. We use ubuntu 22 so we set up to:
```
# Configure BioCManager to use Posit Package Manager:
options(BioC_mirror = "https://packagemanager.posit.co/bioconductor/latest")
options(BIOCONDUCTOR_CONFIG_FILE = "https://packagemanager.posit.co/bioconductor/latest/config.yaml")
# Configure a CRAN snapshot compatible with Bioconductor 3.20:
options(repos = c(CRAN = "https://packagemanager.posit.co/cran/__linux__/jammy/latest"))
```

```r
install.packages("BiocManager")
BiocManager::install("devtools")
BiocManager::install(c("usethis", "rmarkdown", "knitr", "reticulate",
                       "reshape2", "RColorBrewer",
                       "ggplot2", "tidyverse", "glue", "gridExtra"))
BiocManager::install("pheatmap")
BiocManager::install("Seurat")
BiocManager::install("mojaveazure/seurat-disk")
BiocManager::install("SAVER")
devtools::install_github("ChangSuBiostats/CS-CORE")
```

```r
usethis::use_course('https://github.com/KrishnaswamyLab/MAGIC/archive/master.zip',
                    destdir=".")
devtools::install_local("MAGIC-master/Rmagic/")
```
## python environment

In order to run the correlation workflow, `reticulate` needs to be used as MAGIC is a python based tool. In order to recreate a conda environments that is compatible with running the workflow, you can create a new conda environment with `requirements.txt` file included in this repository. The following code can be used to create said  environment:

```r
library(reticulate)
virtualenv_create("magic2",  packages="numpy==1.26",python_version="3.9")
virtualenv_install("magic2", packages="magic-impute")
#.rs.restartR() # restart R manually if outside RStudio
```

# User supplied inputs

After installing the needed R packages as well, you can being working with the `correlation_workflow.Rmd` to generate a HTML report. The following pieces of information need to be provided in the section titled "User Inputs":

- Path to the seurat object: `path_seurat`
- Directory where intermediate and final results will be stored: `path_outs`
- Name of the metadata column where celltypes are stored: `col_celltype`
- Celltype of interested (that will be subset to): `ct`
- Boolean (TRUE/FALSE) filter value on whether or not to filter genes based on expression and frequency (if TRUE, will remove genes based upon various thresholds): `filter`
- Minimum average expression for a gene (Default 0.2): `min_exp`
- Minimum number of cells a gene must be expressed in (Default 40): `min_cells`
- Minimum percent of cells a gene must be expressed in (Default 0.2): `min_perc`
- List of genes the calculate correlations between: `corr_genes_all`

# Outputs

When the `correlation_workflow.Rmd` is `knit`, a HTML file of the same name will be generated. This report will have the following information:

- Basic information about the seurat object provided
- Summary of gene expression for all genes of interest
- Heatmaps showing the correlation estimates for every gene pair
- Compare the correlation scores and the significance between SAVER, CS-CORE, and MAGIC

Additionally 3 other files will be generated in the folder where `path_outs` was specified:

- `imputed_{ct}.RDS`: Seurat object with assays including the  counts for:
    - log-normalization (RNA)
    - SCT
    - MAGIC
    - SAVER
- `corr_{ct}.csv`: Table of all correlation scores for each method
- `p_val_{ct}.csv`: Table of all p-values from the correlation calculation for each method
