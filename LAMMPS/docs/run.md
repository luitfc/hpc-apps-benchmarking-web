### Run with the USER-INTEL package

The [USER-INTEL](http://lammps.sandia.gov/doc/accelerate_intel.html) package is able to accelerate simulations on Intel CPUs by running in single, mixed, or double precision with vectorization. The styles currently supported by the USER-INTEL package are available [here](http://lammps.sandia.gov/doc/accelerate_intel.html). Simulations should be run with 1 MPI task per physical core, not hardware thread. 

On the [compute partition](http://www.hpc-midlands-plus.ac.uk/user-support/quick-start/) of HPC Midlands+, each computer node has 28 cores and each single core supports only 1 thread as the hardware supported multithreading (known as [Hyper-Threading](https://www.intel.co.uk/content/www/uk/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)) has been disabled in default.

The following commands are examples of running LAMMPS with the **USER-INTEL** package and different **MPI** and **OMP** configurations. 

**-np** specifies the number of MPI ranks while **-ppn** indicates the number of MPI tasks per node. **lmp_exe** is the filename of the compiled LAMMPS executable file and **-sf intel** enables using the USER-INTEL package during your simulations.
 
**-pk intel 0** : don't use coprocessors as they are not available on HPC Midlands+. **omp** specifies the number of the threads per physical core. This number should be set to 1 as the Hyper-Threading has been disabled, otherwise it can be 2 with which extra performance might be achieved.

~~~
$ mpirun -np 1 lmp_exe -sf intel -pk intel 0 omp 1 -in lmp.in          # 1 core; 1 MPI task in total
$ mpirun -np 28 -ppn 28 lmp_exe -sf intel -pk intel 0 omp 1 -in lmp.in # 1 node;  28 MPI tasks/node; 28 MPI tasks in total
$ mpirun -np 56 -ppn 28 lmp_exe -sf intel -pk intel 0 omp 1 -in lmp.in # 2 nodes; 28 MPI tasks/node; 56 MPI tasks in total
$ mpirun -np 28 -ppn 14 lmp_exe -sf intel -pk intel 0 omp 1 -in lmp.in # 2 nodes; 14 MPI tasks/node; 28 MPI tasks in total
~~~

Example batch script:
	
~~~
#!/bin/bash -l
# SLURM submission script

#SBATCH --job-name=LAMMPS-Intel
#SBATCH --time=01:00:00
#SBATCH --partition=compute
#SBATCH --nodes=2
#SBATCH -A account

# load modules that are required to run LAMMPS
module load intel/compiler/64/2017.2.174 intel/mkl/64/2017.2.174 intel/mpi/64/2017.2.174 intel/tbb/64/2017.2.174 gcc/4.9.3 lapack/3.6.1 blas/3.6.0  python/2.7.12

# OMP
export KMP_BLOCKTIME=0
export OMP_PROC_BIND=spread
export OMP_PLACES=threads

# ------------------------ MPI and OMP settings start
export MPI="mpirun" # MPI command
export MPI_TASKS=56
export MPI_PER_NODE=28
export THREADS_PER_TASK=1
# ------------------------ MPI and OMP settings end

# ------------------------ LAMMPS settings start
export LMP_ROOT=~/projects/Lammps/lammps-23Oct17/
export LMP_BIN=~/projects/Lammps/lammps-23Oct17/src/intelcpu_intelmpi_intel_omp_kokkos_Athena
export LMP_IN=../../lammps_in/lmp.in
export LMP_LOG="lmp.log"
export LMP_ARGS="-sf intel -pk intel 0 omp $THREADS_PER_TASK -in $LMP_IN -log $LMP_LOG -screen none"
# ------------------------ LAMMPS settings end 

# run
$MPI -np $MPI_TASKS -ppn $MPI_PER_NODE $LMP_BIN $LMP_ARGS
	
# more
~~~	

------

### Run with the USER-OMP package

For styles that are not supported by the USER-INTEL package, alternatively the USER-OMP package can be used to speedup your simulations.

Example commands:
	
~~~
$ mpirun -np 1 lmp_exe -sf omp -pk omp 28 -in lmp.in        # 1 node; 1 MPI task; 1 MPI task/node; 28 threads/MPI task
$ mpirun -np 4 -ppn 4 lmp_exe -sf omp -pk omp 7 -in lmp.in  # 1 node; 4 MPI tasks; 4 MPI tasks/node; 7 threads/MPI task
$ mpirun -np 4 -ppn 2 lmp_exe -sf omp -pk omp 14 -in lmp.in # 2 nodes; 4 MPI tasks; 2 MPI tasks/node; 14 threads/MPI task
$ mpirun -np 5 -ppn 1 lmp_exe -sf omp -pk omp 28 -in lmp.in # 5 nodes; 5 MPI tasks; 1 MPI task/node; 28 threads/MPI task
~~~
	
Example batch script:
	
~~~
#!/bin/bash -l
# SLURM submission script
	
#SBATCH --job-name=LAMMPS-OMP
#SBATCH --time=01:00:00
#SBATCH --partition=compute
#SBATCH --nodes=2
#SBATCH -A account

# load modules that are required to run LAMMPS
module load intel/compiler/64/2017.2.174 intel/mkl/64/2017.2.174 intel/mpi/64/2017.2.174 intel/tbb/64/2017.2.174 gcc/4.9.3 lapack/3.6.1 blas/3.6.0  python/2.7.12

# OMP
export KMP_BLOCKTIME=0
export OMP_PROC_BIND=spread
export OMP_PLACES=threads

# ------------------------ MPI and OMP settings start
export MPI="mpirun" # MPI command
export MPI_TASKS=4
export MPI_PER_NODE=2
export THREADS_PER_TASK=14
# ------------------------ MPI and OMP settings end

# ------------------------ LAMMPS settings start
export LMP_ROOT=~/projects/Lammps/lammps-23Oct17/
export LMP_BIN=~/projects/Lammps/lammps-23Oct17/src/intelcpu_intelmpi_intel_omp_kokkos_Athena
export LMP_IN=../../lammps_in/lmp.in
export LMP_LOG="lmp.log"
export LMP_ARGS="-sf omp -pk omp $THREADS_PER_TASK -in $LMP_IN -log $LMP_LOG -screen none"
# ------------------------ LAMMPS settings end 

# run
$MPI -np $MPI_TASKS -ppn $MPI_PER_NODE $LMP_BIN $LMP_ARGS
	
# more
~~~

------

### Run with the KOKKOS package

Example commands:
	
~~~
$ mpirun -np 28 -ppn 28 lmp_exe -k on t 1 -sf kk -in lmp.in  # 1 node; 28 MPI tasks; 28 MPI tasks/node; 1 thread/MPI task
$ mpirun -np 14 -ppn 14 lmp_exe -k on t 2 -sf kk -in lmp.in  # 1 node; 14 MPI tasks; 14 MPI tasks/node; 2 threads/MPI task
$ mpirun -np 14 -ppn 7 lmp_exe -k on t 4 -sf kk -in lmp.in   # 2 nodes; 14 MPI tasks; 7 MPI tasks/node; 4 threads/MPI task
$ mpirun -np 40 -ppn 4 lmp_exe -k on t 7 -sf kk -in lmp.in   # 10 nodes; 40 MPI tasks; 4 MPI tasks/node; 7 threads/MPI task
~~~
	
Example batch script:
	
~~~
#!/bin/bash -l
# SLURM submission script
	
#SBATCH --job-name=LAMMPS-KOKKOS
#SBATCH --time=01:00:00
#SBATCH --partition=compute
#SBATCH --nodes=10
#SBATCH -A account

# load modules that are required to run LAMMPS
module load intel/compiler/64/2017.2.174 intel/mkl/64/2017.2.174 intel/mpi/64/2017.2.174 intel/tbb/64/2017.2.174 gcc/4.9.3 lapack/3.6.1 blas/3.6.0  python/2.7.12

# OMP
export KMP_BLOCKTIME=0
export OMP_PROC_BIND=spread
export OMP_PLACES=threads

# ------------------------ MPI and OMP settings start
export MPI="mpirun" # MPI command
export MPI_TASKS=40
export MPI_PER_NODE=4
export THREADS_PER_TASK=7
# ------------------------ MPI and OMP settings end

# ------------------------ LAMMPS settings start
export LMP_ROOT=~/projects/Lammps/lammps-23Oct17/
export LMP_BIN=~/projects/Lammps/lammps-23Oct17/src/intelcpu_intelmpi_intel_omp_kokkos_Athena
export LMP_IN=../../lammps_in/lmp.in
export LMP_LOG="lmp.log"
export LMP_ARGS="-k on t $THREADS_PER_TASK -sf kk -in $LMP_IN -log $LMP_LOG -screen none"
# ------------------------ LAMMPS settings end 

# run
$MPI -np $MPI_TASKS -ppn $MPI_PER_NODE $LMP_BIN $LMP_ARGS
	
# more
~~~
