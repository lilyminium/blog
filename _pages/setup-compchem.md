---
layout: single
title: How to compile things
permalink: /setup-compchem
image: images/disco_dingo.jpg
featured: true
comments: true
sidebar:
  nav: pages/setup-compchem
---


A log of the installation dependencies and processes of annoying packages, as well as said packages.

# GROMACS

## 2016.1
This requires cuda 8.0, which requires gcc 5.

### Installing GCC
You can mostly find gcc 6+ in the Disco Dingo repository and install them with `apt install gcc-8` etc. However, older versions require a bit more effort.

#### GCC 5.4
Download the source from the GCC website. Unpack it and make a build directory ([instructions copied from the GCC website](https://gcc.gnu.org/wiki/InstallingGCC)):

```console
lily@gavle$ tar xzf gcc-5.4.0.tar.gz
lily@gavle$ cd gcc-5.4.0
lily@gavle$ ./contrib/download_prerequisites
lily@gavle$ cd ..
lily@gavle$ mkdir objdir
lily@gavle$ cd objdir
```
Now you need to modify `libgcc/config/i386/linux_unwind.h` to change two instances of `struct ucontext *uc_` to `struct ucontext_t *uc_`. In GCC 6.4 these were on lines 61 and 141, but there may be minor differences. (This is not required for the patched version available on our shared drive, O'Mara group readers.)

Now configure and checkinstall. I've configured this to install on a hard drive mounted at `/store`, but you can equally install it in your home directory or elsewhere.
```console
lily@gavle$ $PWD/../gcc-5.4.0/configure --prefix=/store/opt/gcc-5.4.0 --enable-languages=c,c++,fortran --disable-multilib
lily@gavle$ make
lily@gavle$ sudo checkinstall
```
**Make sure your checkinstall package has a version number, or it'll fail!**

### Installing CUDA



### Installing GROMACS

#### Process
```console
lily@gavle:$ tar xfz gromacs-2016.1.tar.z
lily@gavle:$ cd gromacs-2016.1
lily@gavle:$ mkdir build && cd build
lily@gavle:$ cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/packages/gromacs/gromacs-2016.1 -DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/packages/cuda/cuda-8.0
lily@gavle:$ make -j 6
lily@gavle:$ make check
lily@gavle:$ sudo checkinstall
```

## 2018.3
This is a lot easier.

#### Process
```console
lily@gavle:$ cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/packages/gromacs/gromacs-2018.3 -DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/packages/cuda/10.1
lily@gavle:$ make -j 6
lily@gavle:$ make check
lily@gavle:$ sudo checkinstall
```

<a name="amber"/>

# AMBER

## Amber16
This is the version we have the license for, but I imagine they're all similar.

## AmberTools 19
### Dependencies

gcc=8.3.0

cuda=10.1

```console
lily@gavle:$ gcc --version
gcc (Ubuntu 8.3.0-6ubuntu1) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

lily@gavle:$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Fri_Feb__8_19:08:17_PST_2019
Cuda compilation tools, release 10.1, V10.1.105
```

### Process
```console
lily@gavle:$ cd /packages/amber/18
lily@gavle:$ ./configure -cuda gnu
lily@gavle:$ sudo checkinstall
```

**Note:** I ran into trouble with AMBER not finding $CUDA_HOME. After you've made sure it is set with echo $CUDA_HOME, one solution is:

```console
lily@gavle:$ echo $CUDA_HOME && checkinstall
```

However, if you are installing at a system level and require root permissions, `echo $CUDA_HOME && sudo checkinstall` *did not* work for me. The only way to get around it was to start a root session.

```console
lily@gavle:$ sudo su
root@gavle:$ export CUDA_HOME=/packages/cuda/10.1
root@gavle:$ echo $CUDA_HOME && checkinstall
```

I also symlinked `/bin/cpptraj.cuda` to `/bin/cpptraj` .
