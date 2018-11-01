### Download LAMMPS

Download the latest stable version of LAMMPS from [this page](http://lammps.sandia.gov/download.html#tar).

------ 

### Untar LAMMPS

~~~
$ tar -xf lammps.tar.gz
~~~

------

### Build auxiliary libraries for LAMMPS

*Note:* if you use the packages listed below, the corresponding libraries that are located at *./lib/* must be built first. See more details [here](http://lammps.sandia.gov/doc/Section_start.html#start-3-3). Otherwise, go to next step.
	
~~~
KIM LATTE MEAM POEMS REAX VORONOI LINALG USER-ATC USER-AWPMD USER-COLVARS USER-QMMM USER-SMD
~~~

#### KIM
	
~~~
$ cd ./src/
$ module purge
$ module load gcc/6.3.0/1
$ make lib-kim args="-b"
~~~

#### LATTE
	
~~~
$ cd ./src/
$ module purge
$ module load lapack/3.6.1 blas/3.6.0
$ make lib-latte args="-b"
~~~

#### MEAM

~~~
$ cd ./lib/meam/
$ module purge
$ module load openmpi/ofed/icc17/2.0.1 intel/compiler/64/2017.2.174
$ make -f Makefile.mpi
~~~
	
#### POEMS
	
~~~
$ cd ./lib/poems/
$ module purge
$ module load intel/compiler/64/2017.2.174
$ make -f Makefile.icc
~~~

#### REAX
	
~~~
$ cd ./lib/reax/
$ module purge
$ module load intel/compiler/64/2017.2.174
$ make -f Makefile.ifort
~~~

#### VORONOI
	
~~~
$ cd ./src/
$ module purge
$ module load gcc/6.3.0/1
$ make lib-voronoi args="-b"
~~~
	
#### LINALG
	
~~~
$ cd ./lib/linalg/
$ module purge
$ module load openmpi/ofed/icc17/2.0.1 intel/compiler/64/2017.2.174
$ make -f Makefile.mpi
~~~
	
#### USER-ATC
	
~~~
$ cd ./lib/atc/
$ module purge
$ module load intel/mpi/64/2017.2.174 gcc/6.3.0/1
$ make -f Makefile.mpic++
~~~
	
#### USER-AWPMD
	
~~~
$ cd ./lib/awpmd/
$ module purge
$ module load intel/mpi/64/2017.2.174 gcc/6.3.0/1
$ make -f Makefile.mpicc
~~~
	
#### USER-COLVARS
	
~~~
$ cd ./lib/colvars/
$ module purge
$ module load intel/mpi/64/2017.2.174 gcc/4.9.3
$ make -f Makefile.mpi
~~~
	
#### USER-QMMM
	
~~~
$ cd ./lib/qmmm/
$ module purge
$ module load intel/mpi/64/2017.2.174 gcc/6.3.0/1
$ make -f Makefile.mpi
~~~

#### USER-SMD
	
~~~
$ cd ./src/
$ module purge
$ make lib-smd args="-b"	
~~~

------

### Install packages

#### Install all packages including both standard and user-contributed packages
		
~~~
$ cd ./src/
$ make yes-all
$ make package-update
~~~
	
#### Exclude packages that are not required
		
~~~
$ cd ./src/
$ make no-gpu no-kim no-mscg no-user-h5md no-user-quip no-user-vtk
~~~
	
#### Check the included packages

~~~
$ cd ./src/
$ make ps | grep "YES:"
	Installed YES: package ASPHERE
	Installed YES: package BODY
	Installed YES: package CLASS2
	Installed YES: package COLLOID
	Installed YES: package COMPRESS
	Installed YES: package CORESHELL
	Installed YES: package DIPOLE
	Installed YES: package GRANULAR
	Installed YES: package KOKKOS
	Installed YES: package KSPACE
	Installed YES: package LATTE
	Installed YES: package MANYBODY
	......
~~~

------

### Build LAMMPS

#### Module environment

In order to be able to compile the LAMMPS code, the following modules must be loaded first.

~~~
$ module purge
$ module load intel/compiler/64/2017.2.174 intel/mkl/64/2017.2.174 intel/mpi/64/2017.2.174 intel/tbb/64/2017.2.174 gcc/4.9.3 lapack/3.6.1 blas/3.6.0  python/2.7.12
~~~

#### Makefile

Create a makefile (**Makefile.intelcpu_intelmpi_intel_omp_kokkos_Athena** for example) and put it in **./src/MAKE/MINE/** 

Copy and paste the contents below to the makefile. The makefile provided below is optimised for running LAMMPS on Intel hardware, especially when the USER-INTEL package is used. See the [benchmarking results](benchmarking.md) for details.

~~~
# Makefile.intelcpu_intelmpi_intel_omp_kokkos_Athena = user-intel, user-OMP and kokkos; intel compiler and intel MPI

SHELL = /bin/sh

# ---------------------------------------------------------------------
# compiler/linker settings
# specify flags and libraries needed for your compiler

CC =		mpiicpc 
OPTFLAGS =  -xHost -O2 -fp-model fast=2 -no-prec-div -qoverride-limits -L$(MKLROOT)/lib/intel64/ -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core
CCFLAGS =	-qopenmp -DLAMMPS_MEMALIGN=64 -qno-offload \
		-fno-alias -ansi-alias -restrict -DLMP_INTEL_USELRT -DLMP_USE_MKL_RNG $(OPTFLAGS)
SHFLAGS =	-fPIC
DEPFLAGS =	-M

LINK =		mpiicpc
LINKFLAGS =	-qopenmp $(OPTFLAGS)
LIB = 		-ltbbmalloc
SIZE =		size

ARCHIVE =	ar
ARFLAGS =	-rc
SHLIBFLAGS =	-shared
KOKKOS_DEVICES = OpenMP

# ---------------------------------------------------------------------
# LAMMPS-specific settings, all OPTIONAL
# specify settings for LAMMPS features you will use
# if you change any -D setting, do full re-compile after "make clean"

# LAMMPS ifdef settings
# see possible settings in Section 2.2 (step 4) of manual

LMP_INC =	-DLAMMPS_GZIP

# MPI library
# see discussion in Section 2.2 (step 5) of manual
# MPI wrapper compiler/linker can provide this info
# can point to dummy MPI library in src/STUBS as in Makefile.serial
# use -D MPICH and OMPI settings in INC to avoid C++ lib conflicts
# INC = path for mpi.h, MPI compiler settings
# PATH = path for MPI library
# LIB = name of MPI library

MPI_INC =       -DMPICH_SKIP_MPICXX -DOMPI_SKIP_MPICXX=1
MPI_PATH = 
MPI_LIB =	

# FFT library
# see discussion in Section 2.2 (step 6) of manual
# can be left blank to use provided KISS FFT library
# INC = -DFFT setting, e.g. -DFFT_FFTW, FFT compiler settings
# PATH = path for FFT library
# LIB = name of FFT library

FFT_INC =       -DFFT_MKL -DFFT_SINGLE
FFT_PATH = 
FFT_LIB =       -L$(MKLROOT)/lib/intel64/ -lmkl_intel_ilp64 \
						-lmkl_sequential -lmkl_core	

# JPEG and/or PNG library
# see discussion in Section 2.2 (step 7) of manual
# only needed if -DLAMMPS_JPEG or -DLAMMPS_PNG listed with LMP_INC
# INC = path(s) for jpeglib.h and/or png.h
# PATH = path(s) for JPEG library and/or PNG library
# LIB = name(s) of JPEG library and/or PNG library

JPG_INC =
JPG_PATH =
JPG_LIB =	

# ---------------------------------------------------------------------
# build rules and dependencies
# do not edit this section

include	Makefile.package.settings
include	Makefile.package

EXTRA_INC = $(LMP_INC) $(PKG_INC) $(MPI_INC) $(FFT_INC) $(JPG_INC) $(PKG_SYSINC)
EXTRA_PATH = $(PKG_PATH) $(MPI_PATH) $(FFT_PATH) $(JPG_PATH) $(PKG_SYSPATH)
EXTRA_LIB = $(PKG_LIB) $(MPI_LIB) $(FFT_LIB) $(JPG_LIB) $(PKG_SYSLIB)
EXTRA_CPP_DEPENDS = $(PKG_CPP_DEPENDS)
EXTRA_LINK_DEPENDS = $(PKG_LINK_DEPENDS)

# Path to src files

vpath %.cpp ..
vpath %.h ..

# Link target

$(EXE):$(OBJ) $(EXTRA_LINK_DEPENDS)
	$(LINK) $(LINKFLAGS) $(EXTRA_PATH) $(OBJ) $(EXTRA_LIB) $(LIB) -o $(EXE)
	$(SIZE) $(EXE)

# Library targets
lib:$(OBJ) $(EXTRA_LINK_DEPENDS)
	$(ARCHIVE) $(ARFLAGS) $(EXE) $(OBJ)

shlib:$(OBJ) $(EXTRA_LINK_DEPENDS)
	$(CC) $(CCFLAGS) $(SHFLAGS) $(SHLIBFLAGS) $(EXTRA_PATH) -o $(EXE) \
	$(OBJ) $(EXTRA_LIB) $(LIB)

# Compilation rules

%.o:%.cpp $(EXTRA_CPP_DEPENDS)
	$(CC) $(CCFLAGS) $(SHFLAGS) $(EXTRA_INC) -c $<

%.d:%.cpp $(EXTRA_CPP_DEPENDS)
	$(CC) $(CCFLAGS) $(EXTRA_INC) $(DEPFLAGS) $< > $@

%.o:%.cu $(EXTRA_CPP_DEPENDS)
	$(CC) $(CCFLAGS) $(SHFLAGS) $(EXTRA_INC) -c $<

# Individual dependencies

depend : fastdep.exe $(SRC)
	@./fastdep.exe $(EXTRA_INC) -- $^ > .depend || exit 1

fastdep.exe: ../DEPEND/fastdep.c
	cc -O -o $@ $<

sinclude .depend
~~~

#### Compiling

~~~
$ cd ./src/
$ make clean-all
$ make -j 10 intelcpu_intelmpi_intel_omp_kokkos_Athena
~~~
		
Here as an example, 10 CPU cores (**-j 10**) are used to compile the code. When it is finished, a binary file should be created in **./src/**. The name of the binary file is, for this case, **lmp_intelcpu_intelmpi_intel_omp_kokkos_Athena**.

This build that includes majority of LAMMPS packages should be sufficiently complete for simulating many systems.
