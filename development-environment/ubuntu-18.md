---
description: >-
  This section contains instructions on how to setup your development
  environment in Ubuntu 18.04 in order to compile the Skydel Plug-ins examples.
---

# Ubuntu 18.04

## System Dependencies

```
sudo apt-get update
sudo apt install build-essential libgl1-mesa-dev libxcb-xinerama0 liblapack-dev
```

## GCC 7.5

The GCC compiler is already installed; verify that the version is _7.5:_

```
gcc -v
```

## Qt Open Source 5.12.3

#### Installation

Update the email (_QT\_EMAIL_) and password (_QT\_PW_) accordingly, then launch the installation:

```
wget https://download.qt.io/official_releases/online_installers/qt-unified-linux-x64-online.run -O /tmp/qt-installer.run

chmod +x /tmp/qt-installer.run

sudo /tmp/qt-installer.run install qt.qt5.5123.gcc_64 qt.tools.cmake qt.tools.qtcreator \
    --root /opt/Qt \
    --auto-answer telemetry-question=No --accept-licenses --default-answer --accept-obligations --confirm-command \
    --email QT_EMAIL \
    --pw QT_PW

rm /tmp/qt-installer.run
```

{% hint style="warning" %}
If Qt Creator refuses to open, enable debug traces to help find the source of the problem:`export QT_DEBUG_PLUGINS=1 && /opt/Qt/Tools/QtCreator/bin/qtcreator`
{% endhint %}

#### Configuration

Open Qt Creator and navigate to _Tools / Options... / Kits_ and select _Desktop Qt 5.12.3 GCC 64bit (default):_

![](../.gitbook/assets/ub\_config\_qt\_1.png)

Make sure to match the following:

* Compiler
  * C: _GCC(C, x86 64bit in /usr/bin)_
  * C++: _GCC(C++, x86 64bit in /usr/bin)_
* Qt version : _Qt 5.12.3 GCC 64bit_

## Git 2.17.1

```
sudo apt install git
```

## Blaze 3.7 (CMake Users Only)

CMake users need to install Blaze 3.7 from the official repository in their search path:

```
git clone https://github.com/parsa/blaze
cd blaze && mkdir build && cd build
cmake -DBLAZE_BLAS_MODE=ON .. && sudo make install
```
