# Mac Installation

- [1. Introduction](#1-introduction)
- [2. Installing Prerequisite Software and Tools](#2-installing-prerequisite-software-and-tools)
  - [2-1. Install Homebrew](#1-install-homebrew)
  - [2-2. Install Miniconda](#2-2-install-miniconda)
- [3. Building and Installing HOOMD-blue V3.0.0](#3-building-and-installing-hoomd-blue-v300)
  - [3-1. Create Conda Environment](#3-1-create-conda-environment)
- [4. Installing Tools for Data Processing](#4-installing-tools-for-data-processing)
- [5. Troubleshooting](#5-troubleshooting)

## 1. Introduction

This guide will walk you through the installation process for HOOMD-blue(<http://glotzerlab.engin.umich.edu/hoomd-blue/>) molecular dynamics simulation package and some commonly used data processing tools on macOS system. Prior experience with UNIX CLI would be helpful for you to understand this guide better, but is not required. The majority of the installation will be done through your Terminal.app or it can be done through whichever terminal emulator you prefer.

Note that the most current software does not always mean they are the most stable versions. As a reference, the following installation guide was carried out on
>macOS Big Sur, Version 11.6.5

Also, the following installation guide assumes your machine has x86_64 (AMD64) architecture instead of the ARM M label chips used for recent Macs. Given Apple's Rosetta 2, M label chips should not prohobit you from following this guide, but know that there is likely to be a drop in the performance of your HOOMD simulations ran locally. Making sure you are following this guide but switch all the x86_64 specific instructions to corresponding ones for ARM may prevent this issue depending on the software support for ARM architecture.

## 2. Installing Prerequisite Software and Tools

### 2-1. Install Homebrew and git

Homebrew (<https://brew.sh/>)is a package manager that will help you to install and manage the required software packages easily. If you're familiar with Linux systems, think of Homebrew as apt-get on Debian-based systems or dnf/yum on RedHat-based systems.

Go to <https://brew.sh> and run the installation command shown in Terminal.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This will install Homebrew package manager as well as the Command Line Tools that are used to carry out the following guide.

Git will be installed with the following commands.

```bash
brew install git
```

Note that macOS's default shell has changed to zsh from bash. There should not be much significant differences but if you do prefer to use bash, install bash via homebrew `brew install bash` and then changing the default shell with `chsh -s /usr/local/bin/bash`.

### 2-2. Install Miniconda

Go to <https://docs.conda.io/en/latest/miniconda.html> to download and install Miniconda for Python 3.9 or run the code shown below to install Miniconda 4.11.0. I have confirmed that updating to version 4.12.0 works without any problem.

```bash
curl -sO https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-MacOSX-x86_64.sh
/bin/bash ./Miniconda*.sh -b -p $HOME/miniconda3
$HOME/miniconda3/bin/conda init bash zsh
source $HOME/.bash_profile $HOME/.zshrc
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

```bash
conda install -c conda-forge "hoomd=3.0.0=*cpu*"
```

Note the quotation marks as zsh may interpret the asterisk for something else and not pass it to conda.

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
conda install --strict-channel-priority -c https://conda.ovito.org -c conda-forge ovito freud gsd shapely matplotlib
```

If you see an output regarding

>failed with initial frozen solve,

don't worry about it. It is most likely will resolve itself. After this, you should have commonly used post-processing tools installed within your post_proc environment.

From this point and on, unless there's a serious issue like the version of the software you're currently using is bugged or deprecated, I would recommend you NOT to try to upgrade anything. As the popular saying goes

>If it ain't broke, don't fix it.

However, should you need to update your software packages, priortize OVITO and their software channel by specifying the channel priority.

```bash
conda upgrade --strict-channel-priority -c https://conda.ovito.org -c conda-forge *package_name*
```

## 5. Troubleshooting

To be added.
