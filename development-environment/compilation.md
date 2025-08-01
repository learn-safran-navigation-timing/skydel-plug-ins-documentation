# Compilation

## Plug-in  SDK Compilation

### Source Code

```
git clone https://github.com/learn-safran-navigation-timing/skydel-plug-ins
```

### Qt Creator

In Qt Creator, open the **CMakeLists.txt** file from the root folder:

{% tabs %}
{% tab title="Ubuntu" %}
<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Windows" %}
<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Command Line

{% tabs %}
{% tab title="Ubuntu" %}
```
mkdir build && cd build
cmake -GNinja -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
sudo cmake --install .
```
{% endtab %}

{% tab title="Windows" %}
{% code title="From an elevated Command Prompt" %}
```
mkdir build
cd build
call "C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
cmake -GNinja -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
cmake --install .
```
{% endcode %}
{% endtab %}
{% endtabs %}

#### Additional CMake Arguments

<table><thead><tr><th width="226.49302936102077">Argument</th><th>Description</th><th width="150">Default Value</th></tr></thead><tbody><tr><td><code>CMAKE_INSTALL_PREFIX</code></td><td>Destination folder of the Plug-ins SDK</td><td>-</td></tr><tr><td><code>PLUGIN_INSTALL_DIR</code></td><td>Destination folder of the examples</td><td><em>$HOME/Documents/Skydel-SDX/Plug-ins</em></td></tr></tbody></table>



## Plug-in Examples Compilation

### Source Code

```
git clone https://github.com/learn-safran-navigation-timing/skydel-example-plugins
```

### Compilation

See [#plug-in-sdk-compilation](compilation.md#plug-in-sdk-compilation "mention").

The compiled plugin examples will be located in your _build folder / source / plugin-name_ as a _.so_ or _.dll_ file.

## Using the SDK in an Other CMake Project

After installing the SDK, It can be imported into your own CMake project by adding the following lines in your CMakeLists:

```
find_package(SkydelPlugin)
target_link_libraries(MyPlugin PUBLIC Skydel::SkydelPlugin)
```

