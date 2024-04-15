---
description: >-
  This section contains a description of the SkydelRawDataObserverInterface
  supported by Skydel.
---

# Raw Data Observer

## Real Time Raw Data

During simulation initialization, Skydel will ask for a `SkydelRuntimeRawDataObserver*` from every plug-in via the `createRuntimeRawDataObserver` method. It's mandatory to fully give the ownership of the returned pointer to Skydel. The create function also shares an ID to String map, for all enabled Constellations, and for all enabled Signals.

During simulation, Skydel will send raw data at 1 Hz via the `pushRawData` method for every signal of every space vehicle of every constellation that is currently being simulated with the following data structure:

| TimedRawData  | Definition                | Unit                             |
| ------------- | ------------------------- | -------------------------------- |
| elapsedTimeMs | Simulation elapsed time   | millisecond                      |
| svsData       | Raw data by constellation | vector of `ConstellationRawData` |

| ConstellationRawData | Definition                | Unit                  |
| -------------------- | ------------------------- | --------------------- |
| id                   | Constellation identifier  | -                     |
| svs                  | Raw data by space vehicle | vector of `SVRawData` |

| SVRawData | Definition         | Unit                      |
| --------- | ------------------ | ------------------------- |
| id        | Signal identifier  | -                         |
| rawDatas  | Raw data by signal | vector of `SignalRawData` |



<table><thead><tr><th width="316.3333333333333">SignalRawData</th><th>Definition</th><th>Unit</th></tr></thead><tbody><tr><td>id</td><td>Space vehicle identifier</td><td>-</td></tr><tr><td>svElapsedTimeMs</td><td>Elapsed time of the SV at current simulation elapsed time</td><td>millisecond</td></tr><tr><td>position (x,y,z)</td><td>ECEF coordinates (meters) of the origin of the transmitted signal (satellite’s antenna phase center).</td><td>meter</td></tr><tr><td>positionError (x,y,z)</td><td>Error on the position</td><td>meter</td></tr><tr><td>range</td><td>Geometrical distance between the satellite’s and receiver’s antennas</td><td>meter</td></tr><tr><td>psr</td><td>Pseudorange</td><td>meter</td></tr><tr><td>adr</td><td>Accumulated doppler range</td><td>number of cycle</td></tr><tr><td>clockCorrection</td><td>Satellite's Clock Correction</td><td>second</td></tr><tr><td>clockNoise</td><td>Additional clock error not accounted for in navigation message</td><td>second</td></tr><tr><td>deltaAf0</td><td>Clock offset</td><td>second</td></tr><tr><td>deltaAf1</td><td>Clock drift</td><td>second/second</td></tr><tr><td>ionoCorrection</td><td>Ionospheric corrections</td><td>meter</td></tr><tr><td>tropoCorrection</td><td>Tropospheric corrections</td><td>meter</td></tr><tr><td>psrOffset</td><td>Pseudorange offset</td><td>meter</td></tr><tr><td>receiverAntennaAzimuth</td><td>Satellite’s azimuth from the receiver’s antenna position</td><td>radian</td></tr><tr><td>receiverAntennaElevation</td><td>Satellite’s elevation from the receiver’s antenna position</td><td>radian</td></tr><tr><td>receiverAntennaGain</td><td>Receiver’s antenna gain</td><td>dBi</td></tr><tr><td>svAntennaAzimuth</td><td>Receiver’s azimuth from the satellite’s antenna position</td><td>radian</td></tr><tr><td>svAntennaElevation</td><td>Receiver’s elevation from the satellite’s antenna position</td><td>radian</td></tr><tr><td>relativePowerLevel</td><td>Signal’s relative power level corresponding to the sum of the global power offset, the user’s power offset, the receiver’s antenna gain and the satellite’s antenna gain</td><td>dB</td></tr><tr><td>doplerFrequency</td><td>Doppler frequency due to satellites' and receivers' antennas dynamics'</td><td>hertz</td></tr><tr><td>psrChangeRate</td><td>Pseudorange rate due to satellites' and receivers' antennas dynamics'</td><td>meter/second</td></tr><tr><td>echoPowerLoss</td><td>Multipath power offset relative to Line of Sight (LOS) signal</td><td>dB</td></tr><tr><td>echoDopplerOffset</td><td>Multipath frequency offset relative to LOS signal</td><td>hertz</td></tr><tr><td>echoCarrierPhaseOffset</td><td>Initial phase offset in multipath relative to LOS signal</td><td>radian</td></tr><tr><td>echoPsrOffset</td><td>Multipath pseudorange offset</td><td>meter</td></tr><tr><td>receiverCarrierPhaseOffset</td><td>Phase offset caused by the receiver’s antenna phase pattern</td><td>radian</td></tr><tr><td>satelliteCarrierPhaseOffset</td><td>Phase offset caused by the satellite’s antenna phase pattern</td><td>radian</td></tr><tr><td>gpsTow</td><td>GPS time of week</td><td>second</td></tr><tr><td>gpsWeekNumber</td><td>GPS week number</td><td>-</td></tr><tr><td>sbas</td><td>SBAS time of the day</td><td>second</td></tr><tr><td>calibrationOffset</td><td>Offset applied to the signal during Wavefront simulation. Used to compensate for hardware differences between elements</td><td>meter</td></tr><tr><td>psrSatTime</td><td>The elapsed time of the simulation when the signal was emitted from the satellite</td><td>millisecond</td></tr></tbody></table>

## Dynamic User Interface

Same as `SkydelPositionObsereverInterface`, see [here](raw-data-observer.md#dynamic-user-interface) for more detail.
