---
description: >-
  This section contains instructions on how to setup your development
  environment in Windows 10 in order to compile the Skydel Plug-ins examples.
---

# Windows 10

## Visual Studio Build Tools 2017

Go to the _Visual Studio 2017 and other Products_ download page with the link [here](https://visualstudio.microsoft.com/vs/older-downloads/):

![](../.gitbook/assets/install\_vs\_1.png)

Download _Build Tools for Visual Studio 2017 (version 15.9)_:

![](../.gitbook/assets/install\_vs\_2.png)

Open the installer and select _Continue_:

![](../.gitbook/assets/install\_vs\_3.png)

Wait for the setup to end:

![](../.gitbook/assets/install\_vs\_4.png)

In _Workloads_ select _Visual C++ build tools_ then _Install:_

![](../.gitbook/assets/install\_vs\_5.png)

Wait for installation to end (it may take a while):

![](../.gitbook/assets/install\_vs\_6.png)

## Qt Open Source 5.12.3

#### **Installation**

Download the latest Windows online installer named _qt-unified-windows-x86-online.exe_ [here](https://download.qt.io/official\_releases/online\_installers/).

Update installer name, email (_QT\_EMAIL_) and password (_QT\_PW_) accordingly, then launch the installation:

```aspnet
.\qt-unified-windows-x86-4.1.1-online.exe install qt.qt5.5123.win64_msvc2017_64 qt.tools.cmake.win64 qt.tools.qtcreator `
    --root C:\Qt `
    --auto-answer telemetry-question=No --accept-licenses --default-answer --accept-obligations --confirm-command `
    --email QT_EMAIL `
    --pw QT_PW
```

To validate installation, open Qt Creator, navigate to _Tools / Options... / Kits_ and select _Desktop Qt 5.12.3 MSVC2017 64bit (default)_, and make sure it looks like the screenshot below:

![](../.gitbook/assets/win\_config\_qt\_1.png)

{% hint style="info" %}
The C and C++ compiler version might be slightly different.
{% endhint %}

## Git 2.29.1

Download the latest installer [here](https://gitforwindows.org) and install Git.

{% hint style="info" %}
Use default configuration settings during installation.
{% endhint %}

## CMake

Download the latest installer [here](https://cmake.org/download/) and install CMake.

## Blaze 3.7 (CMake Users Only)

CMake users need to install Blaze 3.7 from the official repository in their search path:

```
git clone https://github.com/parsa/blaze 
cd blaze mkdir build && cd build 
cmake -G"NMake Makefiles" -DCMAKE_BUILD_TYPE=Release -DBLAZE_BLAS_MODE=OFF -DUSE_LAPACK=OFF -DBLAZE_CACHE_SIZE_AUTO=OFF .. 
cmake --install .
```

{% hint style="info" %}
By default _blas_ and _lapack_ libraries are used, but feel free to check all of Blaze options from their repository documentation.
{% endhint %}
