# simd

Cross-sim telemetry mapping daemon.

This daemon automatically detects the currently running simulator and maps its telemetry data (speed, gear, RPMs, etc.) into a common memory mapped file for use by other programs such as monocoque or simmonitor. The output is written to `/dev/shm/SIMAPI.DAT`.

## Supported Games

See [supported sims](docs/supportedsims.md) for the full list of supported simulators and their feature levels. Currently 16+ titles are supported including Assetto Corsa, RFactor 2, Automobilista 2, LeMans Ultimate, Richard Burns Rally, Wreckfest 2, and more.

## Dependencies

- [simapi](https://github.com/spacefreak18/simapi) (libsimapi.so)
- [libuv](https://libuv.org/) - event loop
- [yder](https://github.com/babelouest/yder) - logging
- [libconfig](https://hyperrealm.github.io/libconfig/) - configuration parsing
- argtable3 or argtable2 - CLI argument parsing

## Building

simd and simapi are both available in the [AUR](https://aur.archlinux.org/packages/simd-git).
Be sure to install [simapi](https://aur.archlinux.org/packages/simapi-git) first.

To build manually, first compile and install simapi using the instructions at the root of the repo, then compile simd with cmake:

```bash
cmake -B build
cmake --build build
```

## Installation

### System-wide

```bash
sudo cmake --install build
```

### User-level (recommended)

```bash
cmake -B build -DCMAKE_INSTALL_PREFIX=$HOME/.local
cmake --build build
cmake --install build
```

This installs:
- Binary to `~/.local/bin/simd`
- Systemd service to `~/.config/systemd/user/simd.service`
- Config file to `~/.config/simd/simd.config`

To enable as a systemd user service:

```bash
systemctl --user daemon-reload
systemctl --user enable --now simd
```

To manage the service:
```bash
systemctl --user status simd    # Check status
systemctl --user stop simd      # Stop the service
systemctl --user restart simd   # Restart the service
journalctl --user -u simd -f    # View logs
```

To disable installation of specific components:
```bash
cmake -B build -DCMAKE_INSTALL_PREFIX=$HOME/.local \
    -DINSTALL_SYSTEMD_SERVICE=OFF \
    -DINSTALL_DEFAULT_CONFIG=OFF
```

## Usage

If you wish to use the automatic bridging mode, first copy the config file from [here](https://github.com/Spacefreak18/simapi/blob/master/simd/conf/simd.config) to `~/.config/simd/simd.config` and add the appropriate [bridge](https://github.com/spacefreak18/simshmbridge) exe to your Steam launch command:

```
SIMD_BRIDGE_EXE=~/path/to/acbridge.exe %command%
```
or
```
SIMD_BRIDGE_EXE=~/path/to/pcars2bridge.exe %command%
```

To run in foreground with verbose logging (recommended for testing):
```
simd --nodaemon --nomemmap -vv
```

To run as a daemon:
```
simd
```

If you get an error like `simd: error while loading shared libraries: libsimapi.so.1: cannot open shared object file: No such file or directory`, run:
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
```

For more information see the [simd usage documentation](https://spacefreak18.github.io/simapi/simd).
