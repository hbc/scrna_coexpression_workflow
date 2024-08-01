In order to run the correlation workflow, `reticulate` needs to be used as MAGIC is a python based tool. In order to recreate a conda environments that is compatible with running the workflow, you can create a new conda environment with `requirements.txt` file included in this repository. The following code can be used to create said  environment:

```bash
conda create -n scrna_corr R=4.3.2
conda install --file requirements.txt
```

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