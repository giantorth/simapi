# simd

To jump right into how to use simd visit the [usage page](https://spacefreak18.github.io/simapi/simd_usage).

## Overview

simd is the SimAPI daemon - a background process that automatically detects running racing simulators and maps their telemetry data into a standardized shared memory format (`/dev/shm/SIMAPI.DAT`).

Applications such as monocoque and simmonitor are sim-agnostic, meaning they need a common data format regardless of which simulator is running. simd handles the translation from each simulator's native telemetry format into the unified SimData structure.

## What simd Does

- Automatically detects which simulator is running by monitoring processes
- Reads telemetry via shared memory or UDP depending on the simulator
- Maps simulator-specific data into the standardized SimData format
- Manages bridge processes for sims that require Wine/Proton shared memory workarounds
- Writes consolidated telemetry to `/dev/shm/SIMAPI.DAT`

## Linux Shared Memory Challenges

Running on Linux natively presents challenges. Many titles use shared memory mapped files, which Wine/Proton does not make easily available for consumption outside of the running Wine prefix.

simd contains built-in workarounds for this, such as automatically running bridge processes from [simshmbridge](https://github.com/spacefreak18/simshmbridge). Sims that support native DLL plugins (RFactor 2, LeMans Ultimate, truck simulators) can bypass this limitation entirely.

## Additional Features

- **Poke mode** - Arbitrarily set SimData fields for testing devices and displays without a running sim. See [simd poke](https://spacefreak18.github.io/simapi/simd_poke).
- **Configuration file** - Fine-grained control over simulator detection and bridge process management through [simd.config](https://github.com/Spacefreak18/simapi/blob/master/simd/conf/simd.config).
- **Systemd integration** - Can run as a user-level systemd service for automatic startup.
- **Desktop notifications** - Optional notifications when simulators are detected (can be disabled with `--nonotify`).

## Supported Simulators

See the full [supported simulators](https://spacefreak18.github.io/simapi/supportedsims) page for the compatibility matrix. simd supports 16+ simulator titles across shared memory and UDP protocols.
