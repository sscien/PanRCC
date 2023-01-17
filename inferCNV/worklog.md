## 0 Process seurat object to generate annotation and raw matrix files


## 1 transfer annotation and raw matrix file from katmai to compute1
scp -r /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/ y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/

scp -r /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/inputs/annotation_files/* y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotations_file/


export LSF_DOCKER_VOLUMES="/storage1/fs1/dinglab/Active:/storage1/fs1/dinglab/Active /scratch1/fs1/dinglab:/scratch1/fs1/dinglab"

## 2 run inferCNV on compute1

### Option1 submit multiple bsub jobs for each sample
bash /storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1.trinityctat.sh -T /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotation_files/reference_cells.txt -D /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/

### Option2  batch jobs with log files written

export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u y.song@wustl.edu -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q general -J 'inferCNV_array[1-65]' -g /compute-dinglab/EJ -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"

## 3 Transfer output back to katmai

#On katmai,
scp -r y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/outputs /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/

## 4 Post-processing

### generate input_table.txt
head -2 /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/post_processing/20230106/inputs/infercnv_matrices_input_table.txt

CPT0001540013	/diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/outputs/20230114/CPT0001540013/infercnv.20_HMM_predHMMi6.leiden.hmm_mode-subclusters.Pnorm_0.5.repr_intensities.observations.txt	RCC	/diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/outputs/20230114/CPT0001540013/infercnv.20_HMM_predHMMi6.leiden.hmm_mode-subclusters.Pnorm_0.5.repr_intensities.references.txt
CPT0001220012	/diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/outputs/20230114/CPT0001220012/infercnv.20_HMM_predHMMi6.leiden.hmm_mode-subclusters.Pnorm_0.5.repr_intensities.observations.txt	RCC	/diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/outputs/20230114/CPT0001220012/infercnv.20_HMM_predHMMi6.leiden.hmm_mode-subclusters.Pnorm_0.5.repr_intensities.references.txt

### generate gene level cnv output for each sample
bash /diskmnt/Projects/Users/austins2/tools/inferCNV/convert-cnv-predictions-to-matrix-v3.sh -t /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/post_processing/20230106/inputs/infercnv_matrices_input_table.txt -o /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/post_processing/20230106/outputs/

### plot CNV for each sample and add CNV to rds file

#### need to provide gene of interest as input
bash /diskmnt/Projects/Users/ysong/program/inferCNV_katmai/plotting-cnvs-per-cluster-v3.sh -t /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/post_processing/20230106/inputs/infercnv_plotting_input_table.txt -m /diskmnt/Projects/Pan_RCC/data/2.Seurat_obj/2023_01/metadata/rcc_65_samples_metadata.txt -g VHL,PBRM1,SETD2,BAP1,MTOR,KDM5C,TP53,PTEN,PIK3CA,TSC1,TET2,RID1A,SMAD4,CDKN2A,TP53,ARID1A,PTEN,MYC,KRAS,ERBB2,AKT2,GATA6 -o /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/post_processing/20230106/figures/individual/

## Add main CNV events to the meta data
library(tidyverse)
library(Seurat)
input <-"/diskmnt/Projects/ccRCC_scratch/RCC_snRNA_2022/Results/output/RCC_RNA_merging_cluster_v1.5/may2022_65_samples/RCC_may2022_65_samples_merged_RNA_and_SCT_1_resolution.rds"
outPath <-"/diskmnt/Projects/Pan_RCC/data/2.Seurat_obj/2023_01/metadata/"
obj=readRDS(input)
#meta <-rcc@meta.data
#write.table(meta,paste0(outPath,"rcc_65_samples_metadata.txt"),sep="\t",quote=FALSE,row.names=FALSE,col.names=TRUE)

meta_data_table <- "/diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/post_processing/20230106/figures/individual/metadata_with_cnvs.tsv"
meta <- read.table(meta_data_table, sep = '\t', header = TRUE)
cols_4_loops <- c("VHL_cnvs", "PBRM1_cnvs", "SETD2_cnvs", "BAP1_cnvs", "MTOR_cnvs", "KDM5C_cnvs", "TP53_cnvs", "PTEN_cnvs", "PIK3CA_cnvs", "TSC1_cnvs", "TET2_cnvs","ARID1A_cnvs","SMAD4_cnvs", "ARID1A_cnvs", "PTEN_cnvs", "KRAS_cnvs", "AKT2_cnvs", "GATA6_cnvs", 'ERBB2_cnvs', 'MYC_cnvs', 'CDKN2A_cnvs')
for (col_name in colnames(meta)) {
    if (col_name %in% cols_4_loops) {
        small_data <- data.frame(new_col = meta[, col_name], row.names = rownames(meta))
        obj <- AddMetaData(object = obj, metadata = small_data, col.name = paste(col_name))
    }
}
## Plot CNV on UMAP
Idents(obj) <- "seurat_clusters"
pdf('panRCC_merged_Featureplot_v2_cnv_total.pdf', useDingbats = F, height=8, width = 15)
FeaturePlot(obj, reduction = "umap.30PC", features = 'cnv_total', raster = FALSE, order = TRUE)
FeaturePlot(obj, reduction = "umap.30PC", features = "KRAS_cnvs", raster = FALSE, order = TRUE)
VlnPlot(obj, features = 'cnv_total', group.by ="cell_type_merge")
dev.off()
saveRDS(obj, "/path/to/the/merged/seurat_object_v2.rds")

