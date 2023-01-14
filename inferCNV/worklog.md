scp -r /diskmnt/Projects/Pan_RCC/Analysis/inferCNV/ y.song@compute1-client-1.ris.wustl.edu://storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/


export LSF_DOCKER_VOLUMES="/storage1/fs1/dinglab/Active:/storage1/fs1/dinglab/Active /scratch1/fs1/dinglab:/scratch1/fs1/dinglab"
bash /storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1.trinityctat.sh -T /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotation_files/reference_cells.txt -D /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/


## batch jobs

export LSF_DOCKER_VOLUMES="$HOME:$HOME $STORAGE1_DINGLAB:$STORAGE1_DINGLAB $STORAGE1_MATT:$STORAGE1_MATT $SCRATCH1_DINGLAB:$SCRATCH1_DINGLAB"
bsub -n 12 -q 'general dinglab' -J 'inferCNV_array[1-65]' -g /compute-dinglab/EJ -G compute-dinglab -M 240G -R 'select[mem>240G] rusage[mem=240G]' -a 'docker(trinityctat/infercnv:1.11.2)' /bin/bash -c "/storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/run_inferCNV_compute1_Array.sh /storage1/fs1/dinglab/Active/Projects/ysong/Projects/PanRCC/inferCNV/v1_2023_01/inputs/annotation_files/work.list"


bsub -G compute-dinglab -q general -J ${tumor} -N -n 12 -R 'select[mem>240GB] rusage[mem=240GB]' -M 240GB -oo ${path_log_file} -a 'docker(trinityctat/infercnv:1.11.2)' "ulimit -s unlimited && Rscript /storage1/fs1/dinglab/Active/Projects/ysong/pipelines/inferCNV/inferCNV.trinityctat.R \
