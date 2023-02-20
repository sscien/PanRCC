## 3 Transfer output back to katmai

#On katmai,
```
scp -r y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/cellranger_processed/v7.1.0/outputs/HT293N1-S1H3A3N1Z1_1Bmn1_1 /diskmnt/Projects/ccRCC_scratch/RCC_snRNA_2022/cellranger_arc_2.0.0/all_batches_cellrangers/
```


```
scp -r y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/cellranger_processed/v7.1.0/outputs/HT282N1-S1H3A3N1Z1_1Bmn1_1 /diskmnt/Projects/ccRCC_scratch/RCC_snRNA_2022/cellranger_arc_2.0.0/all_batches_cellrangers/
```



## 4 Merge

```
library(Seurat)
library(dplyr)
library(Matrix)
library(RColorBrewer)
library(tidyverse)
#library(ReactomePA)
#library(clusterProfiler)
library(ggplot2)
library(cowplot)
library(getopt)
library(optparse)
library(Matrix)
library(Seurat)
library(dplyr)
library(RColorBrewer)
library(ggplot2)
#library(devtools)
library(DropletUtils)
library(stringr)
#library(DoubletFinder)
library(gridExtra)
library(SoupX)
library(tibble)
#library (ggstatsplot)
library(cowplot)
options(future.globals.maxSize= 120128960000)

setwd("/diskmnt/Projects/Pan_RCC/data/2.Seurat_obj/2023_02/Integration/")
out_path <- "/diskmnt/Projects/Pan_RCC/data/2.Seurat_obj/2023_02/Integration/"

# Set parameters for filtering cells
mt <- 20
nCount <- 1000
nFeature <- 200

# Define sample IDs
sample_ids <- c("HT293N1-S1H3A3N1Z1_1Bmn1_1", "HT282N1-S1H3A3N1Z1_1Bmn1_1")

# Create empty list to store filtered objects
subset_list <- list()

# Loop through each sample ID
for (sample_id in sample_ids){
  # Load data and create Seurat object
  file_path <- paste0("/diskmnt/Projects/ccRCC_scratch/RCC_snRNA_2022/cellranger_arc_2.0.0/all_batches_cellrangers/", sample_id,"/outs/raw_feature_bc_matrix")
  data <- Read10X(data.dir = file_path)
  tmp <- CreateSeuratObject(counts = data, min.cells = 0)
  DefaultAssay(tmp) <- "RNA"
  
  # Calculate mitochondrial content
  tmp[["percent.mt"]] <- PercentageFeatureSet(tmp, pattern = "^MT-")
  
  # Subset cells based on mitochondrial content, nCount, and nFeature
  tmp <- subset(x = tmp, subset = percent.mt < mt & nCount_RNA > nCount & nFeature_RNA > nFeature)
  
  # Rename cells with sample ID and add sample ID column to meta.data
  tmp <- RenameCells(object = tmp, new.names = paste0(colnames(tmp), "_", sample_id))
  tmp@meta.data$sample_id <- sample_id
  
  # Append filtered object to list
  subset_list[[sample_id]] <- tmp
}
##########################################################################################
### 2. merge combined_ref_Spike_in_Obj
##########################################################################################
# Initialize merged object with the first sample
merged_obj <- subset_list[[1]]

# Remove first sample from the list
subset_list <- subset_list[[-1]]

merged_obj <- merge(merged_obj, subset_list[[1]])

# Loop over remaining samples and merge with the merged object
for (i in seq_along(subset_list)) {
  print(paste("Combining sample", i+1, subset_list[[i]]@meta.data$sample_id[1], sep = " "))
  merged_obj <- merge(merged_obj, subset_list[[i]])
  print(paste("Combined size at step:", i+1, format(object.size(merged_obj), units = "auto")))
  print(paste("Successfully merged object from sample 1 to sample", i+1, subset_list[[i]]@meta.data$sample_id[1], sep = " "))
}
rm(subset_list)
# Save the merged Seurat object
saveRDS(merged_obj, paste0(out_path, "/HTAN_ccRCC_2_samples_mt_", mt, "_nCount_", nCount, "_nFeature_", nFeature, ".rds"))
gc()
##########################################################################################
### 3. merge batch1_3 and batch4 object
##########################################################################################
rcc <-readRDS("/diskmnt/Projects/ccRCC_scratch/RCC_snRNA_2022/Results/output/RCC_RNA_merging_cluster_v1.5/may2022_65_samples/RCC_may2022_65_samples_merged_RNA_and_SCT_1_resolution.rds")
merged_obj <- merge(merged_obj, rcc)
saveRDS(merged_obj, paste0(out_path, "/PanRCC_67_samples_merged_obj_mt_", mt, "_nCount_", nCount, "_nFeature_", nFeature, ".rds"))

```
