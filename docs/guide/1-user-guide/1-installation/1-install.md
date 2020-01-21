title: Install Guide

# Install Guide

[TOC]

---

## Prerequisites

You absolutely need:

* [CMake](https://cmake.org/) (3.10+)
* GNU make or Ninja
* a Fortran compiler which supports at least Fortran 2008 (including the TS 29113 when using the C-bindings)
* a BLAS+LAPACK implementation (reference, OpenBLAS and MKL have been tested. Note: DBCSR linked to OpenBLAS 0.3.6 gives wrong results on Power9 architectures.)
* a Python version installed (2.7 or 3.6+ have been tested)

Optionally:

* [libxsmm](https://github.com/hfp/libxsmm) (1.10+) for Small Matrix Multiplication acceleration
* a LAPACK implementation (reference, OpenBLAS-bundled and MKL have been tested), required when building the tests

To build [libsmm_acc](src/acc/libsmm_acc/) (DBCSR's GPU backend), you further need:

* A GPU-capable compiler, either
  * CUDA Toolkit (targets NVIDIA GPUs, minimal version required: 5.5)
  * or HIP compiler (targets NVIDIA or AMD GPUs)
* a C++ compiler which supports at least C++11 standard

We test against GNU and Intel compilers on Linux systems, GNU compiler on MacOS systems.

## Get DBCSR

Download either a [release tarball](https://github.com/cp2k/dbcsr/releases) or clone the latest version from Git using:

    git clone --recursive https://github.com/cp2k/dbcsr.git

## Build

Run inside the `dbcsr` directory:

    mkdir build
    cd build
    cmake ..
    make

 The configuration flags for the CMake command are (default first):

    -DUSE_MPI=<ON|OFF>
    -DUSE_OPENMP=<ON|OFF>
    -DUSE_SMM=<blas|libxsmm>
    -DUSE_CUDA=<OFF|ON>
    -DUSE_CUBLAS=<OFF|ON>
    -DUSE_HIP=<OFF|ON>
    -DUSE_HIPBLAS=<OFF|ON>
    -DWITH_C_API=<ON|OFF>
    -DWITH_EXAMPLES=<ON|OFF>
    -DWITH_GPU=<P100|K20X|K40|K80|V100|Mi50>
    -DCMAKE_BUILD_TYPE=<Release|Debug|Coverage>
    -DBUILD_TESTING=<ON|OFF>
    -DTEST_MPI_RANKS=<auto,N>
    -DTEST_OMP_THREADS=<2,N>

### CMake Build Recipes

For build recipes on different platforms, make sure to also read the [CMake Build Recipes](docs/user-guide/installation/cmake-build-recipes.md).

### Using Python in a virtual environment

If you want to use Python from a virtual environment and your CMake version is < 3.15, specify the desired python interpreter manually using:

    -DPython_EXECUTABLE=/path/to/python

### Running Tests

To run the tests, use:

    make test

### C/C++ Interface

If MPI support is enabled (the default), the C API is automatically built.

### Workaround issue in HIP

HIP is a relatively new language, and some issues still need to be ironed out. As a workaround to an [issue](https://github.com/ROCm-Developer-Tools/HIP/pull/1543) in HIP's JIT infrastructure, please set the following if you've built HIP from source:

    export HIP_PATH=/opt/rocm/hip

before running on an AMD GPU.