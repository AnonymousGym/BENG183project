# BENG183project
# scRNA-seq
## Purpose
Imagine we are interested in genes that cause lung cancer. We collect cells from our lung tissue, but there are different cells in this  tissue, and gene expressions are different in different cells. How do we know which gene in which kinds of cells is the key abnormality that causes cancer?
![lung tissue](https://github.com/GYDTTDYX/BENG183project/blob/main/%E6%88%AA%E5%B1%8F2022-11-28%2009.37.25.png "cells in lung tissue")
To deal with these problems, techniques that can separately analyze different cells in the same tissue is needed. scRNA-seq is the one that tackle this problem. scRNA-seq can sequence different cells, generate a dataset in which each sample is a cell in the tissue and the features are the gene expression of the cell. And most importantly, this technique allow us to analyze more than 15000 cells in a short period of time, which significantly save researcher's time. 
Due to the efficiency and effectiveness of the technique, it is useful to learn more about scRNA-seq technology.

## Upstream 
### Sequencing Process
Here we can see the process of sequencing: 
Encoded beads will flow through the microfluidics and meet with the cell. The combination of cell and bead or a mere bead will form a droplet when they enters the microfluidic that filled by oil, this step ensures each cell will only react with one bead in next step. Then we will use PCR to amplify reads and sequence all reads using normal RNA seq rechniques.
![10X seq](https://github.com/GYDTTDYX/BENG183project/blob/main/%E6%88%AA%E5%B1%8F2022-11-28%2009.35.23.png "10x seq procedure")
### The Beads and Barcodes
The beads and barcodes is critical for researchers to know the origin of different RNA reads. The 10x barcodes will indicate the origin of read, and the UMI code can reduce the bias from amplification steps by indicating those reads are multiple copies of same gene or actually different genes. 
![barcodes](https://github.com/GYDTTDYX/BENG183project/blob/main/%E6%88%AA%E5%B1%8F2022-11-28%2009.36.03.png "barcodes")

## Downstream
### Quality Check

### Seurat
After the quality check, we first preprocess the data, then perform differential expression analysis and dimension reduction. 

graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
   
For preprocessing, The input count matrix has large variations between rows and columns so we use log normalization. Then, we use Find Variable Features to find the 2000 best genes for next steps. The better genes have larger variability in the count matrix, so we have better PCA results. By scaling data, we perform feature-level scaling. Each feature will have a mean of 0 and scaled by its standard deviation.  
(NEXT PAGE)
Then, we perform dimension reduction, which is PCA. With PCA we find independent and separated features and prepare for clustering. Seurat clustering uses FindNeighbors and Find Clusters methods with an SNN algorithm and Louvain method. The clusters are automatically generated.
To visualize dimension reduction and clusters, we have tSNE or UMAP. They both reduce higher dimensional data to two dimensions and then we plot clusters with the data.
(NEXT PAGE)
Another section is differential expression. To find DE genes, we have a convenient method: Find all markers. It calculates the DE genes in each cluster and creates a sheet like the DESeq2 result.
(NEXT PAGE)
After these, we can use various plots to visualize the analysis.
We use dimplot to draw the clusters we generated and FeaturePlot to use markers to identify each cluster.
We also have like violin plots to draw the changes of expression with time. RidgePlot is an alternative.
(Next Page)
In conclusion, single-cell sequencing provides differentially expressed genes and classify functions with different clusters of cells.
