---
description: >-
  This section contains instructions on how to setup your development
  environment in Ubuntu 22.04 in order to compile the Skydel Plug-ins examples.
---

# Ubuntu 22.04

## Updates & Packages

```
sudo sed -i "s/# deb-src/deb-src/" /etc/apt/sources.list
sudo apt update
sudo apt dist-upgrade
sudo apt install build-essential libgl1-mesa-dev libxcb-xinerama0 uuid-dev git cmake ninja-build
```

<details>

<summary>Packages detailed information</summary>

* build-essential -> GCC and G++ 11.4.0
* libgl1-mesa-dev -> OpenGL
* libxcb-xinerama0 -> Qt installer
* uuid-dev -> Skydel remote API
* git -> Source code
* cmake -> Compilation
* ninja-build -> Compilation

</details>

## GCC/G++

<details>

<summary>Version should be 11.4.0 for <code>gcc</code> and <code>g++</code></summary>

```
gcc --version
> gcc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0

g++ --version
> g++ (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
```

</details>

## CMake

<details>

<summary>Version should be 3.22.1 for <code>cmake</code></summary>

```
cmake --version
> cmake version 3.22.1
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

Copy the following contents in a file named `qt-setup.sh` .

```
# Qt

readonly TMP=/tmp/qt-setup
mkdir --parents "$TMP"

readonly QT5_VERSION=5.15.17
readonly QT5_PREFIX=/opt/Qt/$QT5_VERSION/gcc_64
readonly QT5_TMP=$TMP/Qt5

sudo apt --yes build-dep qtbase5-dev

mkdir --parents "$QT5_TMP"
wget -qO- "https://download.qt.io/official_releases/qt/${QT5_VERSION%.*}/${QT5_VERSION}/submodules/qtbase-everywhere-opensource-src-${QT5_VERSION}.tar.xz" | tar -xJvC "$QT5_TMP"
cd "$QT5_TMP"/qtbase-everywhere-src*

sed -i "0,/#ifdef __cplusplus/!b;//a#  include <limits>" src/corelib/global/qglobal.h

sed -i "s|^\([[:space:]]*\)out\.d_func()->fileEngine->syncToDisk();|\1d->fileEngine->syncToDisk();|" src/corelib/io/qfile.cpp

./configure -prefix "$QT5_PREFIX" -xcb -opensource -confirm-license -nomake tests -nomake examples
make --jobs="$(nproc)"
sudo make install

wget -qO- "https://download.qt.io/official_releases/qt/${QT5_VERSION%.*}/${QT5_VERSION}/submodules/qtserialport-everywhere-opensource-src-${QT5_VERSION}.tar.xz" | tar -xJvC "$QT5_TMP"
cd "$QT5_TMP"/qtserialport-everywhere-src*
"$QT5_PREFIX"/bin/qmake
make --jobs="$(nproc)"
sudo make install

wget -qO- "https://download.qt.io/official_releases/qt/${QT5_VERSION%.*}/${QT5_VERSION}/submodules/qtsvg-everywhere-opensource-src-${QT5_VERSION}.tar.xz" | tar -xJvC "$QT5_TMP"
cd "$QT5_TMP"/qtsvg-everywhere-src*
"$QT5_PREFIX"/bin/qmake
make --jobs="$(nproc)"
sudo make install

# Qt Creator

readonly QTCREATOR_VERSION=14.0.1
readonly QTCREATOR_TMP=$TMP/QtCreator

mkdir --parents "$QTCREATOR_TMP"
wget "https://github.com/qt-creator/qt-creator/releases/download/v${QTCREATOR_VERSION}/qtcreator-linux-x64-${QTCREATOR_VERSION}.deb" --directory-prefix="$QTCREATOR_TMP"
sudo apt --yes install "$(realpath "$QTCREATOR_TMP/qtcreator-linux-x64-${QTCREATOR_VERSION}".deb)"

mkdir --parents ~/.local/bin/
cp -r /opt/qt-creator/share/ ~/.local/
ln -fs /opt/qt-creator/bin/qtcreator ~/.local/bin/

sudo cp /opt/qt-creator/lib/Qt/lib/libicu* "$QT5_PREFIX"/lib/

rm  -rf "$TMP"
```

Open a terminal and run the commands:

```
chmod +x qt-setup.sh
./qt-setup.sh
```

You can now delete the file `qt-setup.sh` .

<details>

<summary>Version should be 5.15.17 for <code>Qt</code></summary>

```
/opt/Qt/5.15.17/gcc_64/bin/qmake --version
> QMake version 3.1
> Using Qt version 5.15.17 in /opt/Qt/5.15.17/gcc_64/lib
```

</details>

<details>

<summary>Version should be 14.0.1 for <code>Qt Creator</code> </summary>

```
/opt/qt-creator/bin/qtcreator --version
> Qt Creator 14.0.1 based on Qt 6.7.2
```

</details>
