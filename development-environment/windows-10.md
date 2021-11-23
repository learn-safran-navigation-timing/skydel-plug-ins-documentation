---
description: >-
  This section contains instructions on how to setup your development
  environment in Windows 10 in order to compile the Skydel Plug-ins examples.
---

# Windows 10

## Prerequisites

### Visual Studio Build Tools 2017

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

### Qt Open Source 5.12.3

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

### Git 2.29.1

Download the latest installer [here](https://gitforwindows.org) and install Git.

{% hint style="info" %}
Use default configuration settings during installation.
{% endhint %}

## Compilation

### **Getting the Source Code**

The GitHub repository contains the Skydel Plug-ins SDK and some examples:

```
git clone https://github.com/learn-orolia/skydel-plug-ins
```

**Updating the Souce Code**

```
cd skydel-plug-ins
git pull
```

**Getting a Specific Version of the Source Code**

{% hint style="info" %}
Checkout this [page](https://github.com/learn-orolia/skydel-plug-ins/releases) for supported versions
{% endhint %}

```
cd skydel-plug-ins
git checkout VERSION
```

### **Compiling the Source Code**

Open Qt Creator, go to _File / Open File or Project..._ and select the project file located in _skydel-plug-ins / skydel\_plugin.pro_. Make sure the selected kit is _Desktop Qt 5.12.3 MSVC2017 64bit_ and select _Configure Project:_

![](../.gitbook/assets/win\_compile\_1.png)

Go to Projects, change _Edit build configuration_ for _Release_ and disable _Shadow build:_

![](../.gitbook/assets/win\_compile\_2.png)

Go to _Edit_, right click on the root folder and select _Rebuild_:

![](../.gitbook/assets/win\_compile\_3.png)

Build output can be found in _skydel-plugin-ins / bin_ under the form of a dynamic-link library (e.g. simple\_plugin.lib)

![](../.gitbook/assets/win\_compile\_4.png)

To make build output available in Skydel, move the _.dll_ file to _Skydel Data Folder / Plug-ins:_

![](../.gitbook/assets/win\_compile\_5.png)
