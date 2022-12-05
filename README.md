# BENG183project
# scRNA-seq
## Purpose

## Upstream 


## Downstream
### Quality Check

### Seurat
After the quality check, we first preprocess the data, then perform differential expression analysis and dimension reduction. 

#### Preprocessing
```
Dev_NPC <- NormalizeData(object = Dev_NPC, normalization.method = "LogNormalize", scale.factor = 10000)
Dev_NPC <- FindVariableFeatures(Dev_NPC, selection.method = "vst", nfeatures = 2000)
Dev_NPC <- ScaleData(Dev_NPC, vars.to.regress = c("nCount_RNA","percent.mt"), features = row.names(Dev_NPC))
```
This is the preprocessing R code. ```Dev_NPC``` is a Seurat object.

```
Dev_NPC <- NormalizeData(object = Dev_NPC, normalization.method = "LogNormalize", scale.factor = 10000)
```
For preprocessing, The input count matrix has large variations between rows and columns. To level the inputs, we use log normalization (the default normalizing method).
```
Dev_NPC <- FindVariableFeatures(Dev_NPC, selection.method = "vst", nfeatures = 2000)
```
Then, we use ```FindVariableFeatures()``` to find the 2000 best genes for next steps. We chose the default selection method ```vst```, so we decide the better genes will have larger variability in the count matrix, and thus we will have better PCA results. This process is calculated by standardizing the feature values using the observed mean and expected variance.
```
Dev_NPC <- ScaleData(Dev_NPC, vars.to.regress = c("nCount_RNA","percent.mt"), features = row.names(Dev_NPC))
```
By scaling data, we perform feature-level scaling. Each feature will have a mean of 0 and scaled by its standard deviation, which means it is regressed. Then, we scale and center the residues.

#### Dimension Reduction
_Why do we perform Dimension Reduction?

The count matrix is a multidimensional data set, which is very hard to explore the inner patterns (especially for human eyes!!). If you remember the beautiful figures in papers like this:

![intro_dr](SC_umap.png "A UMAP Plot of single-cell Clusters")

This is a example result of dimension reduction. By using Seurat, we can visualize the count matrix using two most significant dimensions, and achieve clusters automatically. After this, we can find marker genes for each cluster and determine what they represent. As in UMAP plots, the general structure and distances between clusters also contains information, we are able to generate rich analysis results using Dimension Reduction.

```
Dev_NPC <- RunPCA(Dev_NPC, features = VariableFeatures(object = Dev_NPC), verbose = FALSE, npcs = 100) 
```

The key step of dimension reduction is PCA -- Principal Component Analysis. With PCA we find independent and separated features and prepare for clustering. Seurat clustering uses FindNeighbors and Find Clusters methods with an SNN algorithm and Louvain method. The clusters are automatically generated.
To visualize dimension reduction and clusters, we have tSNE or UMAP. They both reduce higher dimensional data to two dimensions and then we plot clusters with the data.
(NEXT PAGE)
Another section is differential expression. To find DE genes, we have a convenient method: Find all markers. It calculates the DE genes in each cluster and creates a sheet like the DESeq2 result.
(NEXT PAGE)
After these, we can use various plots to visualize the analysis.
We use dimplot to draw the clusters we generated and FeaturePlot to use markers to identify each cluster.
We also have like violin plots to draw the changes of expression with time. RidgePlot is an alternative.
(Next Page)

The Seurat toolkit updates fast, so some methods with similar functions (i.e. ```FindMarkers``` and ```FindAllMarkers```) have been used in different time periods. Make sure to consult the current [Seurat reference page](https://satijalab.org/seurat/reference/index.html) for advice!

In conclusion, single-cell sequencing provides differentially expressed genes and classify functions with different clusters of cells.
