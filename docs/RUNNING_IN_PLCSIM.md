# Running the ThreeTank project in PLCSIM

## Required project

Download the latest TIA Portal archive from the repository Releases page:

https://github.com/mohammadHamdan96-Eng/ThreeTank-TIA-Automation/releases/latest

Expected asset:

`ThreeTank_TIA_V2_FINAL_2026-08-29.zap20`

Restore/open the archive with TIA Portal V20.

## Startup workflow

1. Compile the PLC software.
2. Compile the WinCC Unified project.
3. Start the PLCSIM environment used by the project and load the PLC.
4. Put the simulated CPU in RUN.
5. Start WinCC Unified simulation/runtime.
6. Confirm the controller/System Ready state and simulation path.
7. Select Auto from the HMI.
8. Issue Start.
9. Follow the sequence and tank levels through Overview/Automatic/Trends or the watch table.

A normal cycle progresses through the major states:

`S20 Fill T101 -> S40 T101 to T102 -> S60 T102 to T103 -> S70 Complete`

## Manual testing

Select Manual from the HMI and use the device controls. Manual requests still pass through the PLC permissive/equipment path; a pump request is not a direct output override.

## Fault injection

The simulation DB contains deliberate injection points used by the FAT, including valve feedback, pump feedback/overload and plant safety test conditions. Clear an injected condition as required by the individual FAT before validating recovery/restart.

## Useful observation points

- `DB_HMI.Status.Sequence.Step`
- `DB_IO.ProcessOut.*`
- `DB_IO.PhysicalOut.*`
- selected valve/pump feedback
- `DB_Alarms.Alarm.AnyTrip`
- `DB_Alarms.Alarm.FirstOutCode`
- tank level values

> **PLCSIM note:** The CPU configuration uses access protection.  
> When TIA Portal requests the CPU password during download to PLCSIM, use: `YOUR_PASSWORD_HERE`
