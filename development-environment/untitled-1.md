# Ubuntu 18.04

## GCC 7.5

The GCC compiler is already installed, make sure the version is _7.5:_

```text
gcc -v
```

## Qt Open Source 5.12.3

### Installation

{% hint style="info" %}
The installer name may differ depending of the version name
{% endhint %}

Download the latest online installer: [https://www.qt.io/download-open-source](https://www.qt.io/download-open-source)

Make the installer an executable and launch it:

```text
chmod +x ./qt-unified-linux-x64-3.2.3-online.run
./qt-unified-linux-x64-3.2.3-online.run
```

![](../.gitbook/assets/install_qt_1.png)

![](../.gitbook/assets/install_qt_2.png)

![](../.gitbook/assets/install_qt_3.png)

![](../.gitbook/assets/install_qt_4.png)

![](../.gitbook/assets/install_qt_8.png)

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

### Configuration

Open Qt Creator, go to _Tools / Options... / Kits_ and select _Desktop Qt 5.12.3 GCC 64bit \(default\):_

![](../.gitbook/assets/config_qt_1.png)

Make sure to match the following:

* Compiler 
  * C: _GCC\(C, x86 64bit in /usr/bin\)_
  * C++: _GCC\(C++, x86 64bit in /usr/bin\)_
* Qt version : _Qt 5.12.3 GCC 64bit_

## Git 2.17.1

```text
sudo apt install git
```

