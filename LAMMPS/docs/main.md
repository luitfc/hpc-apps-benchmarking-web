### LAMMPS

LAMMPS (Large-scale Atomic/Molecular Massively Parallel Simulator) is a classical molecular dynamics code, and an acronym for Large-scale Atomic/Molecular Massively Parallel Simulator. It has a nice collection of atom styles and force fields, and is capable of modelling systems with only a few particles up to millions or billions. 

LAMMPS is designed to run efficiently on single-processor desktop or shared-memory and distributed-memory parallel machines that support [MPI](https://www.open-mpi.org/) message-passing libraries. The LAMMPS code contains a large number of standard packages and user contributed packages including acceleration packages for improving LAMMPS performance for different classes of problems running on different kinds of hardware (multi-core CPUs, GPUs, and Intel Xeon Phi coprocessors). For full documentation visit the [official site](http://lammps.sandia.gov/doc/Manual.html).

### Build LAMMPS on HPC Midlands+

This document describes how to install LAMMPS packages and compile the LAMMPS code on HPC Midlands+ using Intel compiler and Intel MPI. A better performance of LAMMPS can be achieved on HPC Midlands+ when compiling the codes by using the optimised makefile that has been included [here](build.md), particularly when the USER-INTEL package is used.

[Read more ...](build.md)

### Run LAMMPS on HPC Midlands+

This section explains how to run LAMMPS on HPC Midlands+ using different acceleration packages and computer resources, and how to setup MPI ranks and OMP threads that are suitable for the hardware of HPC Midlands+. For each case, example codes are also provided as your reference.
	
[Read more ...](run.md)

### Benchmarking

The benchmarking aims at providing a representative performance figure for running LAMMPS application on HPC Midlands+. The performance analysis is carried out by comparing the performance of the optimisation packages (USER-INTEL, USER-OMP and KOKKOS) against that of the standard LAMMPS that runs without any optimisation packages.

The typical potentials that are available at **./src/USER-INTEL/TEST/** are used for the benchmarking. The results for both [strong scaling](https://www.sharcnet.ca/help/index.php/Measuring_Parallel_Scaling_Performance) and [weak scaling](https://www.sharcnet.ca/help/index.php/Measuring_Parallel_Scaling_Performance) are present. 		
[Read more ...](benchmarking.md)

