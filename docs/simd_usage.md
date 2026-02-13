# simd usage

If you have not yet built simd, see the [simd README](https://github.com/Spacefreak18/simapi/blob/master/simd/README.md).

## Setup

### 1. Bridge binaries (for shared memory sims)

For simulators that use shared memory (AC, ACC, AMS2, PC2), you need the shared memory compatibility [binaries](https://github.com/spacefreak18/simshmbridge?tab=readme-ov-file#compilation). Sims using native plugins (RF2, LMU, truck sims) or UDP protocols do not need bridge binaries.

### 2. Configuration file

If you wish to use the automatic bridging mode (recommended), copy the config file from [here](https://github.com/Spacefreak18/simapi/blob/master/simd/conf/simd.config) to `~/.config/simd/simd.config`.

The config file uses libconfig format and defines per-simulator settings:

```
sims = (
    {
        name         = "AssettoCorsa";
        gameid       = 244210;              // Steam AppID
        launchexe    = "AssettoCorsa.exe";
        liveexe      = "acs.exe";           // Runtime executable
        bridgedelay  = 5;                   // Seconds before bridge timeout (optional)
        simapi       = 1;                   // SimulatorAPI enum from simapi.h
    },
);
```

### 3. Steam launch command

Add the appropriate [bridge](https://github.com/spacefreak18/simshmbridge) exe to your Steam launch command:

```
SIMD_BRIDGE_EXE=~/path/to/acbridge.exe %command%
```
or
```
SIMD_BRIDGE_EXE=~/path/to/pcars2bridge.exe %command%
```

It is recommended to temporarily back up your launch command and use this minimal launch command first to get it working.

 
## Alternate brdige launch method: Using linux-sim-launcher

[linux-sim-launcher](https://github.com/giantorth/linux-sim-launcher) is a Python utility that can manage the launching and cleanup of shared memory bridges (and other sim tools like SimHub and Opentrack) alongside your Steam games. It automatically builds the [simshmbridge](https://github.com/spacefreak18/simshmbridge) binaries on first use and handles their lifecycle, setting `SIMD_BRIDGE_EXE` is not required with this method.


#### Steam launch command examples

For Assetto Corsa:
```
~/.local/bin/sim-launcher --acbridge %command%
```

For Automobilista 2 or Project Cars 2:
```
~/.local/bin/sim-launcher --pc2bridge %command%
```

## Running simd

### Foreground mode (recommended for initial testing)

```
simd --nodaemon --nomemmap -vv
```

### Daemon mode

```
simd
```

### As a systemd user service

If you installed with `cmake --install` to `~/.local`, simd can run as a systemd user service:

```bash
systemctl --user daemon-reload
systemctl --user enable --now simd
```

To manage the service:
```bash
systemctl --user status simd      # Check status
systemctl --user stop simd        # Stop the service
systemctl --user restart simd     # Restart the service
journalctl --user -u simd -f      # View logs
```

## Command line options

| Flag | Long form | Description |
| ---- | --------- | ----------- |
| `-v` | `--verbose` | Increase logging verbosity (use up to 2x: `-vv`) |
| `-n` | `--nodaemon` | Run in foreground (no daemon fork) |
| `-h` | `--nomemmap` | Disable automatic memory mapping for AC/PC2 sims |
| `-a` | `--nobridge` | Disable automatic bridge process launching |
| `-s` | `--nonotify` | Disable desktop notifications |
| `-u` | `--udp` | Force UDP protocol on all sims that support it |
| `-p` | `--poke` | Poke a SimData field (requires `-t`) |
| `-t` | `--target` | Target value for poke operation |
| | `--help` | Show help and exit |
| | `--version` | Show version and exit |

## Library path

If you get an error like:
```
simd: error while loading shared libraries: libsimapi.so.1: cannot open shared object file: No such file or directory
```

Run this first:
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
```

For user-level installs (`~/.local`), the systemd service file sets this automatically.

## How memory mapping works

simd has a built-in workaround to automatically create memory mapped files for Assetto Corsa and Project Cars 2 based sims, so a workaround such as createsim isn't needed. However, a helper process running in the Wine/Proton environment (simshmbridge) is still needed. The `--nomemmap` (`-h`) option disables this workaround.

## Troubleshooting

Start simd before launching any games. For initial debugging, run in foreground with verbose logging:

```bash
simd --nodaemon -vv
```

Check shared memory files while a sim is running:

```bash
# Assetto Corsa sims
hexdump /dev/shm/acpmf_physics

# Automobilista 2 / Project Cars 2
hexdump /dev/shm/\$pcars2\$

# SimAPI output (should have non-zero data when a sim is active)
hexdump /dev/shm/SIMAPI.DAT
```

If there is no data in the sim-specific files while the sim is running, there is likely a problem with your Steam launch command or the path to your [simshmbridge](https://github.com/spacefreak18/simshmbridge) exe.

If you have non-zero data in these files, simd is running and doing its job. Running monocoque or simmonitor should work at that point.
