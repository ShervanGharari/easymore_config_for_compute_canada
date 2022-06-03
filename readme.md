```
module reset
module load StdEnv/2020 gcc/9.3.0 openmpi/4.0.3
module load gdal/3.4.1 libspatialindex/1.8.5
module load python/3.8.10 scipy-stack/2022a mpi4py/3.0.3

rm -rf ~/easymore-env
virtualenv ~/easymore-env
source ~/easymore-env/bin/activate
pip install --no-index --upgrade pip
pip install --no-index easymore
pip install --no-index Rtree # the dependency I should add to the setup.py
pip install --no-index jupyter # just to convert the notebook to python here (not needed by easymore)

cd # go to home
rm -rf EASYMORE
git clone https://github.com/ShervanGharari/EASYMORE.git
cd EASYMORE/examples/
jupyter nbconvert Chapter1_E1.ipynb --to python
python Chapter1_E1.py
```

```
module load gcc/9.3.0
module load geo-stack

cd # go to home
rm -rf EASYMORE
git clone https://github.com/ShervanGharari/EASYMORE.git
cd EASYMORE/examples/
jupyter nbconvert Chapter1_E1.ipynb --to python
python Chapter1_E1.py
```


```
#!/bin/bash
#SBATCH --account=rpp-kshook
#SBATCH --ntasks=1
#SBATCH --mem-per-cpu=2G
#SBATCH --time=24:00:00           # time (DD-HH:MM)
#SBATCH --job-name=SUMMA_post_processing
#SBATCH --error=errors1

# load modules
module load StdEnv/2020 gcc/9.3.0 openmpi/4.0.3
module load gdal/3.4.1 libspatialindex/1.8.5
module load python/3.8.10 scipy-stack/2022a mpi4py/3.0.3

# create virtual env inside the job
virtualenv --no-download $SLURM_TMPDIR/env
source $SLURM_TMPDIR/env/bin/activate
pip install --no-index --upgrade pip
pip install --no-index easymore
pip install --no-index Rtree # the dependency I should add to the setup.py

python python_file_for_your_case.py
```
