---
description: >-
  This section contains a description of the SkydelHilObserverInterface
  supported by Skydel.
---

# HIL Observer

## HIL Interface Received Positions

During simulation initialization, Skydel will ask for a `SkydelRuntimeHilObserver*` from every plug-in via the `createRuntimeHilObserver` method. It is mandatory to give full ownership of the returned pointer to Skydel.

During the simulation, Skydel will send the HIL positions in _ecef_ format as they are received using the `pushHilInput` method. The position will be received in the same data structure as the real time positions of `SkydelPositionObsereverInterface` presented [here](position-observer.md).

## Dynamic User Interface

Same as `SkydelPositionObsereverInterface`, see [#dynamic-user-interface](position-observer.md#dynamic-user-interface "mention") of [position-observer.md](position-observer.md "mention") for more details.

## Example

See the plug-in example [hil\_observer\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/hil_observer_plugin) for more information. It covers:

* Receiving HIL data in real time
* Updating the user interface
* Logging in the temporary folder
