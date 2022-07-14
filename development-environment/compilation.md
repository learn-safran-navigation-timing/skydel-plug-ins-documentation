# Compilation

## Source Code

```
git clone https://github.com/learn-orolia/skydel-plug-ins
```

## Compilation

### Command Line

{% tabs %}
{% tab title="Ubuntu" %}
```
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j4 install
```
{% endtab %}

{% tab title="Windows" %}
```
mkdir build
cd build
& 'C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Auxiliary\Build\vcvars64.bat'
cmake -G"NMake Makefiles" -DCMAKE_BUILD_TYPE=Release ..
cmake --install .
```
{% endtab %}
{% endtabs %}

#### Additional CMake Arguments

| Argument                       | Description                                    | Default Value                         |
| ------------------------------ | ---------------------------------------------- | ------------------------------------- |
| `CMAKE_INSTALL_PREFIX`         | Destination folder of the Plug-ins SDK         | -                                     |
| `BUILD_SKYDEL_PLUGIN_EXAMPLES` | Whether the examples should be compiled or not | TRUE                                  |
| `PLUGIN_INSTALL_DIR`           | Destination folder of the examples             | _$HOME/Documents/Skydel-SDX/Plug-ins_ |

## Using the SDK in an Other CMake Project

After installing the SDK, It can be imported into your own CMake project by adding the following lines in your CMakeLists:

```
find_package(SkydelPlugin)
target_link_libraries(MyPlugin PUBLIC Skydel::SkydelPlugin)
```
