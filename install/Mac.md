# Mac Installation

- [Mac Installation](#mac-installation)
- [1. Introduction](#1-introduction)
- [2. Installing Prerequisite Software and Tools](#2-installing-prerequisite-software-and-tools)
  - [2-1. Install Homebrew](#1-install-homebrew)
  - [2-2. Install Miniconda](#2-2-install-miniconda)
- [3. Building and Installing HOOMD-blue V3.0.0](#3-building-and-installing-hoomd-blue-v300)
  - [3-1. Create Conda Environment](#3-1-create-conda-environment)
- [4. Installing Tools for Data Processing](#4-installing-tools-for-data-processing)

## 1. Introduction

This guide will walk you through the installation process for HOOMD-blue(<http://glotzerlab.engin.umich.edu/hoomd-blue/>) molecular dynamics simulation package and some commonly used data processing tools on macOS system. Prior experience with UNIX CLI would be helpful for you to understand this guide better, but is not required. The majority of the installation will be done through your Terminal.app or it can be done through whichever terminal emulator you prefer.

Note that the most current software does not always mean they are the most stable versions. Thus, I will be specifying the software versions I am installing. As a reference, the following installation guide was carried out on
>macOS Big Sur, Version 11.6.5

Also, the following installation guide assumes your machine has x86_64 (AMD64) architecture instead of ARM, which is the case for recent MacBooks with M label chips. Given Apple's Rosetta 2, M label chips should not prohobit you from following this guide, but know that there is likely to be a drop in the performance of your HOOMD simulations ran locally. Making sure you are following this guide but switch all the x86_64 specific instructions to corresponding ones for ARM may prevent this issue.

## 2. Installing Prerequisite Software and Tools

### 2-1. Install Homebrew

Homebrew (<https://brew.sh/>)is a package manager that will help you to install and manage the required software packages easily. If you're familiar with Linux systems, think of Homebrew as apt-get on Debian-based systems or dnf/yum on RedHat-based systems.

Go to <https://brew.sh> and run the installation command shown in Terminal.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This will install Homebrew package manager as well as the Command Line Tools that are used to carry out the following guide.

### 2-2. Install Miniconda

Go to <https://docs.conda.io/en/latest/miniconda.html> to download and install Miniconda for Python 3.9 or run the code shown below to install Miniconda 4.11.0. I have confirmed that updating to version 4.12.0 works without any problem.

```bash
curl -sO https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-MacOSX-x86_64.sh
/bin/bash ./Miniconda*.sh -b -p $HOME/miniconda3
$HOME/miniconda3/bin/conda init bash zsh
source $HOME/.bash_profile $HOME/.zshrc
rm $HOME/Miniconda*.sh
```

## 3. Installing HOOMD-blue V3.0.0

### 3-1 Create Conda Environment



## 4. Installing Tools for Data Processing
