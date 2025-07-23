---
description: >-
  This section contains a description of the SkydelRawDataObserverInterface
  supported by Skydel.
---

# Raw Data Observer

## Real Time Raw Data

During simulation initialization, Skydel calls `getUpdateIntervalMs` to determine the update interval in milliseconds, at which raw data should be shared. If the returned value is zero, the runtime object will not be instantiated. Otherwise, Skydel will proceed to call the `createRuntimeRawDataObserver` method from each plug-in to obtain a `SkydelRuntimeRawDataObserver*`. Ownership of the returned pointer must be fully transferred to Skydel. This creation function also provides a map from IDs to strings, listing all enabled constellations and their corresponding signals.

During the simulation, Skydel will invoke the `pushRawData` method at the configured interval. This is done for every signal of every space vehicle in each simulated constellation, using the following data structure:

| TimedRawData  | Definition                | Unit                             |
| ------------- | ------------------------- | -------------------------------- |
| elapsedTimeMs | Simulation elapsed time   | millisecond                      |
| svsData       | Raw data by constellation | vector of `ConstellationRawData` |

| ConstellationRawData | Definition                | Unit                  |
| -------------------- | ------------------------- | --------------------- |
| id                   | Constellation identifier  | -                     |
| svs                  | Raw data by space vehicle | vector of `SVRawData` |

| SVRawData                | Definition                                                 | Unit                      |
| ------------------------ | ---------------------------------------------------------- | ------------------------- |
| id                       | Signal identifier                                          | -                         |
| receiverAntennaAzimuth   | Satellite’s azimuth from the receiver’s antenna position   | radian                    |
| receiverAntennaElevation | Satellite’s elevation from the receiver’s antenna position | radian                    |
| rawDatas                 | Raw data by signal                                         | vector of `SignalRawData` |



| SignalRawData               | Definition                                                                                                                                                               | Unit            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| id                          | Space vehicle identifier                                                                                                                                                 | -               |
| svElapsedTimeMs             | Elapsed time of the SV at current simulation elapsed time                                                                                                                | millisecond     |
| position (x,y,z)            | ECEF coordinates (meters) of the origin of the transmitted signal (satellite’s antenna phase center).                                                                    | meter           |
| positionError (x,y,z)       | Error on the position                                                                                                                                                    | meter           |
| bodyAzimuth                 | Azimuth angle of the satellite at the receiver relative to the North.                                                                                                    | radian          |
| bodyElevation               | Elevation angle of satellite at the receiver relative to the North                                                                                                       | radian          |
| range                       | Geometrical distance between the satellite’s and receiver’s antennas                                                                                                     | meter           |
| adr                         | Accumulated doppler range                                                                                                                                                | number of cycle |
| clockCorrection             | Satellite's Clock Correction                                                                                                                                             | second          |
| clockNoise                  | Additional clock error not accounted for in navigation message                                                                                                           | second          |
| deltaAf0                    | Clock offset                                                                                                                                                             | second          |
| deltaAf1                    | Clock drift&#xD;                                                                                                                                                         | second/second   |
| ionoCorrection              | Ionospheric corrections                                                                                                                                                  | meter           |
| tropoCorrection             | Tropospheric corrections                                                                                                                                                 | meter           |
| psrOffset                   | Pseudorange offset                                                                                                                                                       | meter           |
| receiverAntennaGain         | Receiver’s antenna gain                                                                                                                                                  | dBi             |
| svAntennaAzimuth            | Receiver’s azimuth from the satellite’s antenna position                                                                                                                 | radian          |
| svAntennaElevation          | Receiver’s elevation from the satellite’s antenna position                                                                                                               | radian          |
| relativePowerLevel          | Signal’s relative power level corresponding to the sum of the global power offset, the user’s power offset, the receiver’s antenna gain and the satellite’s antenna gain | dB              |
| doplerFrequency             | Doppler frequency due to satellites' and receivers' antennas dynamics'                                                                                                   | hertz           |
| psrChangeRate               | Pseudorange rate  due to satellites' and receivers' antennas dynamics'                                                                                                   | meters/second   |
| isEcho                      | Is this signal an echo                                                                                                                                                   |                 |
| echoPowerLoss               | Multipath power offset relative to Line of Sight (LOS) signal                                                                                                            | dB              |
| echoDopplerOffset           | Multipath frequency offset relative to LOS signal                                                                                                                        | hertz           |
| echoCarrierPhaseOffset      | Initial phase offset in multipath relative to LOS signal                                                                                                                 | radian          |
| echoPsrOffset               | Multipath pseudorange offset                                                                                                                                             | meter           |
| receiverCarrierPhaseOffset  | Phase offset caused by the receiver’s antenna phase pattern                                                                                                              | radian          |
| satelliteCarrierPhaseOffset | Phase offset caused by the satellite’s antenna phase pattern                                                                                                             | radian          |
| calibrationOffset           | Offset applied to the signal during Wavefront simulation. Used to compensate for hardware differences between elements                                                   | meter           |
| psrSatTime                  | The elapsed time of the simulation when the signal was emitted from the satellite                                                                                        | millisecond     |
| pseudorange                 | Pseudorange between the satellite’s and receiver’s antennas                                                                                                              | meter           |
| gpsTow                      | GPS time of week                                                                                                                                                         | second          |
| gpsWeekNumber               | GPS week number                                                                                                                                                          | -               |
| sbas                        | SBAS time of the day                                                                                                                                                     | second          |

## Dynamic User Interface

Same as `SkydelPositionObsereverInterface`, see [#dynamic-user-interface](position-observer.md#dynamic-user-interface "mention") of [position-observer.md](position-observer.md "mention").
