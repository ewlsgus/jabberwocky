# WSL Set-Up Guide

- [1. Introduction](#1-introduction)
- [2. Setting up Windows 10/11 for WSL2 Installation](#2-setting-up-windows-1011-for-wsl2-installation)
- [3. Installing WSL Linux Distribution](#3-installing-wsl-linux-distribution)
  - [3-1. Install NVIDIA GPU Drivers for WSL Support](#3-1-install-nvidia-gpu-drivers-for-wsl-support) 

## 1. Introduction

HOOMD-blue molecular dynamics simulation package is unfortunately not built to be installed on Windows. You won't find it on the conda repository nor will you be able to build/run the simulation package with Windows alone. **However**, this doesn't mean you can't use Windows machines to run your local simulations. It may not be the best option, but with Windows Subsystem for Linux (WSL), you can still proceed with HOOMD-blue installation. In the following sections, you'll be able to learn how to set-up WSL to be used on your machine. The majority of this guide is borrowed from or builds on top of Microsoft's WSL Installation Guide found here: <https://docs.microsoft.com/en-us/windows/wsl/install>.

## 2. Setting up Windows 10/11 for WSL2 Installation

Before we begin, you must check the Windows version. Press <kbd>⊞ Win</kbd> + <kbd>r</kbd> and run `winver` to view your current OS version.

>- For x64 systems: Version 1903 or later, with Build 18362 or later.
>- For ARM64 systems: Version 2004 or later, with Build 19041 or later.

To find out which architecture your machine is, use `systeminfo` on PowerShell. You'll be able to find out whether your system is x64 or ARM64.

If your machine does not meet the version requirement, update your Windows:
>open "Settings" from your Windows menu, navigate to "Update & Security" and select "Check for Updates".

If you're on a supported, version, open PowerShell "as Administrator," and type the following commands:

```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
restart-computer
```

**NOTE** that the above command includes a restart command. Make sure to save all your work and have your machine ready for a reboot.

After the reboot, install the WSL kernel:

- for x64 systems: <https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>
- for AMD64 systems: <https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi>

If your machine has a supported NVIDIA GPU (anything at or after GeForce 10 series and Quadro P series), proceed to section [3-1](#3-1-install-nvidia-gpu-drivers-for-wsl-support). Otherwise, skip to section [3-2](#3-2-install-ubuntu).

## 3. Installing WSL Linux Distribution

Before the installation, ensure your WSL install is updated by using `wsl --update` on PowerShell and/or checking for Windows system update. In addition, ensure that WSL 2 is used by default instead of WSL 1 by using `wsl --set-default-version 2` on PowerShell. The difference between the two versions is not within the scope of this guide and if you have a reason to use WSL 1 instead of 2, you're more than likely to know enough to not need this guide.

If your machine is equipped with a supported NVIDIA GPU (GeForce 10 series or after / Quadro P series or after), proceed to section [3-1](#3-1-install-nvidia-gpu-drivers-for-wsl-support). Otherwise, skip to section [3-2](#3-2-install-ubuntu).

### 3-1. Install NVIDIA GPU Drivers for WSL Support

The majority of this guide is borrowed from or builds on top of NVIDIA's Installation Guide found here: <https://docs.nvidia.com/cuda/wsl-user-guide/index.html>.

**I have not enough experience with ***ARM64*** systems to be able to help you with GPU set up on WSL. So, if you have an ARM64 system with an NVIDIA GPU, please search for the process elsewhere or skip the GPU sections.**

To find out which graphics card you have, use `wmic path win32_VideoController get name` on PowerShell.

Once you know which graphics card you have, proceed to <https://www.nvidia.com/Download/index.aspx?lang=en-us> to find and download the correct driver for your graphics card. Once downloaded, install the driver and proceed to the next section.

### 3-2. Install Ubuntu

In this guide, we will install Ubuntu as it is arguably the most popular distro and thus have much larger database of troubleshooting errors online. However, if you have a specific distro that you prefer, you can check the list with `wsl --list --online` on PowerShell.

Otherwise, you can install Ubuntu with `wsl --install -d Ubuntu` on PowerShell.
Once installed, a command prompt will pop up, finalizing the installation. After the installation, you will be asked to enter a username and a password. This will be your user account within your WSL installation (completely unrelated to your Windows account and login), so pick something you can remember and don't worry too much about it. You're unlikely to need it too often.

After setting up the user account, you will be able to start using the Ubuntu shell. Update the distro with `sudo apt update` followed by `sudo apt upgrade`. You will be prompted for the password you set in the previous step.

For the machines equipped with NVIDIA graphics cards, you need to do a few extra steps for you to be able to utilize your GPU. If your machine is not equipped with a GPU, proceed to section [4](#4-installing-hoomd-blue-in-wsl).

For **x64** systems, use the following commands to get CUDA on WSL:

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/ /"
sudo apt-get update
sudo apt-get -y install cuda
```

Once the installation is done, add the installation paths to the system path:

```bash
echo 'export PATH=/usr/local/cuda-11.7/bin${PATH:+:${PATH}}' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.7/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc
source ~/.bashrc
```

Check your installation with `nvcc --version` and `nvidia-smi`. If the command runs without any error, your WSL system is now ready to use your graphics card.

**Note**: as of 2022-06-08, CUDA 11.7 is the most current version, but this might change and the commands above may need to be modified. Check your cuda version with `ls /usr/local | grep cuda` and modify the version number in the commands above. Otherwise, refer <https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#environment-setup>.

## 4. Installing HOOMD-blue in  WSL

Proceed with the following guide for installing HOOMD-blue on Linux: [Linux Installation Guide](./install/Linux.md)
