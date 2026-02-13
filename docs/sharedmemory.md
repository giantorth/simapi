# Shared Memory

Shared memory mapped files are a high-performance method of inter-process communication, used often in game development for telemetry output. To learn more about shared memory visit this [page](https://learn.microsoft.com/en-us/dotnet/standard/io/memory-mapped-files), and to learn about different methods of telemetry output visit this [page](https://docs.departedreality.com/dr-sim-manager/development/telemetry-outputs-overview).

## The Linux Challenge

For Linux gaming, Proton/Wine does not make shared memory mapped files available to native Linux processes [yet](https://bugs.winehq.org/show_bug.cgi?id=54015). This means a native application like monocoque cannot directly read telemetry that a Windows game writes to shared memory inside its Wine prefix.

## Solutions

There are two approaches to work around this limitation:

### Native DLL Plugins (no bridge needed)

Some simulators support native DLL plugins that can be compiled with Winelib. These plugins run inside the Wine/Proton environment but write to memory mapped files accessible by native Linux processes. Simulators with native plugin support:

- **RFactor 2** / **LeMans Ultimate** - via [rF2SharedMemoryMapPlugin_Wine](https://github.com/schlegp/rF2SharedMemoryMapPlugin_Wine). See the [setup guide](https://spacefreak18.github.io/simapi/rfactor2).
- **American Truck Simulator** / **Euro Truck Simulator 2** - via [scs-sdk-plugin](https://github.com/alxwk/scs-sdk-plugin)

### Bridge Processes (simshmbridge)

For simulators without native plugin support (Assetto Corsa, Automobilista 2, Project Cars 2), a bridge process runs inside the Wine/Proton environment, reads the game's shared memory, and writes it to a location accessible by native processes. See [simshmbridge](https://github.com/spacefreak18/simshmbridge).

### UDP Protocol

Some simulators support UDP telemetry output, which bypasses the shared memory issue entirely. These sims (Live For Speed, BeamNG, Dirt Rally 2, F1 2022, Wreckfest 2) send data over the network and simd receives it directly.

## Recommended Setup

The recommended approach is to install [simd](https://spacefreak18.github.io/simapi/simd), which handles bridge process management and memory mapping automatically.
