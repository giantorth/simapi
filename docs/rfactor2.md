# RFactor 2 / LeMans Ultimate

RFactor 2 and LeMans Ultimate (which uses the same engine) have native Linux shared memory support through a Winelib-compiled plugin. This means no bridge process is required -- the plugin writes directly to shared memory accessible by native Linux processes.

## Plugin Installation

Download the Linux shared memory plugin from: [rF2SharedMemoryMapPlugin_Wine](https://github.com/schlegp/rF2SharedMemoryMapPlugin_Wine)

Pre-built binaries are available in the [build](https://github.com/schlegp/rF2SharedMemoryMapPlugin_Wine/tree/master/build) directory.

### RFactor 2

Place the plugin DLL in your RFactor 2 plugins directory:

```
~/.steam/steam/steamapps/common/rFactor 2/Bin64/Plugins/
```

### LeMans Ultimate

Place the plugin DLL in your LeMans Ultimate plugins directory:

```
~/.steam/steam/steamapps/common/Le Mans Ultimate/Bin64/Plugins/
```

## How It Works

The plugin is compiled with Winelib, which allows it to run inside the Wine/Proton environment while writing to memory mapped files that are accessible to native Linux processes. This bypasses the typical Wine shared memory limitation entirely.

Once the plugin is installed and the game is running, simd will automatically detect the simulator and read telemetry from the shared memory files.

## Support Level

Both RFactor 2 and LeMans Ultimate have **Platinum** support in SimAPI, meaning all current monocoque and simmonitor features are available.
