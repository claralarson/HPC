### On Eagle

Load VASP with Intel MPI:
```
ml vasp
```
[script to run VASP on Eagle with Intel MPI](https://github.com/NREL/HPC/blob/master/applications/vasp/Performance%20Study%202/VASP%20scripts/Eagle_IntelMPI.slurm)

Load VASP with Open MPI:
```
source /nopt/nrel/apps/210830a/myenv.2108301742
ml vasp/6.1.1-l2mkbb2
```
[script to run VASP on Eagle with Open MPI](https://github.com/NREL/HPC/blob/master/applications/vasp/Performance%20Study%202/VASP%20scripts/Eagle_OpenMPI.slurm)

Load the GPU build of VASP:
```
module use /nopt/nrel/apps/220511a/modules/lmod/linux-centos7-x86_64/gcc/12.1.0
ml fftw nvhpc
export LD_LIBRARY_PATH=/nopt/nrel/apps/220511a/install/opt/spack/linux-centos7-skylake_avx512/gcc-12.1.0/nvhpc-22.3-c4qk6fly5hls3mjimoxg6vyuy5cc3vti/Linux_x86_64/22.3/compilers/extras/qd/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/nopt/nrel/apps/220511a/install/opt/spack/linux-centos7-skylake_avx512/gcc-12.1.0/nvhpc-22.3-c4qk6fly5hls3mjimoxg6vyuy5cc3vti/Linux_x86_64/22.3/compilers/extras/qd/lib:$LD_LIBRARY_PATH
export PATH=/projects/hpcapps/tkaiser2/vasp/6.3.1/nvhpc_acc:$PATH
```
[script to run VASP on Eagle on GPU nodes](https://github.com/NREL/HPC/blob/master/applications/vasp/Performance%20Study%202/VASP%20scripts/Eagle_OpenACC_GPU.slurm)

### On Swift 
Load VASP with Intel MPI:
```
ml vaspintel 
ml slurm/21-08-1-1-o2xw5ti 
ml gcc/9.4.0-v7mri5d 
ml intel-oneapi-compilers/2021.3.0-piz2usr 
ml intel-oneapi-mpi/2021.3.0-hcp2lkf 
ml intel-oneapi-mkl/2021.3.0-giz47h4
```
[script to run VASP on Swift with Intel MPI](https://github.com/NREL/HPC/blob/master/applications/vasp/Performance%20Study%202/VASP%20scripts/Swift_IntelMPI.slurm)

Load VASP with Open MPI:
```
ml vasp 
ml slurm/21-08-1-1-o2xw5ti 
ml openmpi/4.1.1-6vr2flz
```
[script to run VASP on Swift with Open MPI](https://github.com/NREL/HPC/blob/master/applications/vasp/Performance%20Study%202/VASP%20scripts/Swift_OpenMPI.slurm)
