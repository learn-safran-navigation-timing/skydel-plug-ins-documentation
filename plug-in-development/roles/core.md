---
description: >-
  This section contains a description of the SkydelCoreInterface supported by
  Skydel.
---

# Core

## Runtime Logging

During simulation initialization, Skydel will create a temporary folder for every plug-in. Existing folders will be wiped. Plug-ins can access this path via the `setLogPath` method. The path has the following format: _Skydel Data Folder / Output / A / B_ where _A_ is the configuration name and _B_ the plug-in name.

## Graphical User Interface

When enabling a plug-in, Skydel requests a `SkydelWidgets` object via the `createUI` method, which is a collection of `SkydelWidget` instances. This allows a plug-in to integrate multiple widgets through the Skydel user interface and define a custom location for each of its widgets. When no `path` and `name` is specified, the default location of a widget will be under _Settings / Plug-ins / B_, where _B_ is the plugin name. It is mandatory to fully transfer ownership of the widget pointer to Skydel.

```cpp
SkydelWidgets B::createUI()
{
    auto* ui = new QWidget{};

    // connect signal/slots

    return {{ui, "Settings", "B"}};
}
```

<table><thead><tr><th width="166">SkydelWidget</th><th>Description</th></tr></thead><tbody><tr><td>widget</td><td>Contains the user interface widget</td></tr><tr><td>path</td><td>Location of the widget in the Skydel user interface supports two values: <code>Settings</code> and <code>Dock</code>. An exception is made for <code>Settings</code>; in this case, sub-paths are supported (for example, <code>Settings/MyPath</code>).</td></tr><tr><td>name</td><td>Name of the widget access to be displayed in the user interface</td></tr></tbody></table>

## Notification

When enabling a plug-in, Skydel will create and give a `SkydelNotifierInterface*` via the `setNotifier` method. With this object, a plug-in can:

* Send 3 different types of notifications to Skydel via the `notify` method
* Tell Skydel to change its state to Unsaved via the `setDirty` method

<table data-full-width="false"><thead><tr><th width="146" align="center">Type</th><th>Description</th></tr></thead><tbody><tr><td align="center"><code>INFO</code></td><td>[Default] Message logged in <em>simulator.log as</em> an <em>info</em></td></tr><tr><td align="center"><code>WARNING</code></td><td>Message logged in <em>simulator.log</em> and Skydel <em>Status Log</em> tab as a <em>warning</em></td></tr><tr><td align="center"><code>ERROR</code></td><td>Message logged in <em>simulator.log and</em> Skydel <em>Status Log</em> tab as an <em>error</em></td></tr></tbody></table>

Sending an `ERROR` notification at runtime will cause the simulation to stop.

```cpp
void B::setNotifier(SkydelNotifierInterface* notifier)
{
    // Sends an warning message to Skydel
    notifier->notify("Hello", SkydelNotifierInterface::Type::WARNING);

    // Tells Skydel to change it' state to unsaved
    notifier->setDirty();
}
```

## Configuration

When saving, Skydel will demand a `QJsonObject` to every plug-in via the `getConfiguration` method. The JSON object holds the current configuration of the plug-in, which will be saved in the Skydel configuration file and paired with the version of the plug-in it was generated with. The version saved in the Skydel configuration file comes from the plug-in metadata at save time.

When loading a configuration file, Skydel will automatically restore the configuration of the plug-in via the `setConfiguration` method. It's possible for a plug-in to handle configuration backward compatibility since the configuration received is paired with a creation version.

## Simulation Initialization

During the simulation initialization state, Skydel allows plug-ins to execute code before the simulation starts via the `initialize` method.

Remote API clients cannot execute commands during Skydel's initialization state, but an exception is made for plug-ins within the initialize method. Although the simulator is technically in the _Started_ state, Skydel allows plug-ins to execute _Idle_ commands.

## Example

See the plug-in example [simple\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/simple_plugin) for more information. It covers:

* Creating a user interface
* Sending notification messages
* Triggering the Unsaved state
* Saving and loading of configuration
