scp -r /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/ y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/

scp -r /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/v1_2023_01/inputs/annotation_files/* y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotations_file/


export LSF_DOCKER_VOLUMES="/storage1/fs1/dinglab/Active:/storage1/fs1/dinglab/Active /scratch1/fs1/dinglab:/scratch1/fs1/dinglab"
bash /storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1.trinityctat.sh -T /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotation_files/reference_cells.txt -D /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/


## batch jobs with log files written

export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q 'general dinglab' -J 'inferCNV_array[1-65]' -g /compute-dinglab/EJ -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotation_files/work.list"


## 1-10
export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q dinglab -J 'inferCNV_array[1-10]' -g /compute-dinglab/EJ -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"

## 11-65 general

export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q general -J 'inferCNV_array[11-30]' -g /compute-dinglab/GBM -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"

export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q general -J 'inferCNV_array[31-65]' -g /compute-dinglab/GBM -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"


##### TEST

export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q 'general dinglab' -J 'inferCNV_array[1-2]' -g /compute-dinglab/EJ -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"


/storage1/fs1/dinglab/Active/Projects/PECGS/PECGS_pipeline/Fusion/Fusion_hg38_scripts/fusion_pipeline_ris_v1.sh 
cp /storage1/fs1/dinglab/Active/Projects/ysong/Projects/Fusion_hg38/Multiple_myeloma/MM_RNA_seq_Fusion_batch_0122/work.list /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/



####
export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q 'general dinglab' -J 'inferCNV_array[1-2]' -g /compute-dinglab/EJ -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"

#####
export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 8 -N -u ysongwustl@gmail.com -oo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.out.%J.%I.log -eo /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/logs/inferCNV.err.%J.%I.log -q dinglab -J 'inferCNV_array[1-2]' -g /compute-dinglab/GBM -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/work.list"

######
/storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotation_files/work.list

###
bsub -G compute-dinglab -q general -J ${tumor} -N -n 12 -R 'select[mem>240GB] rusage[mem=240GB]' -M 240GB -oo ${path_log_file} -a 'docker(trinityctat/infercnv:1.11.2)' "ulimit -s unlimited && Rscript /storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/inferCNV.trinityctat.R \
