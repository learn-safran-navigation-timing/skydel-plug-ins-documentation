---
description: >-
  This section contains a description of the SkydelPositionObserverInterface
  supported by Skydel.
---

# Position Observer

## Real Time Positions

During simulation initialization, Skydel will ask for a `SkydelRuntimePositionObserver*` from every plug-in via the `createRuntimePositionObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel.

During simulation, Skydel will send the vehicle position data in _ecef_ at 1000 Hz via the `pushPosition` method with the following data structure:

| TimedPosition                 | Unit        |
| ----------------------------- | ----------- |
| time                          | millisecond |
| position(x, y, z)             | meter       |
| orientation(roll, pitch, yaw) | radian      |

## Dynamic User Interface

During simulation initialization, Skydel will provide opportunity for plug-ins to connect to their user interface for real-time updates via the `connectToView` method. The `QWidget*` parameter is the same as the one created by the `createUI` method.

## Example

See the plug-in example [position\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/position\_observer\_plugin) for more information. It covers:

* Receiving real time position data
* Updating the user interface
* Logging in the temporary folder
* Sending position data on the network
