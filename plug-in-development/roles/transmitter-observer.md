---
description: >-
  This section contains a description of the SkydelTransmitterObserverInterface
  supported by Skydel.
---

# Transmitter Observer

## Real Time Spoofers/Interferences Data

During simulation initialization, Skydel will ask for a `SkydelRuntimeTransmitterObserver*` from every plug-in via the `createRuntimeTransmitterObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel.

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

## String ID Uniqueness

Implementors should be careful not to rely on global uniqueness of ID strings. A spoofer and a transmitter can share identical ID strings and identical names. However, Skydel guarantees that IDs are unique among spoofer transmitters and IDs are unique among interference transmitters.

## Dynamic User Interface

Same as `SkydelPositionObsereverInterface`, see [#dynamic-user-interface](position-observer.md#dynamic-user-interface "mention") of [position-observer.md](position-observer.md "mention") for more details.

## Example

See the plug-in example [transmitter\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/transmitter_observer_plugin) for more information. It covers:

* Receiving real time spoofers/interferences data
* Updating the user interface
* Logging in the temporary folder
* Sending spoofers/interferences data on the network
