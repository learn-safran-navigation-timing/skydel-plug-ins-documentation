---
description: This section contains a description of every role supported by Skydel.
---

# Roles

## SkydelCoreInterface

### Runtime Logging

During simulation initialization, Skydel will create a temporary folder for every plug-in instance. Existing folders will be wiped. Plug-in instances can access this path via the `setLogPath` method. The path has the following format: _Skydel Data Folder / Output / A / B_ where _A_ is the configuration name and _B_ the plug-in instance name.

### Graphical User Interface

When instantiating a plug-in, Skydel asks for a `QWidget*` via the `createUI` method. The returned widget will be displayed in the Skydel user interface under _Settings / Plug-ins / B / Plug-in UI_ where _B_ is the plug-in instance name. It's mandatory to fully give the ownership of the widget pointer to Skydel.

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

* Send 3 different types of notifications to Skydel via the `notify` method
* Tell Skydel to change its state to Unsaved via the `setDirty` method

| Type      | Description                                                                  |
| --------- | ---------------------------------------------------------------------------- |
| `INFO`    | \[Default] Message logged in _simulator.log as_ an _info_                    |
| `WARNING` | Message logged in _simulator.log_ and Skydel _Status Log_ tab as a _warning_ |
| `ERROR`   | Message logged in _simulator.log and_ Skydel _Status Log_ tab as an _error_  |

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

When saving, Skydel will demand a `QJsonObject` to every plug-in instance via the `getConfiguration` method. The JSON object holds the current configuration of the plug-in instance, which will be saved in the Skydel configuration file and paired with the version of the plug-in instance it was generated with. The version saved in the Skydel configuration file comes from the plug-in instance metadata at save time.

When loading a configuration file, Skydel will automatically instantiate all saved plug-in instances and restore their configuration via the `setConfiguration` method. It's possible for a plug-in instance to handle configuration backward compatibility since the configuration received is paired with a creation version.

### Example

See the plug-in example [simple\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/simple\_plugin) for more information. It covers:

* Creating a user interface
* Sending notification messages
* Triggering the Unsaved state
* Saving and loading of configuration

## SkydelPositionObserverInterface

### Real Time Positions

During simulation initialization, Skydel will ask for a `SkydelRuntimePositionObserver*` from every plug-in instance via the `createRuntimePositionObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel.

During simulation, Skydel will send the vehicle position data in _ecef_ at 1000 Hz via the `pushPosition` method with the following data structure:

| TimedPosition                 | Unit        |
| ----------------------------- | ----------- |
| time                          | millisecond |
| position(x, y, z)             | meter       |
| orientation(roll, pitch, yaw) | radian      |

### Dynamic User Interface

During simulation initialization, Skydel will provide opportunity for plug-in instances to connect to their user interface for real-time updates via the `connectToView` method. The `QWidget*` parameter is the same as the one created by the `createUI` method.

### Example

See the plug-in example [position\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/position\_observer\_plugin) for more information. It covers:

* Receiving real time position data
* Updating the user interface
* Logging in the temporary folder
* Sending position data on the network

## SkydelRawDataObserverInterface

### Real Time Raw Data

During simulation initialization, Skydel will ask for a `SkydelRuntimeRawDataObserver*` from every plug-in instance via the `createRuntimeRawDataObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel.

During simulation, Skydel will send raw data at 1 Hz via the `pushRawData` method for every signal of every space vehicle of every constellation that is currently being simulated with the following data structure:

| TimedRawData  | Definition                | Unit                             |
| ------------- | ------------------------- | -------------------------------- |
| elapsedTimeMs | Simulation elapsed time   | millisecond                      |
| svsData       | Raw data by constellation | vector of `ConstellationRawData` |

| ConstellationRawData | Definition                | Unit                  |
| -------------------- | ------------------------- | --------------------- |
| system               | Constellation identifier  | Enum `Constellation`  |
| svs                  | Raw data by space vehicle | vector of `SVRawData` |

| SVRawData | Definition               | Unit                      |
| --------- | ------------------------ | ------------------------- |
| svID      | Space vehicle identifier | -                         |
| rawDatas  | Raw data by signal       | vector of `SignalRawData` |

| SignalRawData   | Definition                                                | Unit            |
| --------------- | --------------------------------------------------------- | --------------- |
| signal          | Signal identifier                                         | Enum `Signal`   |
| svElapsedTimeMs | Elapsed time of the SV at current simulation elapsed time | millisecond     |
| pseudorange     | Pseudorange                                               | meter           |
| adr             | Accumulated doppler range                                 | number of cycle |

### Dynamic User Interface

Same as `SkydelPositionObsereverInterface`, see [here](roles.md#dynamic-user-interface) for more detail.

## SkydelHilObserverInterface

### HIL Interface Received Positions

During simulation initialization, Skydel will ask for a `SkydelRuntimeHilObserver*` from every plug-in instance via the `createRuntimeHilObserver` method. It is mandatory to give full ownership of the returned pointer to Skydel.

During the simulation, Skydel will send the HIL positions in _ecef_ format as they are received using the `pushHilInput` method. The position will be received in the same data structure as the real time positions of `SkydelPositionObsereverInterface` presented [here](roles.md#skydelpositionobserverinterface).

### Dynamic User Interface

Same as `SkydelPositionObsereverInterface`; see [here](roles.md#dynamic-user-interface) for more detail.

### Example

See the plug-in example [hil\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/hil\_observer\_plugin) for more information. It covers:

* Receiving HIL data in real time
* Updating the user interface
* Logging in the temporary folder

## SkydelRadioTimeObserverInterface

### Radio Time

During simulation initialization, Skydel will ask for a `SkydelRuntimeRadioTimeObserver*` from every plug-in instance via the `createRuntimeRadioTimeObserver` method. It is mandatory to give full ownership of the returned pointer to Skydel.

During simulation, Skydel will send an estimate of the radio time at 1000 Hz via the `pushRadioTime` method with the following data structure:

| RadioTimeEstimate  | Definition                                                | Unit                     |
| ------------------ | --------------------------------------------------------- | ------------------------ |
| radioElapsedTimeMs | Latest radio simulation estimated time                    | millisecond              |
| osTime             | Operating system time point of when the estimate was made | std::chrono::time\_point |

### Helper Functions

#### getDeadline(int64\_t elapsedMs, const RadioTimeEstimate& recentEstimate)

Returns an operating system `time_point` for when the RF corresponding to the given `elapsedMs`is expected to be broadcast from the radio.

This same deadline can be used in conjunction with the `DelayedBroadcaster` in the given example to broadcast a UDP packet at the same time as the RF.

#### microsecondsUntilRadioTransmission(int64\_t elapsedMs, const RadioTimeEstimate& recentEstimate)

Returns an integer number of microseconds until RF corresponding to the given `elapsedMs` is expected to be broadcast from the radio.

### Dynamic User Interface

Same as `SkydelPositionObsereverInterface`; see [here](roles.md#dynamic-user-interface) for more detail.

### Example

See the plug-in example [radio\_time\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/radio\_time\_observer\_plugin) for more information. It covers:

* Receiving the radio time data in real time
* Updating the user interface
* Synchronizing multiple types of real time data
* Synchronizing real time data with RF transmission

## SkydelTransmitterObserverInterface

### Real Time Spoofers/Interferences Data

During simulation initialization, Skydel will ask for a `SkydelRuntimeTransmitterObserver*` from every plug-in instance via the `createRuntimeTransmitterObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel.

This role provides detailed information about each transmitter. A transmitter can broadcast interference signals such as spoofers and interferences. During the simulation, Skydel will send transmitters data at 1000 Hz via the `pushTransmitters` method with the following data structure:

| TimedTransmitters | Definition              | Unit                    |
| ----------------- | ----------------------- | ----------------------- |
| elapsedTimeMs     | Simulation elapsed time | millisecond             |
| interferences     | Simulated interferences | vector of `Transmitter` |
| spoofers          | Simulated spoofers      | vector of `Transmitter` |

| Transmitter                   | Definition                                                          | Unit                           |
| ----------------------------- | ------------------------------------------------------------------- | ------------------------------ |
| id                            | Unique ID among transmitters                                        | -                              |
| name                          | User-friendly name for the transmitter                              | -                              |
| colorName                     | Transmitter color in Skydel UI                                      | -                              |
| position(x, y, z)             | Transmitter position                                                | meter                          |
| orientation(roll, pitch, yaw) | Orientation is in NED reference frame                               | radian                         |
| isEnabled                     | When false, no signals are broadcast                                | -                              |
| isMasked                      | When false, no signals are broadcast                                | -                              |
| usingManualPropagationLoss    | When false, free-space-loss is used. When true, manual loss is used | -                              |
| referencePower                | Transmitter reference power                                         | dBm                            |
| range                         | Distance to vehicle                                                 | meter                          |
| elevation                     | Elevation of this transmitter from the vehicle                      | radian                         |
| azimuth                       | Azimuth of this transmitter from the vehicle                        | radian                         |
| interferenceSignals           | All signals associated with this transmitter                        | vector of `InterferenceSignal` |

| InterferenceSignal | Definition                                                                  | Unit |
| ------------------ | --------------------------------------------------------------------------- | ---- |
| id                 | Unique ID among signals from the transmitter                                | -    |
| isEnabled          | When false, signal is not broadcast                                         | -    |
| centerFrequency    | Signal center frequency                                                     | Hz   |
| propagationLoss    | Propagation loss                                                            | dB   |
| gainAtTransmitter  | Transmitter antenna gain in the direction pointing to the vehicle           | dB   |
| gainAtVehicle      | Vehicle(receiver) antenna gain in the direction pointing to the transmitter | dB   |
| signalPower        | Power relative to transmitter, `NaN` if spoofing signal                     | dB   |
| typeName           | Type of transmitter                                                         | -    |

{% hint style="info" %}
`typeName` can have the following values: _Spoof_, _CW_, _Chirp_, _Pulse_, _BPSK_, _BOC_, _AWGN_ or _IQ file_.
{% endhint %}

### String ID Uniqueness

Implementors should be careful not to rely on global uniqueness of ID strings. A spoofer and a transmitter can share identical ID strings and identical names. However, Skydel guarantees that IDs are unique among spoofer transmitters and IDs are unique among interference transmitters.

### Dynamic User Interface

Same as `SkydelPositionObsereverInterface`; see [here](roles.md#dynamic-user-interface) for more detail.

### Example

See the plug-in example [transmitter\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/transmitter\_observer\_plugin) for more information. It covers:

* Receiving real time spoofers/interferences data
* Updating the user interface
* Logging in the temporary folder
* Sending spoofers/interferences data on the network

## SkydelLicensingInterface

{% hint style="warning" %}
To use this role and see an example, contact Orolia Canada's technical support.
{% endhint %}

## SkydelRapiInterface

To facilitate the usage of the remote API by a plug-in, it is recommended to configure the plug-in to inherit `SkydelRapiAccess` over `SkydelRapiInterface`.

Using the remote API in a plug-in is the same as using the remote API in _python_, _C#_ and _C++_ except that the connection to Skydel is already handled since the plug-in can talk directly to the Skydel engine.

### Posting Commands

At anytime, a plug-in instance can send a command with the `post` method. It's possible to add a timestamp when calling `post` in order to delay the execution of the command during simulation. It's also possible to add a function callback that will contain the command execution result.

### Example

See the plug-in example [rapi\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/rapi\_plugin) for more information. It covers:

* Inheriting `SkydelRapiAccess` to facilitate `post` call
* Posting commands to Skydel
* Using callback to handle command result
