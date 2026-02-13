# simd poke

simd has a mode that allows setting (or "poking") arbitrary SimData fields. This is useful for testing devices and displays in monocoque or simmonitor without needing a running simulator.

The simd daemon must already be running for poke to work.

## Activating test data

To start sending test data, two fields must be set:

```bash
simd -p SimData_simstatus -t 2
simd -p SimData_simon -t 1
```

Both fields must be set before monocoque or simmonitor will detect active data.

## Setting telemetry values

Once active, you can set any SimData field:

```bash
# Set speed (km/h)
simd -p SimData_velocity -t 185

# Set RPMs
simd -p SimData_rpms -t 7500

# Set gear
simd -p SimData_gear -t 4
```

The naming scheme is `SimData_[fieldname]` where fieldname corresponds to fields defined in `simdata.h`.

## Stopping test data

To stop the simulated data:

```bash
simd -p SimData_simstatus -t 0
simd -p SimData_simon -t 0
```
