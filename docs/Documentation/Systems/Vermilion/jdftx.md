---
 layout: default
 title: Running on Vermilion
 parent: Vermilion
 grand_parent: Systems
---
## JDFTx

The [JDFTx package](http://jdftx.org) is a plane-wave density functional theory (DFT) code that calculates the electronic properties of optoelectronic and electrocatalytic systems, with a particular strength in describing solvated system under applied bias. JDFTx supports a range of exchange correlation functionals, dispersion corrections, several formats of norm-conserving and ultrasoft pseudopotentials that are pre-installed, and calculations of systems of any dimensionality from 0 to 3: molecules, wires, slabs / 2D materials and bulk. 

## Availability on Vermillion

The latest development version of JDFTx available on [GitHub](https://github.com/shankar1729/jdftx) is built on Vermillion. Additionally, it is built with both CPU and GPU support. Please refer to the sample job scripts provided below for how to run for CPUs and GPUs.  


## Application Information, Documentation, and Support

JDFTx is freely available and can be downloaded from [GitHub](https://github.com/shankar1729/jdftx). Extensive online documentation, including tutorials, input command documentation, and compilation instructions, can be found on the [JDFTx homepage](http://jdftx.org). For troubleshooting, see the [issues portion](https://github.com/shankar1729/jdftx/issues) of the Github site and open a new issue if the issue has not been raised before. If there are any issues with the JDFTx module, please submit a ticket to HPC-Help. 


## Using JDFTx on Vermillion

DVF: Frankly, this section doesn't seem that valuable on the NERSC site. If we want to have it, it would likely need to be after the module for JDFTx has been created. 

## Sample Job Scripts

<details>
<summary>Vermillion CPU</summary>
<br>

```
#!/bin/bash
#SBATCH --job-name=vasp
#SBATCH --nodes=1
#SBATCH --time=8:00:00
##SBATCH --error=std.err
##SBATCH --output=std.out
#SBATCH --partition=sm
#SBATCH --exclusive

cat $0

hostname

source /nopt/nrel/apps/210929a/myenv.2110041605

module purge
ml gcc
ml vasp

# some extra lines that have been shown to improve VASP reliability on Vermilion
ulimit -s unlimited
export UCX_TLS=tcp,self
export OMP_NUM_THREADS=1

# lines to set "ens7" as the interconnect network
module use /nopt/nrel/apps/220525b/level01/modules/lmod/linux-rocky8-x86_64/gcc/12.1.0
module load openmpi
OMPI_MCA_param="btl_tcp_if_include ens7"

#### wget is needed to download data
ml wget

#### get input and set it up
#### This is from an old benchmark test
#### see https://github.nrel.gov/ESIF-Benchmarks/VASP/tree/master/bench2


mkdir input

wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/INCAR?token=AAAALJZRV4QFFTS7RC6LLGLBBV67M   -q -O INCAR
wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/POTCAR?token=AAAALJ6E7KHVTGWQMR4RKYTBBV7SC  -q -O POTCAR
wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/POSCAR?token=AAAALJ5WKM2QKC3D44SXIQTBBV7P2  -q -O POSCAR
wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/KPOINTS?token=AAAALJ5YTSCJFDHUUZMZY63BBV7NU -q -O KPOINTS

srun --mpi=pmi2 -n 16 vasp_std

```
 
</details>

<details>
<summary>Vermillion GPU</summary>
<br> 

```
#!/bin/bash
#SBATCH --job-name=vasp
#SBATCH --nodes=2
#SBATCH --time=1:00:00
##SBATCH --error=std.err
##SBATCH --output=std.out
#SBATCH --partition=gpu
#SBATCH --gpu-bind=map_gpu:0,1,0,1
#SBATCH --exclusive

cat $0

hostname

#load necessary modules and set library paths
module use  /nopt/nrel/apps/220421a/modules/lmod/linux-rocky8-x86_64/gcc/11.3.0/
ml nvhpc
ml gcc
ml fftw
export LD_LIBRARY_PATH=/nopt/nrel/apps//220421a/install/opt/spack/linux-rocky8-zen2/gcc-11.3.0/nvhpc-22.2-ruzrtpyewnnrif6s7w7rehvpk7jimdrd/Linux_x86_64/22.2/compilers/extras/qd/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/nopt/nrel/apps//220421a/install/opt/spack/linux-rocky8-zen2/gcc-11.3.0/gcc-11.3.0-c3u46uvtuljfuqimb4bgywoz6oynridg/lib64:$LD_LIBRARY_PATH

#add a path to the gpu build of VASP to your script
export PATH=/projects/hpcapps/tkaiser2/vasp/6.3.1/nvhpc_acc:$PATH

#### wget is needed to download data
ml wget

#### get input and set it up
#### This is from an old benchmark test
#### see https://github.nrel.gov/ESIF-Benchmarks/VASP/tree/master/bench2


mkdir input

wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/INCAR?token=AAAALJZRV4QFFTS7RC6LLGLBBV67M   -q -O INCAR
wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/POTCAR?token=AAAALJ6E7KHVTGWQMR4RKYTBBV7SC  -q -O POTCAR
wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/POSCAR?token=AAAALJ5WKM2QKC3D44SXIQTBBV7P2  -q -O POSCAR
wget https://github.nrel.gov/raw/ESIF-Benchmarks/VASP/master/bench2/input/KPOINTS?token=AAAALJ5YTSCJFDHUUZMZY63BBV7NU -q -O KPOINTS

mpirun -npernode 1 vasp_std > vasp.$SLURM_JOB_ID
```

</details>

## Building JDFTx from Source on Vermillion

Some users may want to build a more recent release of JDFTx than is available on Vermillion, or modify the source code. To do so, one must clone the JDFTx repository 

```
git clone https://github.com/shankar1729/jdftx.git your_JDFTX_directory_name
```

and go to the main directory 

```
cd your_JDFTX_directory_name/jdftx
```

JDFTx requires multiple software packages that handle things such as matrix diagonalization and fast fourier transforms, and is built with cmake, so modules for all of these must be loaded before executing the needed commands for the actual compilation. Here is the set of commands needed to build JDFTx, with annotations about the purpose of different commands

```
source /nopt/nrel/apps/210929a/myenv.2110041605   # get appropriate environment
module use /nopt/nrel/apps/210929a/level02/modules/lmod/linux-rocky8-x86_64/openmpi/4.1.1-ce66uss/Core # get set of modules needed
module use /nopt/nrel/apps/210929a/level02/modules/lmod/linux-rocky8-x86_64/Core
ml cmake gcc intel-oneapi-mkl hdf5 gsl openmpi/4.1.1cuda-xo4gxni cuda  # load modules
export HDF5_ROOT_DIR=/nopt/nrel/apps/210928a/level02/gcc-9.4.0/hdf5-1.12.1  #give cmake paths to certain libraries so it knows where to find them
export CMAKE_PREFIX_PATH=$HDF5_ROOT_DIR
export GSL_ROOT_DIR=/nopt/nrel/apps/210929a/level02/gcc-9.4.0/gsl-2.7-k2w2ugrj5wvhplcvtdarimrmy6vikqvl
mkdir build  # make build direcory and go there, execute actual cmake command, and then compile JDFTx with make
cd build
CC="gcc" CXX="c++" cmake -D EnableMKL=yes -D EnableScaLAPACK=yes -D EnableCUDA=yes -D EnableCuSolver=yes -D EnableHDF5=yes -D EnableProfiling=yes -D CudaAwareMPI=yes -D PinnedHostMemory=yes -D CUDA_ARCH='compute_80' -D CUDA_CODE='sm_80' -D MKL_PATH="$MKLROOT" -D GSL_PATH=$GSL_ROOT_DIR ../
make -j
```

After the last command is executed, the JDFTx executables will be generated in the build directory.

