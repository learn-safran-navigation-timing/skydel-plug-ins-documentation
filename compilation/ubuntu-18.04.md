# Ubuntu 18.04

## Getting the Source Code

The GitHub repository contains the Skydel Plug-ins SDK and some examples:

```
git clone https://github.com/learn-orolia/skydel-plug-ins
```

#### Updating the Souce Code

```
cd skydel-plug-ins
git pull
```

#### Getting a Specific Version of the Source Code

{% hint style="info" %}
Checkout this [page](https://github.com/learn-orolia/skydel-plug-ins/releases) for supported versions.
{% endhint %}

```
cd skydel-plug-ins
git checkout VERSION
```

## QMake

In this section we will show you how to compile the examples plug-ins using QMake and make them available in Skydel.

### QtCreator

You can use QtCreator to compile the project.

Open Qt Creator, go to _File / Open File or Project..._ and select the project file located in _skydel-plug-ins / skydel\_plugin.pro_. Make sure the selected kit is _Desktop Qt 5.12.3 QCC 64bit_ and select _Configure Project:_

![](../.gitbook/assets/ub\_compile\_1.png)

Go to Projects, change _Edit build configuration_ for _Release_ and disable _Shadow build:_

![](../.gitbook/assets/ub\_compile\_2.png)

Go to Edit, right click on the root folder and select _Rebuild:_

![](../.gitbook/assets/ub\_compile\_3.png)

Build output can be found in _skydel-plugin-ins / bin_ under the form of a shared object (e.g. libsimple\_plugin.so)

![](../.gitbook/assets/ub\_compile\_4.png)

To make build output available in Skydel, move the _.so_ file to _Skydel Data Folder / Plug-ins:_

![](../.gitbook/assets/ub\_compile\_5.png)

## CMake

In this section we will show you how to compile the examples plug-ins using CMake and make them available in Skydel.

### Command Line

#### Build and Install

You can build and install Skydel Plug-ins SDK with all the examples from command line, out of source build are suggested:&#x20;

```
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j4 install
```

#### Installation Destination

Use the CMake argument `DPLUGIN_INSTALL_DIR` to configure the installation destination folder:

```
cmake -DPLUGIN_INSTALL_DIR=mypath ..
```

{% hint style="info" %}
By default, plug-ins are installed in _$HOME/Documents/Skydel-SDX/Plug-ins._
{% endhint %}

#### Examples Compilation

Use the CMake argument `DBUILD_SKYDEL_PLUGIN_EXAMPLES` to configure whether the examples should be compiled or not:

```
cmake -DBUILD_SKYDEL_PLUGIN_EXAMPLES=FALSE ..
```

{% hint style="info" %}
By default, the examples are compiled.
{% endhint %}

#### Using the SDK in an Other CMake Project

After installing the SDK, It can be imported into your own CMake project by adding the following lines in your CMakeLists:

```
find_package(SkydelPlugin)
target_link_libraries(MyPlugin PUBLIC Skydel::SkydelPlugin)
```
