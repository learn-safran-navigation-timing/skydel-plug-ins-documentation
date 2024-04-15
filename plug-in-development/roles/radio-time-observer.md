---
description: >-
  This section contains a description of the SkydelRadioTimeObserverInterface
  supported by Skydel.
---

# Radio Time Observer

## Radio Time

During simulation initialization, Skydel will ask for a `SkydelRuntimeRadioTimeObserver*` from every plug-in via the `createRuntimeRadioTimeObserver` method. It is mandatory to give full ownership of the returned pointer to Skydel.

During simulation, Skydel will send an estimate of the radio time at 1000 Hz via the `pushRadioTime` method with the following data structure:

| RadioTimeEstimate  | Definition                                                | Unit                     |
| ------------------ | --------------------------------------------------------- | ------------------------ |
| radioElapsedTimeMs | Latest radio simulation estimated time                    | millisecond              |
| osTime             | Operating system time point of when the estimate was made | std::chrono::time\_point |

## Helper Functions

#### getDeadline(int64\_t elapsedMs, const RadioTimeEstimate& recentEstimate)

Returns an operating system `time_point` for when the RF corresponding to the given `elapsedMs`is expected to be broadcast from the radio.

This same deadline can be used in conjunction with the `DelayedBroadcaster` in the given example to broadcast a UDP packet at the same time as the RF.

#### microsecondsUntilRadioTransmission(int64\_t elapsedMs, const RadioTimeEstimate& recentEstimate)

Returns an integer number of microseconds until RF corresponding to the given `elapsedMs` is expected to be broadcast from the radio.

## Dynamic User Interface

Same as `SkydelPositionObsereverInterface`; see [here](radio-time-observer.md#dynamic-user-interface) for more detail.

## Example

See the plug-in example [radio\_time\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/radio\_time\_observer\_plugin) for more information. It covers:

* Receiving the radio time data in real time
* Updating the user interface
* Synchronizing multiple types of real time data
* Synchronizing real time data with RF transmission
