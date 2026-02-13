# Welcome to the SimAPI wiki

SimAPI is a telemetry mapping library and daemon for racing simulators on Linux. It reads telemetry data from simulator software running under Wine/Proton (or natively via plugins) and maps it into a standardized shared memory format for use by other applications.

## Supported Applications
* [monocoque](/simapi/monocoque) - haptic feedback, LED displays, and device control
* simmonitor - telemetry monitoring

SimAPI can also be used in conjunction with:
* CrewChief

## Getting Started

[SimAPI supports 16+ simulator titles](/simapi/supportedsims) including Assetto Corsa, RFactor 2, Automobilista 2, LeMans Ultimate, and more.

The recommended setup is to install [simd](/simapi/simd), the SimAPI daemon, which handles automatic simulator detection and telemetry mapping. For manual setup, see [simshmbridge](https://github.com/spacefreak18/simshmbridge).

## Documentation

* [Supported Simulators](/simapi/supportedsims) - full compatibility matrix
* [Shared Memory](/simapi/sharedmemory) - how SimAPI bridges Wine/Proton shared memory to native Linux
* [simd Daemon](/simapi/simd) - automatic telemetry mapping daemon
* [simd Usage](/simapi/simd_usage) - setup and configuration guide
* [simd Poke](/simapi/simd_poke) - testing and debugging with simulated data
* [RFactor 2 Setup](/simapi/rfactor2) - RFactor 2 / LeMans Ultimate native plugin setup
