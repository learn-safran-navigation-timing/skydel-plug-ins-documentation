---
description: >-
  Here's how you need setup your development environment on Ubuntu 18.04 in
  order to compile the Skydel Plug-ins examples. It's also useful for user who
  wants to develop their own Plug-in.
---

# Ubuntu 18.04

## Prerequisites

### System Dependencies

```text
sudo apt-get update
sudo apt install build-essential libgl1-mesa-dev libxcb-xinerama0
```

### GCC 7.5

The GCC compiler is already installed, make sure the version is _7.5:_

```text
gcc -v
```

### Qt Open Source 5.12.3

#### Installation

Download the latest online installer: [https://www.qt.io/download-open-source](https://www.qt.io/download-open-source)

{% hint style="info" %}
The installer name may differ depending of the version name
{% endhint %}

Make the installer an executable and launch it:

```text
chmod +x ./qt-unified-linux-x64-3.2.3-online.run
./qt-unified-linux-x64-3.2.3-online.run
```

![](../.gitbook/assets/install_qt_1.png)

![](../.gitbook/assets/install_qt_2.png)

![](../.gitbook/assets/install_qt_3.png)

![](../.gitbook/assets/install_qt_4.png)

![](../.gitbook/assets/install_qt_5.png)

Change the installation folder to _/opt/Qt:_

![](../.gitbook/assets/install_qt_6.png)

Select _Desktop gcc 64-bit_ under _Qt 5.12.3_ and make sure no option is selected in _Developer and Designer Tools_:

![](../.gitbook/assets/install_qt_7.png)

![](../.gitbook/assets/install_qt_8.png)

![](../.gitbook/assets/install_qt_9.png)

Wait to the installation to end, it may take a while:

![](../.gitbook/assets/install_qt_10.png)

![](../.gitbook/assets/install_qt_11.png)

{% hint style="warning" %}
If Qt Creator refuses to open, enable debug traces to help find the source of the problem:`export QT_DEBUG_PLUGINS=1 && /opt/Qt/Tools/QtCreator/bin/qtcreator`
{% endhint %}

#### Configuration

Open Qt Creator, go to _Tools / Options... / Kits_ and select _Desktop Qt 5.12.3 GCC 64bit \(default\):_

![](../.gitbook/assets/config_qt_1.png)

Make sure to match the following:

* Compiler 
  * C: _GCC\(C, x86 64bit in /usr/bin\)_
  * C++: _GCC\(C++, x86 64bit in /usr/bin\)_
* Qt version : _Qt 5.12.3 GCC 64bit_

### Git 2.17.1

```text
sudo apt install git
```

## Compilation

### Getting the Source Code

The GitHub repository contains the Skydel Plug-ins SDK and some examples:

```text
git clone https://github.com/learn-orolia/skydel-plug-ins
```

#### Updating the Souce Code

```text
git pull
```

#### Getting a Specific Version of the Source Code

{% hint style="info" %}
Checkout this [page](https://github.com/learn-orolia/skydel-plug-ins/releases) for supported versions
{% endhint %}

```text
git checkout VERSION
```

### Compiling the Source Code

Open Qt Creator, go to _File / Open File or Project..._ and select the project file located in _skydel-plug-ins / skydel\_plugin.pro_. Make sure the selected kit is _Desktop Qt 5.12.3 QCC 64bit_ and select _Configure Project:_

![](../.gitbook/assets/compile_ubuntu_1.png)

Go to Projects, change _Edit build configuration_ for _Release_ and disable _Shadow build:_

![](../.gitbook/assets/compile_ubuntu_2.png)

Go to Edit, right click on the root folder and select _Rebuild:_

![](../.gitbook/assets/compile_ubuntu_3.png)

Build output can be found in _skydel-plugin-ins / bin_ under the form of a shared object \(e.g. libsimple\_plugin.so\)

