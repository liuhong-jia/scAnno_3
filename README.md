# scAnno

***

A deconvolution strategy-based automatic cell type annotation tool for single-cell RNA sequencing datasets.
![image](https://user-images.githubusercontent.com/115637576/199387392-3d66fb26-e3d3-43c9-9378-04f541600e3f.png)


# Installing the package

***

You can install the package using devtools::install_github:

    devtools::install_github("liuhong-jia/scAnno")

# Getting started with scAnno

***

In this tutorial we will use GSE136103 (Liver) as an example.

    library(scAnno)
    
    library(Seurat)
    
    library(patchwork)
    
    library(dplyr)
    
    library(MASS)
    
    library(irlba)
    
    library(future)
    
    library(glmnet)
    
    library(reticulate)
    
    library(parallel)
    
    data(Human_cell_landscape)
    
    data(gene.anno)
    
    [INFO] Protein-coding genes.
    
    data(tcga.data.u)
    
    [INFO] bulk RNA-seq data of pan-cancer in TCGA.
    
    data(GSE136103)
    
    obj.seu <- GSE136103
    
    [INFO] 35 single-cell clusters that need to be annotated.
    
    ref.obj <- Human_cell_landscape
    
    [INFO] Reference spectrum of human cell landscape, including 63 cell types across more than 50 tissue types.
    
    ref.expr <- GetAssayData(ref.obj, slot = 'data') %>% as.data.frame
    
    ref.anno <- Idents(ref.obj) %>% as.character

# single cell annotation

***
	scAnno(query = obj.seu,
	ref.expr = ref.expr,
	ref.anno = ref.anno ,
	save.markers = "ref.markers",
	cluster.col = "seurat_clusters",
	factor.size = 0.1,
	pvalue.cut  = 0.01,
	method ="co.exp",
	seed.num = 10, 
	redo.markers = FALSE,
	gene.anno = gene.anno,
	permut.num = 100,
	show.plot = TRUE,
	verbose = TRUE,
	tcga.data.u = tcga.data.u
	)
	
	[INFO] Checking the legality of parameters
	[INFO] 30 cell types in reference, 35 clusters in query objects
	[INFO] Deconvolution by using RLM method
	[INFO] Logistic regression for cell-type predictions, waiting...
	[INFO] Merging the scores of both models, and assign annotations to clusters
	[INFO] Estimating p-values for annotations...
	[INFO] Finish![INFO] Checking the legality of parameters
	[INFO] 30 cell types in reference, 35 clusters in query objects
	[INFO] Deconvolution by using RLM method
	[INFO] Logistic regression for cell-type predictions, waiting...
	[INFO] Merging the scores of both models, and assign annotations to clusters
	[INFO] Estimating p-values for annotations...
	[INFO] Finish!
	
	$query
	An object of class Seurat
	21898 features across 16036 samples within 1 assay
	Active assay: RNA (21898 features, 2830 variable features)
	2 dimensional reductions calculated: pca, umap
	
	$reference
	An object of class Seurat
	17020 features across 5561 samples within 1 assay
	Active assay: RNA (17020 features, 0 variable features)

    $pred.label
                   C0                    C1                    C2 
             "T cell"              "T cell"              "T cell" 
                   C3                    C4                    C5 
             "T cell"              "T cell"      "Dendritic cell" 
                   C6                    C7                    C8 
             "T cell"              "T cell"              "T cell" 
                   C9                   C10                   C11 
           "Monocyte"     "Epithelial cell"          "Macrophage" 
                  C12                   C13                   C14 
             "T cell"    "Endothelial cell"            "Monocyte" 
                  C15                   C16                   C17 
	"Endothelial cell"    "Endothelial cell"              "T cell" 
                  C18                   C19                   C20 
         "Macrophage"  "Smooth muscle cell"              "T cell" 
                  C21                   C22                   C23 
	"Smooth muscle cell"              "B cell"            "Monocyte" 
                  C24                   C25                   C26 
             "T cell"              "T cell" "B cell (Plasmocyte)" 
                  C27                   C28                   C29 
     "Dendritic cell"    "Endothelial cell"    "Endothelial cell" 
                  C30                   C31                   C32 
             "B cell"        "Stromal cell"    "Endothelial cell" 
                  C33                   C34 
     "Dendritic cell"     "Epithelial cell"

    $pred.score
	[1] 0.9999973 0.9999552 0.9997857 1.0000000 1.0000000 0.9964331 1.0000000
 	[8] 0.9986025 0.9989499 0.9998946 0.7059541 0.9920111 0.9999960 0.9984048
	[15] 0.9824844 0.9903304 0.9989935 1.0000000 0.9903538 0.9998917 1.0000000
	[22] 0.9998866 0.9989194 0.8723226 1.0000000 0.9999767 0.9994844 1.0000000
	[29] 0.9983985 0.9996705 0.9532563 0.5015979 0.9670198 1.0000000 0.7521303
    
    $pvals
     	C0      C1      C2      C3      C4      C5      C6      C7      C8 
	2.7e-17 2.9e-17 3.6e-17 2.7e-17 2.7e-17 0.0e+00 2.7e-17 1.6e-16 1.0e-16 
     	C9     C10     C11     C12     C13     C14     C15     C16     C17 
	0.0e+00 0.0e+00 0.0e+00 2.7e-17 0.0e+00 0.0e+00 0.0e+00 0.0e+00 2.7e-17 
    	C18     C19     C20     C21     C22     C23     C24     C25     C26 
	0.0e+00 0.0e+00 2.7e-17 0.0e+00 0.0e+00 0.0e+00 2.7e-17 2.8e-17 0.0e+00 
    	C27     C28     C29     C30     C31     C32     C33     C34 
	0.0e+00 0.0e+00 0.0e+00 0.0e+00 0.0e+00 0.0e+00 0.0e+00 0.0e+00

    [INFO] Show annotation results...
![c75c451c3a993d5f5a78adae32947c4](https://user-images.githubusercontent.com/115637576/218242912-44df6b81-7501-4840-aa1d-d97bb7121aea.png)


