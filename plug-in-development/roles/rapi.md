---
description: >-
  This section contains a description of the SkydelRapiInterface supported by
  Skydel.
---

# RAPI

## Remote API

To facilitate the usage of the remote API by a plug-in, it is recommended to configure the plug-in to inherit `SkydelRapiAccess` over `SkydelRapiInterface`.

Using the remote API in a plug-in is the same as using the remote API in _python_, _C#_ and _C++_ except that the connection to Skydel is already handled since the plug-in can talk directly to the Skydel engine.

## Posting Commands

At anytime, a plug-in can send a command with the `post` method. It's possible to add a timestamp when calling `post` in order to delay the execution of the command during simulation. It's also possible to add a function callback that will contain the command execution result.

## Example

See the plug-in example [rapi\_plugin](https://github.com/learn-safran-navigation-timing/skydel-example-plugins/tree/master/source/rapi\_plugin) for more information. It covers:

* Inheriting `SkydelRapiAccess` to facilitate `post` call
* Posting commands to Skydel
* Using callback to handle command result
