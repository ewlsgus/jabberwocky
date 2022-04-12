# Longleaf Installation

- [1. Introduction](#1-introduction)
- [2. Getting Prerequisite Software and Tools](#2-getting-prerequisite-software-and-tools)
- [2-1. Load Modules](#2-1-load-modules)
- [2-2. Install Miniconda](#2-2-install-miniconda)
- [3. Installing HOOMD-blue V3.0.0](#3-installing-hoomd-blue-v300)
- [3-1 Create Conda Environment](#3-1-create-conda-environment)
- [3-2 Install HOOMD-blue Molecular Dynamics Simulation Package](#3-2-install-hoomd-blue-molecular-dynamics-simulation-package)
- [4. Installing Tools for Data Processing](#4-installing-tools-for-data-processing)
- [5. Troubleshooting](#5-troubleshooting)

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

## 3. Installing HOOMD-blue V3.0.0

### 3-1 Create Conda Environment

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

### 3-2 Install HOOMD-blue Molecular Dynamics Simulation Package

```bash
conda install -c conda-forge "hoomd=3.0.0=*cpu*"
```

Note the quotation marks as zsh may interpret the asterisk for something else and not pass it to conda.

If you see an output regarding

>failed with initial frozen solve,

don't worry about it. It is most likely will resolve itself.

As far as I know, there is no support from HOOMD-blue for AMD GPUs on Mac (more specifically no support for ROCm for Mac). Thus, you shouldn't need any GPU related builds on your Mac. I doubt this will ever be the case, but if you do have an eGPU, just search for instructions on CUDA setup on Macs and then install the GPU build instead of CPU.

This sets up the conda environment with HOOMD-blue simulation package on your machine. Confirm that the simulation package is working by starting up a python session with `python`.
Then, use the following codes to confirm.

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
