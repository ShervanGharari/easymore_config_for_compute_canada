# EASYMORE Configuration for Clusters of Compute Canada

[EASYMORE](https://github.com/ShervanGharari/EASYMORE) is now available on [wheelhouse](https://docs.alliancecan.ca/wiki/Available_Python_wheels) on Digital Research Alliance of Canada (formely ComputeCanada) clusters.

To use EASYMORE on clusters managed by Digital Research Alliance of Canada, you can follow the below instructions either for creating virtual environemtn and small test of that on log in node or inside a job.

## On log in node

```
# go to home and remove all the modules and deactivate potential active 
cd
module reset
module purge
deactivate

# load needed modules
module load StdEnv/2020 gcc/9.3.0 openmpi/4.0.3
module load gdal/3.5.1 libspatialindex/1.8.5
module load python/3.8.10 scipy-stack/2022a mpi4py/3.0.3

# remove existing virtual env folder and set up virtual env for easymore
rm -rf ~/easymore-env
virtualenv ~/easymore-env
source ~/easymore-env/bin/activate
pip install --no-index --upgrade pip
pip install --no-index easymore[complete]

# check if the code run smoothly given the example on the easymore github repo
cd # go to home
rm -rf EASYMORE
git clone https://github.com/ShervanGharari/EASYMORE.git
cd EASYMORE/examples
pip install --no-index jupyter
jupyter nbconvert Chapter1_E1.ipynb --to python
python Chapter1_E1.py
#
```

## Inside job

```
#!/bin/bash
#SBATCH --account=group_name
#SBATCH --ntasks=1
#SBATCH --mem-per-cpu=32G
#SBATCH --time=24:00:00    # time (DD-HH:MM)
#SBATCH --job-name=SUMMA_post_processing
#SBATCH --error=errors1

# load needed modules
module load StdEnv/2020 gcc/9.3.0 openmpi/4.0.3
module load gdal/3.5.1 libspatialindex/1.8.5
module load python/3.8.10 scipy-stack/2022a mpi4py/3.0.3

# create virtual env inside the job
virtualenv --no-download $SLURM_TMPDIR/env
source $SLURM_TMPDIR/env/bin/activate
pip install --no-index --upgrade pip
pip install --no-index easymore[complete]

# OR use your locally created virtual env in home directory (created as explained on log in node; above)
# source ~/easymore-env/bin/activate # when this is uncommneted, then commnet above (virtualenv ...)

# run python script that include easymore remapper
python python_file_for_your_case.py
```
