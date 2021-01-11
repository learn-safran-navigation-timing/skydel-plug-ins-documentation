---
description: Here's the description of every role supported by Skydel.
---

# Roles

## SkydelCoreInterface

### Runtime Logging

During simulation initialization, Skydel will create a temporary folder for every plug-in instance. Existing folders will be wiped. Plug-in instances can access this path via the `setLogPath` method. The path have the following format: _Skydel Data Folder / Output / A / B_ where _A_ is the configuration name and _B_ the plug-in instance name.

### Graphical User Interface

When instantiating a plug-in, Skydel asks for a `QWidget*` via the `createUI` method. The returned widget will be displayed in Skydel user interface under _Settings / Plug-ins / B / Plug-in UI_ where _B_ is the plug-in instance name. It's mandatory to fully give the ownership of the widget pointer to Skydel. 

```cpp
QWidget* B::createUI()
{
    auto* ui = new QWidget{};
    
    // connect signal/slots
    
    return ui;
}
```

### Notification

When instantiating a plug-in, Skydel will create and give a `SkydelNotifierInterface*` for every plug-in instance via the `setNotifier` method. With this object, a plug-in instance can:

* Send 3 different type of notification to Skydel via the `notify` method
* Tell Skydel to change it's state to unsaved via the `setDirty` method

| Type | Description |
| :--- | :--- |
| `INFO` | \[Default\] Message logged in _simulator.log as_ an _info_ |
| `WARNING` | Message logged in _simulator.log_ and Skydel _Status Log_ tab as a _warning_ |
| `ERROR` | Message logged in _simulator.log and_ Skydel _Status Log_ tab as an _error_ |

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

### Configuration

When saving, Skydel will demand a `QJsonObject` to every plug-in instance via the `getConfiguration` method. The JSON object holds the current configuration of the plug-in instance, which will be saved in the Skydel configuration file paired with the version of the plug-in instance it was generated with. The version saved in the Skydel configuration file comes from the plug-in instance metadata at save time.

When loading a configuration file, Skydel will automatically instantiate all saved plug-in instance and restore their configuration via the `setConfiguration` method. It's possible for a plug-in instance to handle configuration backward compatibility since the configuration received is paired with a creation version.

### Example

See plug-in example [simple\_plugin](https://github.com/learn-orolia/skydel-plug-ins/tree/master/source/example/simple_plugin). It covers:

* Creating a user interface
* Sending notification messages
* Triggering the unsaved state
* Saving and loading of configuration

## SkydelPositionObserverInterface

### Real Time Positions

During simulation initialization, Skydel will ask for a `SkydelRuntimePositionObserver*` from every plug-in instance via the `createRuntimePositionObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel.

During simulation, Skydel will send vehicle position data in _ecef_ at 1000 Hz via the `pushPosition` method with the following data structure:

| TimedPosition | Unit |
| :--- | :--- |
| time | millisecond |
| position\(x, y, z\) | meter |
| orientation\(roll, pitch, yaw\) | radian |

### Dynamic User Interface

During simulation initialization, Skydel will give the chance to plug-in instances to connect to their user interface for real-time updates via the `connectToView` method.

### Example

See plug-in example [position\_observer\_plugin](https://github.com/learn-orolia/skydel-plug-ins/tree/master/source/example/position_observer_plugin). It covers:

* Receiving real time position data
* Updating the user interface
* Logging in the temporary folder
* Sending position data on the network

## SkydelLicensingInterface

{% hint style="warning" %}
To use this role and see an example, contact Orolia Canada's technical support.
{% endhint %}

## SkydelInstrumentationInterface

### Engine Graph

During simulation initialization, Skydel will send a graph showing the relationship between each module in the Skydel engine to every plug-in instance via the `setEngineGraph` method.

### Real Time Statistics

During simulation, Skydel will send statistics about which module is throttling the simulation at 100 Hz via the `pushQueueMeasures` method.

### Example

See plug-in example skydel\_default\_instrumentation\_plugin. It covers:

* Generating a _.dot_ file with the graph data
* Logging Skydel engine queue size

## SkydelRapiInterface

To facilitate the usage of the remote API by a plug-in, it's suggested to make the plug-in inherit  `SkydelRapiAccess` over `SkydelRapiInterface`. 

Using the remote API in a plug-in is the same as using the remote API in _python_, _C\#_ and _C++_ except that the connection to Skydel is already handled since the plug-in can talk directly to the Skydel engine.

### Posting Commands

At anytime, a plug-in instance can send a command with the `post` method. It's possible to add a timestamp when calling `post` in order to delay the execution of the command during simulation. It's also possible to add a function callback that will contain the command execution result. 

### Example

See plug-in example rapi\_plugin. It covers:

* Inheriting `SkydelRapiAccess` to facilitate `post` call
* Posting commands to Skydel
* Using callback to handle command result

