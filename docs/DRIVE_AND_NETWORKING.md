# Drive, networking and external interface

## PROFINET

The main PN/IE architecture contains:

- CPU 1511C-1 PN,
- WinCC Unified HMI,
- ET 200SP PROFINET station,
- SINAMICS G120C PN.

The process application works through structured I/O data and mapping layers so network/hardware addresses do not become part of the automatic state-machine design.

`FC_PROFINET_Diagnostics [FC8]` and `DB_PROFINET_Diag` provide a defined PLC-side diagnostic boundary.

## PROFIBUS DP

The CPU rack contains a **CM 1542-5** module. The configured DP side includes an ET 200SP station with **IM 155-6 DP HF** and I/O modules. This gives the project a second Siemens fieldbus architecture in addition to PROFINET.

## SINAMICS G120C / Startdrive

The G120C is part of the TIA project and the P101 application interface is implemented in `FC_DriveP101 [FC9]` using Siemens `SinaSpeed` with instance DB `DB_SinaSpeed_P101`.

### Command side

The drive block receives controller-level values such as:

- EnableAxis,
- AckError,
- SpeedSp,
- RefSpeed,
- the configured drive/telegram hardware reference.

### Status side

The returned data includes:

- AxisEnabled,
- ActVelocity,
- Error,
- Status,
- DiagId.

`FC_DriveP101` then converts those block-level outputs into application states. The retained logic includes:

- Running qualification from command + AxisEnabled + actual speed,
- AtSpeed qualification around the requested speed with a tolerance band,
- TripFault qualification from the drive status cases used in the project.

This keeps the state machine and pump logic independent from telegram/status-word interpretation.

## OPC UA

`FC_OPC_UA_Interface [FC7]` copies selected values to `DB_OPC_UA`.

The mapped interface includes:

- three tank levels,
- operating mode,
- sequence step/state,
- cycle-ready/fault state,
- alarm/trip information,
- P101 command/status values.

The external data model is therefore selected intentionally rather than exposing arbitrary internal controller DB members.
