---
description: >-
  This section contains instructions on how to setup your development
  environment in Ubuntu 22.04 in order to compile the Skydel Plug-ins examples.
---

# Ubuntu 22.04

## Updates & Packages

```
sudo apt update
sudo apt dist-upgrade
sudo apt install build-essential libgl1-mesa-dev libxcb-xinerama0 uuid-dev git libssl-dev ninja-build
```

<details>

<summary>Packages detailed information</summary>

* build-essential -> GCC and G++ 11.2.0
* libgl1-mesa-dev -> OpenGL
* libxcb-xinerama0 -> Qt installer
* uuid-dev -> Skydel remote API
* git -> Source code
* libssl-dev -> CMake compilation
* ninja-build -> Compilation

</details>

## GCC/G++

<details>

<summary>Version should be 11.2.0 for <code>gcc</code> and <code>g++</code></summary>

```
gcc --version
> gcc (Ubuntu 11.2.0-19ubuntu1) 11.2.0

g++ --version
> g++ (Ubuntu 11.2.0-19ubuntu1) 11.2.0
```

</details>

## Ninja

<details>

<summary>Version should be 1.10.1 for <code>ninja</code></summary>

```
ninja --version
> 1.10.1
```

</details>

## Qt

```
wget https://download.qt.io/official_releases/online_installers/qt-unified-linux-x64-online.run -O /tmp/qt-installer.run

chmod +x /tmp/qt-installer.run

sudo /tmp/qt-installer.run install qt.qt5.5152.gcc_64 qt.tools.qtcreator \
    --root /opt/Qt \
    --auto-answer telemetry-question=No --accept-licenses --default-answer --accept-obligations --confirm-command \
    --email QT_EMAIL \
    --pw QT_PW

rm /tmp/qt-installer.run
```

{% hint style="info" %}
Update the email (_QT\_EMAIL_) and password (_QT\_PW_) accordingly
{% endhint %}

## CMake

```
wget https://github.com/Kitware/CMake/releases/download/v3.22.1/cmake-3.22.1.tar.gz
tar -xzvf cmake-3.22.1.tar.gz
cd cmake-3.22.1
./bootstrap
make -j4
sudo make install 
```
