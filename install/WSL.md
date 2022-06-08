# WSL Set-Up Guide

-[1. Introduction](#1-introduction)
-[2. Setting up Windows 10/11 for WSL2 Installation](#2-setting-up-windows-1011-for-wsl2-installation)
-[3. Installing WSL Linux Distribution - Ubuntu](#3-installing-wsl-linux-distribution---ubuntu)

## 1. Introduction

HOOMD-blue molecular dynamics simulation package is unfortunately not built to be installed on Windows. You won't find it on the conda repository nor will you be able to build/run the simulation package with Windows alone. **However**, this doesn't mean you can't use Windows machines to run your local simulations. It may not be the best option, but with Windows Subsystem for Linux (WSL), you can still proceed with HOOMD-blue installation. In the following sections, you'll be able to learn how to set-up WSL to be used on your machine. The majority of this guide is borrowed from or builds on top of Microsoft's WSL Installation Guide found here: <https://docs.microsoft.com/en-us/windows/wsl/install>.

## 2. Setting up Windows 10/11 for WSL2 Installation

Before we begin, you must check the Windows version. Press <kbd>⊞ Win</kbd> + <kbd>r</kbd> and run `winver` to view your current OS version.

>- For x64 systems: Version 1903 or later, with Build 18362 or later.
>- For ARM64 systems: Version 2004 or later, with Build 19041 or later.

**Note** 

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

### 2-1. Install NVIDIA GPU Drivers for WSL Support

## 3. Installing WSL Linux Distribution - Ubuntu

## 4. Installing HOOMD-blue in  WSL

Proceed with the following guide for installing HOOMD-blue on Linux: [Linux Installation Guide](./install/Linux.md)
