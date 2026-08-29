# CODESYS V1 -> TIA Portal V2

The Siemens project keeps the original process idea but changes the software architecture substantially.

## 1. Single sequence owner

**V1:** sequence state could be influenced from several logic areas.

**V2:** `FB_AutoSequence` is the only writer of the automatic state. Other functions produce conditions, permissions or equipment status; they do not compete for state ownership.

## 2. Structured interfaces

**V1:** process, HMI, simulation and equipment variables were concentrated in a large flat set.

**V2:** command, feedback, configuration, alarm, simulation, drive and external-interface data are separated into dedicated DBs and UDT-based structures.

## 3. I/O abstraction

The process sequence works with symbolic/structured controller data. Hardware mapping is pushed to input/output boundaries instead of scattering absolute addresses through the state machine.

## 4. Fixed-cycle simulation

The original simulation advanced according to scan execution. V2 uses one simulation owner and a fixed 100 ms time base so the process model does not run faster or slower simply because OB1 execution changes.

## 5. Mass-balanced transfer

The process model was changed from independent tank increments to a cascade relation:

```text
dT101 = inlet - P101
dT102 = P101 - P102
dT103 = P102
```

This makes the simulated source/destination behavior internally consistent.

## 6. Reusable equipment supervision

XV101, XV102 and XV103 use the same valve supervision concept. P101 and P102 use the same pump supervision concept. Device-specific data changes; the command-feedback-fault pattern does not.

## 7. Requested vs active configuration

Operator-requested values are separated from the active controller configuration. This gives the controller a place to validate a request before it becomes the value used by the process logic.

## 8. Diagnostic context

V2 adds alarm/trip aggregation, device fault codes and first-out retention. The first-out code preserves the initiating event after the fault reaction removes commands or creates secondary alarm conditions.

## 9. HMI split by task

The single-screen CODESYS visualization evolved into WinCC Unified pages for Overview, Automatic, Manual, Alarms, Diagnostics, Trends and Settings.

## 10. Siemens integration layer

The TIA project extends the software baseline with:

- CPU 1511C-1 PN hardware configuration,
- PROFINET and ET 200SP,
- CM 1542-5 plus ET 200SP on PROFIBUS DP,
- SINAMICS G120C / Startdrive,
- Siemens `SinaSpeed` integration for P101,
- PLC-side OPC UA mapping,
- structured PLCSIM FAT evidence.
