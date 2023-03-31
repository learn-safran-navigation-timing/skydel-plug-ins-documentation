# IMU Plugin

To compile the IMU plugin, you will need to install the default development environment and ensure the following dependencies are also installed.\


## Blaze

### Ubuntu

```
git clone https://github.com/parsa/blaze
cd blaze && mkdir build && cd build
cmake -DBLAZE_BLAS_MODE=ON .. && sudo make install
```

### Windows

CMake users need to install Blaze 3.7 from the official repository in their search path:

{% code title="From an elevated Command Prompt" %}
```
git clone https://github.com/parsa/blaze
cd blaze 
mkdir build
cd build 
call "C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
cmake -G"NMake Makefiles" -DCMAKE_BUILD_TYPE=Release -DBLAZE_BLAS_MODE=OFF -DUSE_LAPACK=OFF -DBLAZE_CACHE_SIZE_AUTO=OFF .. 
cmake --install .
```
{% endcode %}

{% hint style="info" %}
By default _blas_ and _lapack_ libraries are used, but feel free to check all of Blaze options from their repository documentation.
{% endhint %}
