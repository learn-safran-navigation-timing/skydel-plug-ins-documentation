# Migration

## Introduction

Custom signals are now integrated into plug-ins since Skydel version **24.12**. This step-by-step guide will help you migrate your legacy custom signal code to the plug-in SDK.

## Prerequisites

Before you begin, ensure you have completed the following:

* You have a copy of the legacy [skydel-custom-signal](https://github.com/learn-safran-navigation-timing/skydel-custom-signal) GitHub repository.
* You have installed the development environment. If not, please refer to [Broken link](broken-reference "mention").
* You have checked out and compiled the latest version of the [skydel-example-plugins](https://github.com/learn-safran-navigation-timing/skydel-example-plugins) GitHub repository. If not, please refer to [compilation.md](../development-environment/compilation.md "mention").

## Migration

The interfaces required for custom signals remain largely the same but are now integrated into the plug-in SDK. In this guide, we'll use the custom signal GPS CA as an example. The steps outlined here are applicable to any custom signal. The GPS CA example is located in [_example\_gps\_ca_](https://github.com/learn-safran-navigation-timing/skydel-custom-signal/tree/main/example_gps_ca) of the legacy project, and [_source / custom\_signals / example\_gps\_ca_](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/custom_signals/example_gps_ca) in the new project. A few differences can be observed if we compare both project, here are the outlines.

Here is a compressed archive containing the code before and after the migration. You can use a "diff " tool like [WinMerge](https://winmerge.org/?lang=en) to better visualize the differences between the legacy and the new approach.

{% file src="../.gitbook/assets/custom_ca_diff.zip" %}

### <mark style="color:yellow;">Changes - CMakeLists.txt</mark>

Changes were required in the CMake configuration file to compile a custom signal example as a plug-in.

```diff
# New variable for plugins to be compatible with Skydel
+ set(CMAKE_AUTOMOC ON)
+ set(CMAKE_AUTORCC ON)
+ set(CMAKE_AUTOUIC ON)

# Simplification of source files initialisation
+ file(GLOB EXAMPLE_CA_SRC *.cpp)
+ add_library(custom_ca SHARED ${EXAMPLE_CA_SRC})
- file(GLOB CUSTOM_CA_SRC *.cpp)
- file(GLOB CUSTOM_COMMON "../common/*.cpp")
- list(APPEND CUSTOM_CA_SRC ${CUSTOM_COMMON})
- add_library(custom_ca SHARED ${CUSTOM_CA_SRC})
- add_library(custom_ca_static STATIC ${CUSTOM_CA_SRC})

# Link with plug-in SDK and common library
+ target_link_libraries(custom_ca PUBLIC SkydelPlugin custom_signal_common)
+ set_target_properties(custom_ca PROPERTIES CXX_STANDARD 17)
- target_link_libraries(custom_ca PUBLIC custom_signal)
- target_link_libraries(custom_ca_static PUBLIC custom_signal)
- target_include_directories(custom_ca PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
- target_include_directories(custom_ca_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Add plug-in target compile definitions
+ target_compile_definitions(custom_ca PRIVATE PLUGIN_IID="custom_ca" PLUGIN_FILE="custom_ca.json")
- target_compile_definitions(custom_ca_static PRIVATE IS_STATIC)

# Improved deployment
+ install(TARGETS custom_ca RUNTIME DESTINATION ${PLUGIN_INSTALL_DIR})
+ install(FILES custom_ca_downlink.csv custom_ca.xml DESTINATION ${PLUGIN_INSTALL_DIR})
- file(COPY custom_ca_downlink.csv custom_ca.xml DESTINATION ${CMAKE_LIBRARY_OUTPUT_DIRECTORY})
```

### <mark style="color:yellow;">Changes - custom\_ca.xml</mark>

* **Root Element** - The mandatory root element `CustomSignals` has been added to the XML file format, enabling a single XML file to define multiple custom signals.
* **Mandatory Fields** - The `CustomSignal` element now includes the mandatory field `Name`, which corresponds to the legacy custom signal name from the Skydel command used to add a custom signal.
* **Removed Field** - The `DLLPath` element is no longer required, as custom signals are now imported as plug-ins in Skydel.
* **File Naming** - For Skydel to properly detect the custom signal plug-in, the XML file must have the same name as the `PLUGIN_IID` name specified in the _CMakeLists.txt_ file.

```diff
+ <CustomSignals>
+   <CustomSignal Name="CustomCa" Version="1.0" NavMsg="True">
-   <CustomSignal Version="1.0" NavMsg="True">
        <Constellation>GPS</Constellation>
        <CentralFreq>1575420000</CentralFreq>
        <Bandwidth>2046000</Bandwidth>
        <ModulationCoef Real="0" Imag="1" />
-       <DLLPath>./custom_ca</DLLPath>
        <Codes>
            <Code Id="L1CA" />
        </Codes>
    </CustomSignal>
+ </CustomSignals>
```

### <mark style="color:yellow;">Changes - custom\_ca.h</mark>

The changes to the custom signal implementation are primarily naming-related. The following interfaces and structures have been renamed:

* `ICustomSignal` → `SkydelCustomSignalInterface`
* `ICustomSignalCode` → `SkydelCustomSignalCode`
* `ICustomSignalNavMsg` → `SkydelCustomSignalNavMsg`
* `CSInitData` → `Sdx::CS::InitData`

### <mark style="color:yellow;">Changes - custom\_ca.cpp</mark>

The macro `DEFINE_CUSTOM_SIGNAL` is no longer required. Plug-in registration is now handled directly in the _custom\_ca\_plugin.h_ file using the macro `REGISTER_SKYDEL_PLUGIN`.

### <mark style="color:green;">New - custom\_ca\_plugin.h</mark>

This new file contains the boilerplate code required for a library to function as a Skydel plug-in.

* **Main Class -** The main plug-in class must inherit from `SkydelCustomSignalFactoryInterface`.&#x20;
* **Key Function** - Implement the `createCustomSignal` function to return the custom signal implementation.
* **Data Sharing** - Skydel shares `InitData` with the custom signal plug-in, allowing it to initialize it's custom signal implementation.
* **Return Type** - The returned object is of type `SkydelCustomSignalInterface`, equivalent to `ICustomSignal` in the legacy implementation.

Here is the _custom\_ca\_plugin.h_ file annotated with comments to clarify its content.

```cpp
#pragma once

// Contains the generic implementation of a custom CA signal
#include "custom_ca.h"

// Include in order to have access to interfaces from the plug-in SDK
#include "skydel_plug_ins/skydel_plugin.h"

// SkydelCoreInterface - Mandatory for all plug-ins
// SkydelCustomSignalFactoryInterface - Mandatory for custom signal plug-ins
class CustomCAPlugin : public QObject, public SkydelCoreInterface, public SkydelCustomSignalFactoryInterface
{
  Q_OBJECT

public:
  CustomCAPlugin();

  // SkydelCoreInterface
  inline void setLogPath(const QString&) override {}
  inline void setNotifier(SkydelNotifierInterface*) override {}
  void setConfiguration(const QString&, const QJsonObject&) override {}
  QJsonObject getConfiguration() const override { return {}; }
  SkydelWidgets createUI() override { return {}; }
  inline void initialize() override {}

  // SkydelCustomSignalFactoryInterface
  inline SkydelCustomSignalInterface* createCustomSignal(const Sdx::CS::InitData& data) override
  {
    // Returns the custom signal implementation
    return new CustomCA(data);
  };

signals:
  void configurationChanged();
};

// Mandatory macro in order to register CustomCAPlugin as a Skydel plug-in
REGISTER_SKYDEL_PLUGIN(CustomCAPlugin)
```

### <mark style="color:green;">New - custom\_ca\_plugin.cpp</mark>

Only the constructor is implemented in the source file to ensure that the source code is properly detected by CMake and successfully compiled.

### <mark style="color:green;">New - custom\_ca.json</mark>

Every Skydel plug-in requires a metadata file to specify its generic name, description, and version. Refer to any plug-in example for more details.

## Usage in Skydel

To use your newly compiled custom signal, ensure you move the dynamic library, downlink file, and XML file to the _Skydel-SDX / Plug-ins_ folder. If you are compiling directly from the example project, you can run the `cmake --install .` command from the build folder.

In Skydel, navigate to _Help / Plug-ins..._, and enable the custom signal plug-in you are interested in. Once enabled, you will have access to the custom signals defined in the XML file in Skydel, just like before.

The key difference is that your custom signal’s availability will now be persistent, unlike previously, where it was bound to the active configuration file.

## Conclusion

Here are a few key points to ensure a successful migration to the Skydel plug-in SDK:

* **Interface Changes -** No functional changes were made to the custom signal interfaces, only the names have changed.
* **Boilerplate Plug-in Class** - Implement the base plug-in class to ensure your library is recognized as a Skydel plug-in.
* **XML File Name** - The XML file name must match the custom signal `PLUGIN_IID` name defined in _CMakeLists.txt_.
* **XML File Format** **Changes** - Assign a unique name to each custom signal in the XML file to avoid conflicts and ensure proper functionality.
* **File Placement** - For custom signals to be properly imported into Skydel, ensure the following files are placed in the _Skydel-SDX / Plug-ins_ directory: dynamic library, downlink file and XML file.
