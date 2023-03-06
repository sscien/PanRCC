##
## On katmai
conda activate methyl-pipeline
conda env export > /diskmnt/Projects/Users/ysong/program/Docker/methyl-pipeline_v1.yml

scp -r ysong@katmai.wusm.wustl.edu://diskmnt/Projects/Users/ysong/program/Docker/methyl-pipeline_v1.yml /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker/methyl-pipeline/

For example, this command will submit a job to build a container based on files in the ‘my_container’ subdirectory, tag it with latest, and push it to the container repo at repo.example.com/repo_username/my_container:

cd /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker
bsub -G compute-dinglab -q general-interactive -Is -a 'docker_build(docker.io/songyizhe/my_container:latest)'  my_container

## To build the seurat docker, run

cd /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker/methyl-pipeline/
cp /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker/Seurat/build_seurat_docker_v0.8.sh .
cp /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker/Seurat/Dockerfile .

```
cd /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker
bash /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker/methyl-pipeline/build_docker.sh
```
bash /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Docker/Seurat/build_seurat_docker_v0.8.sh

## copy scripts and files to compute1
```
scp -r /diskmnt/Projects/Users/ysong/Projects/cptac_methylation/ y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/pipelines/methylation/

```
## Pull docker to do the job

```
export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -Is -n 16 -q 'general-interactive'  -g /compute-dinglab/EJ -G compute-dinglab -M 50G -R 'select[mem>50G] rusage[mem=50G]' -a 'docker(songyizhe/methyl-pipeline:v1.0)' /bin/bash -l
```

# Activate environment
```
conda activate methyl-pipeline
```

## Make input

## create symbolic link for IDAT files

```
python /storage1/fs1/dinglab/Active/Projects/PanRCC/Data/Analysis/KIRC_methylation/make_methlation_input.py /storage1/fs1/dinglab/Active/Projects/PanRCC/Data/Analysis/KIRC_methylation/2023_03_KIRC 2023_03_KIRC /storage1/fs1/dinglab/Active/Projects/PanRCC/Data/Catalog/CTSP_KIRC.BamMap-wide.txt
```

bash /diskmnt/Projects/Users/ysong/Projects/cptac_methylation/methylation_v1.2.bash /diskmnt/Projects/Users/ysong/project/methylation/batch_1123/ batch_1123 /diskmnt/Projects/Users/ysong/project/cptac_analysis/batch_22_11_23/Methylation_Array.run_list.tsv

```
bash /storage1/fs1/dinglab/Active/Projects/PanRCC/Data/Analysis/KIRC_methylation/methylation_v1.2_compute1.sh /storage1/fs1/dinglab/Active/Projects/PanRCC/Data/Analysis/KIRC_methylation/2023_03_KIRC 2023_03_KIRC /storage1/fs1/dinglab/Active/Projects/PanRCC/Data/Catalog/CTSP_KIRC.BamMap-wide.txt
```
## after the job was finished

cp -r /diskmnt/Projects/Users/ysong/project/methylation/batch_1123/ /diskmnt/Projects/cptac_scratch/CPTAC3_analysis/Methylation_hg38

## result folder

/diskmnt/Projects/cptac_scratch/CPTAC3_analysis/Methylation_hg38/batch_1123/

# Read in arguments
Path=$1
Batch_name=$2
Sample=$3
cd $Path

## Make directory

cd /diskmnt/Projects/Users/ysong/project/methylation/
mkdir batch_1123
cd batch_1123

## Make input
python $cptac_methyl/make_pipeline_input.py ${Path} ${Batch_name} ${Sample}

#python $cptac_methyl/make_pipleline_input_v1.3.py /diskmnt/Projects/Users/ysong/project/methylation/batch0318_22 batch0318_22 /diskmnt/Projects/Users/ysong/project/methylation/batch0318_22/Methylation_Array.analysis_SN.dat

python /diskmnt/Projects/Users/ysong/Projects/cptac_methylation/make_pipeline_input_v1.3.py /diskmnt/Projects/Users/ysong/project/methylation/batch_1123/ batch_1123 /diskmnt/Projects/Users/ysong/project/cptac_analysis/batch_22_11_23/Methylation_Array.run_list.tsv

#### Analysis description
cp /diskmnt/Projects/Users/wliang/CPTAC_Pancan_Methylation/09_Pipeline/20211019_batch_1021/Processed_hg38_remap/cptac_methylation_analysis.txt /diskmnt/Projects/Users/ysong/project/methylation/batch0318_22/Processed/Processed_hg38_remap

cp -r /diskmnt/Projects/Users/wliang/CPTAC_Pancan_Methylation/00_Scripts /diskmnt/Projects/Users/ysong/project/jupyter_lab/Methylation

## Softlink IDAT files
## Softlink IDAT files
# for i in `cat /diskmnt/Projects/Users/ysong/Projects/Datasets/CPTAC3.catalog/katmai.BamMap.dat ${Sample}| grep Methylation | awk -F "\t" '{OFS="\t"; print $6}'`; do ln -s $i ${Path}; done
## Pipeline
Rscript $cptac_methyl/cptac_methylation_v1.1.R ${Path} |& tee tmux.methylation.log

Rscript $cptac_methyl/cptac_methylation_liftover.R ${Path}/Processed |& tee tmux.mapping.log


Hi @Matt Check out the methylation result here, README will be provided later. Please let me know if you have any concerns.
hg19 /diskmnt/Projects/cptac_scratch/CPTAC3_analysis/Methylation_hg38/batchFeb22/Processed/
hg38 /diskmnt/Projects/cptac_scratch/CPTAC3_analysis/Methylation_hg38/batchFeb22/Processed/Processed_hg38_remap/
