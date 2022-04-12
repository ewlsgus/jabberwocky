# Longleaf Installation Guide

-[1. Introduction](#1-introduction)
-[2. Getting Prerequisite Software and Tools](#2-getting-prerequisite-software-and-tools)
  -[2-1. Load Modules](#2-1-load-modules)
  -[2-2. Install Miniconda](#2-2-install-miniconda)
-[3. Installing HOOMD-blue V3.0.0](#3-installing-hoomd-blue-v300)
  -[3-1 Create Conda Environment and Install Prerequisite Software](#3-1-create-conda-environment-and-install-prerequisite-software)
  -[3-2 Build and Install HOOMD-blue Molecular Dynamics Simulation Package V3.0.0](#3-2-build-and-install-hoomd-blue-molecular-dynamics-simulation-package-v300)
-[4. Installing Tools for Data Processing](#4-installing-tools-for-data-processing)
-[5. Troubleshooting](#5-troubleshooting)

## 1. Introduction

This guide will walk you through the installation process for HOOMD-blue(<http://glotzerlab.engin.umich.edu/hoomd-blue/>) molecular dynamics simulation package and some commonly used data processing tools on the UNC-CH's Longleaf Research Computing system. Prior experience with UNIX CLI would be helpful for you to understand this guide better, but is not required. This installation guide assumes you have established an account on the Longleaf Cluster (<https://its.unc.edu/research-computing/request-a-cluster-account/>). Should you choose or need to run any HOOMD-blue simulations on the Dogwood Cluster, this installation guide can be followed without any alterations. The following steps assume you have bash as your shell, which is the default setup for longleaf. If you chose to use other shells such as tcsh, change the commands appropriately. There may be some syntax differences that require you to modify the commands accordingly.

## 2. Getting Prerequisite Software and Tools

### 2-1. Load Modules

Modules are pre-built software that are on the cluster for you to easily use. Load and save the following modules

  1. git
  2. gcc/9.1.0
  3. cuda/11.4
  4. cmake
  5. geos

```bash
module load git gcc/9.1.0 cuda/11.4 cmake geos
module save
```

### 2-2. Install Miniconda

Type the following commands to download and install miniconda3 with Python version 3.9 and conda version 4.11.

```bash
curl -sO https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-Linux-x86_64.sh
bash ./Miniconda*.sh -b -p $HOME/miniconda3
$HOME/miniconda3/bin/conda init bash
source $HOME/.bashrc
rm $HOME/Miniconda*.sh
```

You can confirm the Miniconda installation by running `conda -V` in your terminal. The output should resemble the following:
>conda 4.11.0

I have confirmed that 4.12.0 works without any problem, so you can upgrade it to 4.12.0 with `conda install -n base -c defaults conda=4.12.0`.

## 3. Installing HOOMD-blue V3.0.0

### 3-1 Create Conda Environment and Install Prerequisite Software

Conda is capable of creating environments with specified versions of python and packages. You can switch between the environments by activating them, providing you with a customized, separate environment for each specific task. In this guide, we will separate our environments into one specificically for HOOMD-blue simulation package and another for post-processing of the simulation data. By default, the environments are contained within `$HOME/miniconda3/envs/`, but this guide will contain all the environments within `$HOME/pyEnvs/`. If desired, changing the environment directory should not affect workflow as long as the paths in your simulation scripts are changed accordingly.

First, we start with creating an environment for HOOMD-blue simulation package.

```bash
mkdir $HOME/pyEnvs
conda create --prefix $HOME/pyEnvs/hoomd -y python=3.8
```

Note that I am using python 3.8 here, but HOOMD-blue V3.0 requires any python above 3.6. Change this according to your needs.

Activate the created conda environment.

```bash
conda activate $HOME/pyEnvs/hoomd
```

Run `conda list` to ensure the specified python version is installed.

Install the following conda packages needed for building HOOMD-blue.

  1. eigen
  2. numpy
  3. pybind11
  4. tbb
  5. tbb-devel

```bash
conda install -c conda-forge eigen numpy pybind11 tbb tbb-devel
```

Confirm the installation by running `conda list` again.

### 3-2 Build and Install HOOMD-blue Molecular Dynamics Simulation Package V3.0.0

Start by downloading the tar for HOOMD-blue 3.0.0 and unarchiving.

```bash
curl -O https://glotzerlab.engin.umich.edu/Downloads/hoomd/hoomd-v3.0.0.tar.gz
tar -xvf ./hoomd-v3.0.0.tar.gz
```

Prepare the build directory and move to the directory.

```bash
mkdir -p ./hoomd-v3.0.0/build
cd ./hoomd-v3.0.0/build
```

Configure build settings with CUDA and TBB enabled.

```bash
CC=gcc CXX=g++ cmake ../ -DCMAKE_INSTALL_PREFIX=`python3 -c "import site; print(site.getsitepackages()[0])"` -DCMAKE_CXX_FLAGS=-march=native -DCMAKE_C_FLAGS=-march=native -DENABLE_GPU=ON -DENABLE_TBB=ON
```

Once the configuration has finished, build and install HOOMD-blue.

```bash
make all -j16 install
```

To confirm the installation, start a python session with `python` and run the following.

```python
import hoomd
integrator = hoomd.md.Integrator(dt=0.005)
integrator.dt
```

This should output the following.
>0.005

## 4. Installing Tools for Data Processing

We will create a separate conda environment to contain the post-processing tools. The tools taht will be installed in this step include

  1. OVITO (<https://www.ovito.org/docs/current/python/>),
  2. freud (<https://gsd.readthedocs.io/en/stable/index.html>),
  3. gsd (<https://gsd.readthedocs.io/en/stable/index.html>),
  4. shapely (<https://shapely.readthedocs.io/en/stable/manual.html>),
  5. matplotlib (<https://matplotlib.org/stable/index.html>).

Start by creating a conda environment and activating it.

```bash
conda create --prefix $HOME/pyEnvs/post_proc -y
conda activate $HOME/pyEnvs/post_proc
```

Note that a python version is not specified and the environment is created without any python pre-installed. We will let OVITO pull the dependency list and install the appropriate version of python.

Install the packages listed above.

```bash
conda install -y --strict-channel-priority -c https://conda.ovito.org -c conda-forge ovito freud gsd shapely matplotlib
```

Again, here you may get an output regarding

>failed with initial frozen solve,

don't worry about it as long as it resolves itself.

After this, you should have commonly used post-processing tools installed within your post_proc environment.

From this point and on, unless there's a serious issue like the version of the software you're currently using is bugged or deprecated, I would recommend you NOT to try to upgrade anything. As the popular saying goes

>If it ain't broke, don't fix it.

However, should you need to update your software packages, priortize OVITO and their software channel by specifying the channel priority.

```bash
conda upgrade --strict-channel-priority -c https://conda.ovito.org -c conda-forge *package_name*
```

## 5. Troubleshooting

To be added.
