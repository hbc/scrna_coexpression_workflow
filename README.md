# Creating the conda environment

In order to run the correlation workflow, `reticulate` needs to be used as MAGIC is a python based tool. In order to recreate a conda environments that is compatible with running the workflow, you can create a new conda environment with `requirements.txt` file included in this repository. The following code can be used to create said  environment:

```bash
conda create -n scrna_corr

git clone git://github.com/KrishnaswamyLab/MAGIC.git
cd MAGIC/python
python setup.py install --user
cd ../Rmagic
R CMD INSTALL .
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
