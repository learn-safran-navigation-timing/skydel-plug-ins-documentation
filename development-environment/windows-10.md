---
description: >-
  Here's how you need setup your development environment on Windows 10 in order
  to compile the Skydel Plug-ins examples. It's also useful for user who wants
  to develop their own Plug-in.
---

# Windows 10

## Prerequisites

### Microsoft Visual Studio 2017 Community 15.9.28 for Windows Desktop

Download latest online installer: [https://visualstudio.microsoft.com/vs/older-downloads/](https://visualstudio.microsoft.com/vs/older-downloads/)

{% hint style="info" %}
Make sure you download _Visual Studio Community 2017_
{% endhint %}

Open the installer and select _Continue_

![](../.gitbook/assets/win_install_vs_1.png)

Wait to the setup to end:

![](../.gitbook/assets/win_install_vs_2.png)

In _Workloads_ select _Desktop development with C++_  then _Install:_

![](../.gitbook/assets/win_install_vs_3.png)

Wait to the installation to end, it may take a while:

![](../.gitbook/assets/win_install_vs_4.png)

Restart the computer:

![](../.gitbook/assets/win_install_vs_5.png)

After the restart, open the _Visual Studio Installer_ and make sure _Visual Studio Community 2017_ has version _15.9.28_ or greater:

![](../.gitbook/assets/win_install_vs_6.png)

### Qt Open Source 5.12.3

#### Installation

Download the latest online installer: [https://www.qt.io/download-open-source](https://www.qt.io/download-open-source)

Launch the installer:

![](../.gitbook/assets/win_install_qt_1.png)

![](../.gitbook/assets/win_install_qt_2.png)

![](../.gitbook/assets/win_install_qt_3.png)

![](../.gitbook/assets/win_install_qt_4.png)

![](../.gitbook/assets/win_install_qt_5.png)

![](../.gitbook/assets/win_install_qt_6.png)

Select _MSVC 2017 64-bit_ under _Qt 5.12.3_ and make sure no option is selected in _Developer and Designer Tools_:

![](../.gitbook/assets/win_install_qt_7.png)

![](../.gitbook/assets/win_install_qt_8.png)

![](../.gitbook/assets/win_install_qt_9.png)

![](../.gitbook/assets/win_install_qt_10.png)

Wait to the installation to end, it may take a while:

![](../.gitbook/assets/win_install_qt_11.png)

![](../.gitbook/assets/win_install_qt_12.png)

#### Configuration

Open Qt Creator, go to _Tools / Options... / Kits_ and select _Desktop Qt 5.12.3 MSVC2017 64bit \(default\):_

![](../.gitbook/assets/win_config_qt_1.png)

Make sure to match the following:

* Compiler
  * C++: _Microsoft Visual C++ Compiler 15.9.28307.1274_
* Qt version : _Qt 5.12.3 MSVC2017 64bit_

### Git 2.29.0

Download the latest installer [here](https://gitforwindows.org/) and install Git.

## Compilation

#### Getting the Source Code

The GitHub repository contains the Skydel Plug-ins SDK and some examples:

```text
git clone https://github.com/learn-orolia/skydel-plug-ins
```

**Updating the Souce Code**

```text
git pull
```

**Getting a Specific Version of the Source Code**

{% hint style="info" %}
Checkout this [page](https://github.com/learn-orolia/skydel-plug-ins/releases) for supported versions
{% endhint %}

```text
git checkout VERSION
```

#### Compiling the Source Code

Open Qt Creator, go to _File / Open File or Project..._ and select the project file located in _skydel-plug-ins / skydel\_plugin.pro_. Make sure the selected kit is _Desktop Qt 5.12.3 MSVC2017 64bit_ and select _Configure Project:_

![](../.gitbook/assets/win_compile_1.png)

Go to Projects, change _Edit build configuration_ for _Release_ and disable _Shadow build:_

![](../.gitbook/assets/win_compile_2.png)

Go to _Edit_, right click on the root folder and select _Rebuild_:

![](../.gitbook/assets/win_compile_3.png)

Build output can be found in _skydel-plugin-ins / bin_ under the form of a dynamic-link library \(e.g. simple\_plugin.lib\)

![](../.gitbook/assets/win_compile_4.png)

To make build output available in Skydel, move the _.dll_ file to _Skydel Data Folder / Plug-ins:_

![](../.gitbook/assets/win_compile_5.png)

