---
description: Setup windows to do C programming
---

# Windows, WSL

## How to use gcc, gdb on windows?

Mainly there are three methods.

1. Using MinGW, Cygwin like distributions on windows
2. Using WSL (Windows Subsystem for Linux) - _recommended_
3. Using Ubuntu Virtual Machine on Windows&#x20;

### 01: Using MinGW

MinGW is a collection of Linux utilities (executables) such as ls, mkdir, gcc, gdb, etc. for windows.&#x20;

* Download MinGW via [this link](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe/download)&#x20;
* Set the path environment variable on windows to point to \<MinGW Installation Directory>/bin&#x20;
* Open a command prompt on windows
* Type gcc, gdb, etc. :) the commands should be recognized now

_You may refer to Guides | VS Code with MinGW Guide in Blackboard for more details on the installation._

### 02: Using WSL

We are working on preparing some guides (WSL, Virtual Machine, MinGW on Windows) for windows users. For now, in short, try looking for&#x20;

* Installing WSL2 (Windows Subsystem for Linux) - compatibility layer for running Linux binary executables natively on Windows 10
* Once installed you can try all the Linux commands as you would in a Lab PC.

{% embed url="https://docs.microsoft.com/en-us/windows/wsl/install-win10#manual-installation-steps" %}

### 03: Using Ubuntu Virtual Machine

We are working on preparing some guides (WSL, Virtual Machine, MinGW on Windows) for windows users. For now, in short, try looking for&#x20;

* Download VMware player
* Download Ubuntu 18.04.5 LTS or the latest version, configure a virtual machine using the VMWare player
* Once set up, you can try Linux commands as you would in a Lab PC

{% embed url="https://releases.ubuntu.com/18.04.5/" %}

