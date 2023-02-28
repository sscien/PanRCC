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
