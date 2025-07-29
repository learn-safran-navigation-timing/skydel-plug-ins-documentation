---
description: >-
  This section contains instructions on how to setup your development
  environment in Windows 10 & 11 in order to compile the Skydel Plug-ins
  examples.
---

# Windows 10 & 11

## MSVC

Download latest Build Tools for Visual Studio 2019 [online installer](https://learn.microsoft.com/en-us/visualstudio/releases/2019/history#release-dates-and-build-numbers) and install C++ build tools.

<img src="../.gitbook/assets/file.excalidraw (1) (1).svg" alt="" class="gitbook-drawing">

<details>

<summary>Version should at least be 19.29.30158 for <code>cl</code></summary>

Please note that the version in the execution path of cl might be different on newer versions.

```
C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Tools\MSVC\14.29.30133\bin\Hostx64\x64\cl.exe
> Microsoft (R) C/C++ Optimizing Compiler Version 19.29.30158 for x64
```

</details>

## CMake

Download and install [CMake](https://github.com/Kitware/CMake/releases/download/v4.0.3/cmake-4.0.3-windows-x86_64.msi). Make sure to add CMake to the system PATH during installation.

<details>

<summary>Version should be 4.0.3 for <code>cmake</code></summary>

```
cmake --version
> cmake version 4.0.3
```

</details>

## Ninja

* Download and unzip [ninja-win.zip](https://github.com/ninja-build/ninja/releases/download/v1.12.1/ninja-win.zip) to _Program Files / ninja-win_
* From the search bar, search for _Edit the system environment variables_
* Select _Envrionment Variables..._
* Select _Path_ in _System variables_ and select _Edit..._
* Select _Browse..._ and find the folder _Program Files / ninja-win_ (where you unziped _ninja-win.zip_)
* After those steps, ninja should be accessible from anywhere

<details>

<summary>Version should be 1.12.1 for <code>ninja</code></summary>

```
ninja --version
> 1.12.1
```

</details>

## Git

Download and install latest [git](https://gitforwindows.org).

<details>

<summary>Version should at least be 2.40.1 for <code>git</code></summary>

```
git --version
> git version 2.40.1.windows.1
```

</details>

## Perl

Download and install latest [Perl](https://github.com/StrawberryPerl/Perl-Dist-Strawberry/releases/download/SP_54021_64bit_UCRT/strawberry-perl-5.40.2.1-64bit.msi), needed for Qt compilation.

<details>

<summary>Version should at least be 5.40.2.1 for <code>perl</code></summary>

```
perl -v
> This is perl 5, version 40, subversion 2 (v5.40.2) built for MSWin32-x64-multi-thread
```

</details>

## QtCreator

Follow the instructions below to setup the QtCreator IDE:

1. Download [qtcreator-windows-x64-msvc-10.0.1.7z](https://github.com/qt-creator/qt-creator/releases/download/v10.0.1/qtcreator-windows-x64-msvc-10.0.1.7z) and unzip to **C:\Qt\QtCreator**
2. Download [qtcreatorcdbext-windows-x64-msvc-10.0.1.7z](https://github.com/qt-creator/qt-creator/releases/download/v10.0.1/qtcreatorcdbext-windows-x64-msvc-10.0.1.7z) and unzip to **C:\Qt\QtCreator** such that you have **C:\Qt\QtCreator\lib\qtcreatorcdbext64**
3. Optional: To make accessing Qt Creator easier, navigate to **C:\Qt\QtCreator\bin** and launch **qtcreator.exe**. It may take some time to open the first time. Once it's open, you can pin the application to the taskbar for quicker access in the future.

## OpenSSL

Since OpenSSL is required for Qt compilation, follow the instructions below to compile and install it:

{% code title="From an elevated Command Prompt" %}
```
powershell -Command "Invoke-WebRequest -Uri 'https://github.com/openssl/openssl/releases/download/OpenSSL_1_1_1w/openssl-1.1.1w.tar.gz' -OutFile 'C:\'"
cd /d "C:\"
tar -xzf "openssl-1.1.1w.tar.gz"
del "openssl-1.1.1w.tar.gz"
cd openssl-1.1.1w
call "C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
perl Configure VC-WIN64A
nmake
```
{% endcode %}

## Qt

Once you've completed all the steps in the sections above, compile and install Qt using the following command:

{% code title="From an elevated Command Prompt" %}
```
cd /d "C:\"
git clone --branch v5.15.17-lts-lgpl --single-branch https://github.com/qt/qt5
cd qt5
perl init-repository --module-subset="qtbase,qtsvg,qtserialport"
call "C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
set CL=/MP
configure -prefix C:\Qt\5.15.17\msvc2019_64 -opensource -nomake tests -nomake examples -openssl-linked -I C:\openssl-1.1.1w\include -L C:\openssl-1.1.1w OPENSSL_LIBS="-lWs2_32 -lGdi32 -lAdvapi32 -lCrypt32 -lUser32 -llibssl_static -llibcrypto_static"
nmake
nmake install
```
{% endcode %}

<details>

<summary>Version should be 5.15.17 for <code>Qt</code></summary>

```
C:\Qt\5.15.17\msvc2019_64\bin\qmake --version
> QMake version 3.1
> Using Qt version 5.15.17 in C:/Qt/5.15.17/msvc2019_64/lib
```

</details>
