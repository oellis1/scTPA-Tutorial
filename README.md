# scTPA-Tutorial
A guide on how to set up scTPA for Windows.

### Introduction
This tutorial is targeted at users who would like to run the scTPA program locally (http://sctpa.bio-data.cn/sctpa/) on a PC. The local application should be used for files over 150mb-- **otherwise, use the web interface!**
As someone who has coding experience but is unfamiliar with using Powershell/Windows, I ran into many issues trying to set this up. Below, I will provide a guide to set up the program, as well as some issues I ran into and how I fixed them. I hope this helps!

### Getting Started with Powershell
The first step to setting up scTPA is to familiarize yourself with Microsoft's Powershell application. I was mislead into thinking I had to download Ubuntu for Microsoft in order to get this program to run. Maybe you can run it this way, but I personally ran into a lot of permission issues using a 3rd-party application. Powershell is Microsoft's equivalent to Ubuntu on Linux or Mac's terminal application. It will allow you to open and run the R script that contains the scTPA application.
This is what it looks like when you open Powershell:

![Powershell Image](https://github.com/oellis1/scTPA-Tutorial/blob/main/powershell1.png)

One thing to note is that PS H:/>-- **this shows your directory**. A directory is the space in which Powershell will be looking inside your computer. Some simple commands:
1. cd (Change directory)

    If I'm in my home directory, H:/, and I want to move to my "scTPA_local-v7" folder, I could use the command 'cd [path]'. To get the path of the folder, hold shift and right click, then select "Copy as path." To paste into Powershell, right-click. Note that when you paste the pathname into Powershell, it will include quotes. Get rid of these, then hit enter to run the command. You should see the new directory listed after PS. 
 ![1](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%205.40.56%20PM.png)
 ![1.1](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%206.12.58%20PM.png)
 
    If the directory (folder) you want to switch to shows up when you execute 'ls', then you do not need to list the full pathname. Although my full path would be H:/scTPA_local-v7, I could also do 'cd scTPA_local-v7'. Unless you specify a full path, the computer will look only in your current directory, and since this folder is in my current directory (H:/), it will find the folder.
2. ls (list)

    This command will show all of the items in the directory. For example, here is the result of running ls inside my H:/ directory. It should be identical to the list you see when you open your folder in file explorer.
    ![2] (https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%205.46.38%20PM.png)
    ![3] (https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%205.46.44%20PM.png)
    
    
    
### Installing R Dependencies
Dependencies are packages, libraries, etc. which our program will need to have in order to run. The guide to installing scTPA gives you a script to install everything, but this won't actually work for all of the packages because some of the syntax is wrong. 
First, install all of the R-packages. Open RStudio and run the following code.
```R
install.packages('p2data', repos='https://kharchenkolab.github.io/drat/', type='source')
install.packages("devtools")
install.packages("rJava")
library(rJava)
library(devtools)
library(usethis)
devtools::install_local("dependencies/pagoda2-v0.1.1-master.zip")

if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install("parallel")
BiocManager::install("optparse")
install_github("Vivianstats/scImpute")
BiocManager::install("scran")
BiocManager::install("scater")
BiocManager::install("dplyr")
BiocManager::install("Seurat")
BiocManager::install("cluster")
BiocManager::install("fpc")
BiocManager::install("SIMLR")
BiocManager::install("this.path")
BiocManager::install("scales")
BiocManager::install("ggplot2")
BiocManager::install("cowplot")
BiocManager::install("pheatmap")
BiocManager::install("AUCell")
BiocManager::install("Cairo")
BiocManager::install("scde")
```
Pasting this into R and running will download all of the necessary dependencies. To check that everything has installed properly, use the library function to attach each package. If you get any errors, such as "there is no package called <>," then the package did not install correctly. Google "install <package name>" and follow the instructions to install it. Running library(package name) should work.
```R
library(parallel)
library(optparse)
library(scImpute)
library(scran)
library(scater)
library(dplyr)
library(Seurat)
library(cluster)
library(fpc)
library(SIMLR)
library(this.path)
library(scales)
library(ggplot2)
library(cowplot)
library(pheatmap)
library(AUCell)
library(Cairo)
library(scde)
```
There we go!
  
### Installing Dependencies Through Powershell
Now to download the scTPA application. Search for Powershell in the start button. Right click on it, and select "Run as Administrator." If asked, allow Powershell to make changes to your computer. 
    ![5](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%206.08.54%20PM.png)
Whatever directory you are currently in is the one that you will be downloading scTPA into, so make sure it is your preferred directory. Then, execute the following code, line by line:
  ```R
  wget http://sctpa.bio-data.cn:8888/sctpa/resources/scTPA_local-v7.zip
  unzip scTPA_local-v7.zip
  cd scTPA_local-v7
  ```
It may take a while to download. Once you are done, your directory should be "PS .....scTPA_local-v7>". 

Now, you will need to download dependent python packages. First, make sure you have python installed. You can do this by typing python into the search/start button. 
![6](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%206.15.44%20PM.png)
    
If it doesn't show up, you can download python here: https://www.python.org/downloads/release/python-396/
Once python is installed, you will also need to make sure you have "pip" installed. This is a python package installer that runs through Powershell. To check it if is installed, type 'pip' into Powershell and hit enter. If you get the error "'pip' is not recognized as the name of a cmdlet, function, script file, or operable program...", then it is not installed. Follow the instructions here to install it: https://www.geeksforgeeks.org/how-to-install-pip-on-windows/#:~:text=pip%20can%20be%20downloaded%20and%20installed%20using%20command-line,Voila%21%20pip%20is%20now%20installed%20on%20your%20system.
 
If you have installed everything and the 'python' command still does not bring up the >>> (as below), it means you need to add some variables to your path. 
![7](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%206.22.30%20PM.png)
    
Search for "Edit the System Environment Variables," and run it as an administator. Allow it to make changes, then click "Environment Variables."
  
Under path, you will need to add the location of python.exe. Search for python.exe in your file explorer, and then copy it as a pathname. Mine is C:/Python39/python.exe-- yours may be different. Delete /python.exe so you have only C:/Python39. Add this to the list of path variables. Restart your computer and to run the python command-- it should work now. Repeat this process for Rscript.exe, and add its path to the Path variable as well. 
![8](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%206.23.31%20PM.png)
    
![9](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%206.23.35%20PM.png)
    
![10](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%207.06.54%20PM.png)
    

Now, you should be able to use the 'pip install' command to install all of the dependent packages. 
```python
  pip install pandas==0.25.3
pip install numpy
pip install json
pip install clustergrammer
pip install seaborn
pip install multiprocessing
  ```

### Running scTPA
To run scTPA, you will need to use the Rscript command. If you have added the path to your environment variables, you should be able to run Rscript and get the following output:

![11](https://github.com/oellis1/scTPA-Tutorial/blob/main/Screen%20Shot%202021-08-02%20at%208.01.42%20PM.png)
    
Once this works, you are ready to run scTPA. The different options are as follows:
    
```
    Options:
        -f FILE, --file=FILE
                Gene expression profile, genes X cells. The processed gene expression profile can be generated using different platforms, such as 10X genomics and Smart-seq. The values in this profile should be non-negative, and this file can be uploaded depending on data types of UMI count, read count, RPKM, FPKM, CPM or TPM. [default= NULL]

        --cellType=CELLTYPE
                Optional. Cell type file. First column is cell name (same as the colnames of gene expression profile), second column is cell type. No header names. [default= NULL]

        --normalize=NORMALIZE_METHOD
                Methods used for normalization. Available options are 'none', 'log', 'CLR', 'RC' 'sctrancform' or 'scran'. 'log', 'CLR' 'RC' and 'sctrancform': The normalization methods from Seurat R package. 'scran': The normalization strategy for scRNA-seq is implemented based on the deconvolutional size factor using the scran R package. [default= none]

        --min_cells=MIN_CELLS
                Genes must be detected within a minimum number of cells. Used for filtering genes. [default= 3]

        --min_features=MIN_FEATURES
                Cells must have at least the minimum number of genes. Used for filtering cells. [default= 200]

        --species=SPECIES
                Species. Available options are 'homo' or 'mus'. [default= homo]

        --imputation=IMPUTATION
                Imputation method. Available options are 'scImpute' or 'none'. 'scImpute': impute scRNA-seq profile using scImpute R package. [default= FALSE]

        --data_type=FILE
                Data type of gene expression profile∩╝îAvailable options are 'TPM' or 'count'. 'count' indicate that the expression profile is non-negative UMI or read count. 'TPM' indicate that the expression profile is normalized FPKM, RPKM, CPM or TPM. [default= TPM]

        --pathway_database=PATHWAY_DATABASE
                Pathway database. Avalible database are avalible on https://github.com/sulab-wmu/scTPA#details [default= kegg]

        --user_pathway=USER_PATHWAY
                Optional. User defined pathway file in gmt format. [default = NULL]

        --pas_method=PAS_METHOD
                PAS (pathway activation signatures) transformation method. Available options are 'pagoda2', 'Vision', 'AUCell', 'gsva', 'ssgsea', 'zscore' or 'plage'. [default= ssgsea]

        --para_size=PARA_SIZE
                Number of kernels used for parallel computation. [default= 3]

        --cluster_method=CLUSTER_METHOD
                Clustering method. Available options are 'seurat', 'hclust', 'simlr', 'kmedoids', 'kmeans' or 'dbscan'. [default= seurat]

        --seurat_dims=SEURAT_DIMS
                Dimensions of PCA used in Seurat FindNeighbors method. [default= 8]

        --seurat_resolution=SEURAT_RESOLUTION
                Resolution used in Seurat FindClusters method. [default= 0.5]

        --k_cluster=K_CLUSTER
                Number of clusters. Used for clustering methods except Seurat and dbscan. [default= 5]

        --min_pts=MIN_PTS
                Number of nearest neighbors used in dbscan clustering. [default= 3]

        --dims=DIMS
                Number of PCA dimensions used for TSNE or UMAP. [default= 20]

        --marker_method=FIND_MAKER_METHOD
                Method for finding siginificant markers. [default= wilcox]

        --logFC_thre=THRESHOLD_LOGFC
                logfc.threshold Limit testing to genes which show, on average, at least X-fold difference (log-scale) between the two groups of cells. This parameter was same as the 'logfc.threshold' of FindAllMarkers in Seurat R package. [default= 0.25]

        --min_pct=MIN_PCT
                Only test genes that are detected in a minimum fraction of min.pct cells in either of the two populations will be retain. This parameter was same as the 'min.pct' of FindAllMarkers in Seurat R package. [default= 0.1]

        --shown_markers=SHOWN_MARKERS
                The number of markers for each cell type used for heatmap visualization. [default= 3]

        -o OUT_DIR, --out_dir=OUT_DIR
                Output folder. [default= NULL]

        -h, --help
                Show this help message and exit
    ```
  




