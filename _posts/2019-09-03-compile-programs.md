---
categories:
- how-to
- ubuntu
comments: true
featured: false
image: images/gromacs.png
layout: single
sidebar:
  nav: 2019-09-03-compile-programs
tags: null
title: How to compile things

---

**NOTE: this has now been moved to a [permanent group resource](https://omaralab.github.io/sushi/new_computer.html)**

A log of the installation dependencies and processes of annoying packages, as well as said packages. **Begun 29th July 2019, updated September 3rd.** 


# GROMACS

## g_mydensity

[g_mydensity](http://perso.ibcp.fr/luca.monticelli/tools/index.html) is a great tool with an extremely specific required environment. While it seems to have previously compiled with GROMACS 5.1.4, that may have been a fever dream because it definitely didn't in 2019. The program is tested against GROMACS 4.5.x; we compiled 4.5.7 for this. GROMACS 4.5.7 requires `fftw-3`.

**Start off by setting your compiler flags with -fPIC.**
```console
$ export CXXFLAGS=-fPIC
$ export CFLAGS=-fPIC
```

### Install fftw-3

We downloaded [FFTE 3.3.8](http://www.fftw.org/download.html).

```bash
$ tar xvf fftw-3.3.8.tar
$ cd fftw-3.3.8
$ ./configure --enable-threads --enable-float
$ make -j 12
$ sudo checkinstall
```

If something goes wrong here, enable fPIC, run `make clean`, and try again.

### Install GROMACS 4.5.7

```console
$ cd gromacs-4.5.7/
$ mkdir build && cd build
$ cmake ..
$ make -j 12
$ make test
$ sudo checkinstall
```

### Install g_mydensity

We download ours from [Luca Monticelli's website.](http://perso.ibcp.fr/luca.monticelli/tools/index.html) You must source GROMACS 4.5.7 for this to compile. As we have installed 4.5.7 into the default path, it is shown here.

We then symlink it to `/usr/local/bin`.

```console
$ mv jbarnoud-g_mydensity-4f9a93c151a9 g_mydensity && cd g_mydensity
$ source /usr/local/gromacs/bin/GMXRC
$ make
$ sudo chmod a+x g_mydensity
$ sudo ln -s /store/opt/g_mydensity/g_mydensity /usr/local/bin/g_mydensity
```

## 2016.1

This requires cuda 8.0, which requires gcc 5.

### Installing GCC

You can mostly find gcc 6+ in the Disco Dingo repository and install them with `apt install gcc-8` etc. However, older versions require a bit more effort.

#### GCC 5.4

Download the source from the GCC website. Unpack it and make a build directory ([instructions copied from the GCC website](https://gcc.gnu.org/wiki/InstallingGCC)):

```console
$ tar xzf gcc-5.4.0.tar.gz
$ cd gcc-5.4.0
$ ./contrib/download_prerequisites
$ cd ..
$ mkdir objdir
$ cd objdir
```
Now you need to modify `libgcc/config/i386/linux_unwind.h` to change two instances of `struct ucontext *uc_` to `struct ucontext_t *uc_`. In GCC 6.4 these were on lines 61 and 141, but there may be minor differences. (This is not required for the patched version available on our shared drive, O'Mara group readers.)

Now configure and checkinstall. I've configured this to install on a hard drive mounted at `/store`, but you can equally install it in your home directory or elsewhere.
```console
$ $PWD/../gcc-5.4.0/configure --prefix=/store/opt/gcc-5.4.0 --enable-languages=c,c++,fortran --disable-multilib
$ make
$ sudo checkinstall
```
**Make sure your checkinstall package has a version number, or it'll fail!**

At this point you may wish to set up `update-alternatives` for easy switching between compilers. You may not have all the gcc's below – if you do need them, the below command will do it.

```console
$ sudo apt install gcc-6 g++-6 gcc-7 g++-7
```

The lines below clear the alternatives you already have, and then configure the program path (second last field, eg `/store/opt/gcc-5.4.0/bin/gcc`) to the first field (eg `/usr/bin/gcc`) with the priority of the last number. A larger number indicates a higher priority. 
```console
$ sudo update-alternatives --remove-all gcc
$ sudo update-alternatives --remove-all g++
$ sudo update-alternatives --install /usr/bin/gcc gcc /store/opt/gcc-5.4.0/bin/gcc 10
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 20
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 30
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 40
$ sudo update-alternatives --install /usr/bin/g++ g++ /store/opt/gcc-5.4.0/bin/g++ 10
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 20
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 30
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 40
```

To switch between them:
```console
$ sudo update-alternatives --config gcc
```

### Installing CUDA
Make sure you have the patched version of `cuda_8.0.61_375.26_linux.run`. Unpack this and copy `InstallUtils.pm` to your computer.
```console
$ ./cuda_8.0.61_375.25_linux.run --tar mxvf
$ sudo cp InstallUtils.pm /usr/lib/x86_64-linux-gnu/perl-base
$ export $PERL5LIB
```
You can run through this interactively or with flags. If you remove the flags below, the interactive installer will come up. We only want to install the toolkit (nothing else), to the path specified. 
```console
$ sudo sh cuda_8.0.61_375.25_linux.run --toolkit \
--toolkitpath=/store/packages/cuda/cuda-8.0 --silent
```
Check to see that `/store/packages/cuda/cuda-8.0/bin/nvcc` exists.

### Installing GROMACS
Make sure you have `cmake` installed. You can get this with `sudo apt install cmake`. 
#### Process
```console
$ tar xfz gromacs-2016.1.tar.gz
$ cd gromacs-2016.1
$ mkdir build && cd build
$ cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON \
-DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/store/packages/gromacs/gromacs-2016.1 \
-DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/store/packages/cuda/cuda-8.0
$ make -j 6
$ make check
$ sudo checkinstall
```

## 2018.3
This is a lot easier.

### Process
```console
$ cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON \
-DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/store/packages/gromacs/gromacs-2018.3 \
-DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/store/packages/cuda/10.1
$ make -j 6
$ make check
$ sudo checkinstall
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
gcc --version
gcc (Ubuntu 8.3.0-6ubuntu1) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Fri_Feb__8_19:08:17_PST_2019
Cuda compilation tools, release 10.1, V10.1.105
```

### Process
```console
$ sudo apt-get install csh flex bison patch gcc gfortran g++ make\
build-essential xorg-dev xutils-dev libbz2-dev zlib1g-dev
$ cd /store/packages/amber/18
$ ./configure gnu
$ sudo checkinstall
```

**Note:** I ran into trouble with AMBER not finding $CUDA_HOME. After you've made sure it is set with echo $CUDA_HOME, one solution is:

```console
$ echo $CUDA_HOME && checkinstall
```

However, if you are installing at a system level and require root permissions, `echo $CUDA_HOME && sudo checkinstall` *did not* work for me. The only way to get around it was to start a root session.

```console
$ sudo su
$ export CUDA_HOME=/store/packages/cuda/10.1
$ echo $CUDA_HOME && checkinstall
```

I also symlinked `/bin/cpptraj.cuda` to `/bin/cpptraj` .

# Visualisation programs
## VMD
This is quite straightforward; the only key is making sure you have CUDA loaded if you have that turned on in your configuration options.

```console
$ cd vmd-1.9.3
$ ./configure
$ cd src
$ sudo checkinstall
```
## Pymol
Note &ndash; the open source version is not the same as the student version! By all accounts (i.e. Katie said so), the open source version is far superior to the neutered EduPymol that Schrodinger now releases. [Get the open source version here.](https://github.com/schrodinger/pymol-open-source)

```console
$ git clone https://github.com/schrodinger/pymol-open-source
$ cd pymol-open-source
$ python setup.py install --prefix=/store/opt/packages/pymol/pymol-open-source
```

Make sure to update your path.

```console
$ echo 'PATH=/store/packages/pymol/pymol-open-source/bin:$PATH' >> ~/.bashrc
```