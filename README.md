# scTPA-Tutorial
A guide on how to set up scTPA for Windows.

### Introduction
This tutorial is targeted at users who would like to run the scTPA program locally (http://sctpa.bio-data.cn/sctpa/) on a PC. The local application should be used for files over 150mb-- **otherwise, use the web interface!**
As someone who has coding experience but is unfamiliar with using Powershell/Windows, I ran into many issues trying to set this up. Below, I will provide a guide to set up the program, as well as some issues I ran into and how I fixed them. I hope this helps!

### Getting Started with Powershell
The first step to setting up scTPA is to familiarize yourself with Microsoft's Powershell application. I was mislead into thinking I had to download Ubuntu for Microsoft in order to get this program to run. Maybe you can run it this way, but I personally ran into a lot of permission issues using a 3rd-party application. Powershell is Microsoft's equivalent to Ubuntu on Linux or Mac's terminal application. It will allow you to open and run the R script that contains the scTPA application.
This is what it looks like when you open Powershell:

![Poweshell Image](https://github.com/oellis1/scTPA-Tutorial/blob/main/powershell1.png)

One thing to note is that PS H:/>-- **this shows your directory**. A directory is the space in which Powershell will be looking inside your computer. Some simple commands:
1. cd (Change directory)

    If I'm in my home directory, H:/, and I want to move to my "scTPA_local-v7" folder, I could use the command 'cd [path]'. To get the path of the folder, hold shift and right click, then select "Copy as path." To paste into Powershell, right-click. Note that when you paste the pathname into Powershell, it will include quotes. Get rid of these, then hit enter to run the command. You should see the new directory listed after PS. 
    If the directory (folder) you want to switch to shows up when you execute 'ls', then you do not need to list the full pathname. Although my full path would be H:/scTPA_local-v7, I could also do 'cd scTPA_local-v7'. Unless you specify a full path, the computer will look only in your current directory, and since this folder is in my current directory (H:/), it will find the folder.
2. ls (list)

    This command will show all of the items in the directory. For example, here is the result of running ls inside my H:/ directory. It should be identical to the list you see when you open your folder in file explorer.
    
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
Now to download the scTPA application. Search for Powershell in the start button. Right click on it, and select "Run as Administrator." If asked, allow Powershell to make changes to your computer. Whatever directory you are currently in is the one that you will be downloading scTPA into, so make sure it is your preferred directory. Then, execute the following code, line by line:
  ```R
  wget http://sctpa.bio-data.cn:8888/sctpa/resources/scTPA_local-v7.zip
  unzip scTPA_local-v7.zip
  cd scTPA_local-v7
  ```
It may take a while to download. Once you are done, your directory should be "PS .....scTPA_local-v7>". 

Now, you will need to download dependent python packages. First, make sure you have python installed. You can do this by typing python into the search/start button. If it doesn't show up, you can download python here: https://www.python.org/downloads/release/python-396/
Once python is installed, you will also need to make sure you have "pip" installed. This is a python package installer that runs through Powershell. To check it if is installed, type 'pip' into Powershell and hit enter. If you get the error "'pip' is not recognized as the name of a cmdlet, function, script file, or operable program...", then it is not installed. Follow the instructions here to install it: https://www.geeksforgeeks.org/how-to-install-pip-on-windows/#:~:text=pip%20can%20be%20downloaded%20and%20installed%20using%20command-line,Voila%21%20pip%20is%20now%20installed%20on%20your%20system.
  
If you have installed everything and the 'python' command still does not bring up the >>> (as below), it means you need to add some variables to your path. Search for "Edit the System Environment Variables," and run it as an administator. Allow it to make changes, then click "Environment Variables."
  
Under path, you will need to add the location of python.exe. Search for python.exe in your file explorer, and then copy it as a pathname. Mine is C:/Python39/python.exe-- yours may be different. Delete /python.exe so you have only C:/Python39. Add this to the list of path variables. Restart your computer and to run the python command-- it should work now. Repeat this process for Rscript.exe, and add its path to the Path variable as well. 

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
  




