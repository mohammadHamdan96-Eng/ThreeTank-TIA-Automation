# PLC block reference

This reference makes the project inspectable from GitHub even when TIA Portal is not open. It summarizes the role of the important blocks and points to the related evidence in the repository.

## Organization blocks

### `Main [OB1]`
- principal cyclic integration point
- calls the plant controller and output-mapping path
- connects the main DB interfaces used by the application
- evidence: `../media/control/ob1-controller-output-gate.png`

### `Startup [OB100]`
- startup initialization / deterministic initial controller state

### `CyclicInterrupt [OB30]`
- fixed cyclic execution used by the process simulation timing
- evidence: `../media/control/ob30-process-simulation.png`

## Functional blocks

### `FB_PlantController [FB10]`
Central coordination block. It receives operator/system/configuration inputs and coordinates modes, automatic sequence, equipment logic, alarm status and HMI-facing outputs.

### `FB_AutoSequence [FB6]`
- owns S0/S10/S20/S30/S40/S50/S60/S70/S90
- requests only the process actions relevant to the active state
- reacts to completion, timeout and trip conditions
- evidence: `../media/control/auto-sequence-s0-s20.png`, `../media/control/auto-sequence-s30-s90.png`

### `FB_ModeManager [FB5]`
- resolves controller operating mode
- keeps Manual and Auto mutually deliberate rather than allowing both paths to command equipment independently

### `FB_ConfigManager [FB2]`
- separates requested and active process configuration
- exposes configuration validity/error information to the controller/HMI

### `FB_Valve [FB3]`
Reusable valve supervision:
- OpenCmd demand
- OpenFb proof
- supervision timing
- state/fault indication
- fault code exposure

### `FB_Pump [FB4]`
Reusable pump supervision:
- StartCmd demand
- RunFb proof
- overload/trip condition
- running status
- failed-to-start / trip fault information

### `FB_AlarmManager [FB7]`
- combines raw alarm/trip sources
- maintains alarm/trip summary status
- records FirstOutCode
- produces active alarm count / diagnostic outputs
- evidence: `../media/control/alarmmanager-*.png`

### `FB_ProcessSimulation [FB9]`
- owns simulated tank levels
- generates simulated device feedback
- holds deliberate fault-injection paths
- runs from the fixed-cycle timing path

## Functions

### `FC_PermissiveLogic [FC5]`
Calculates whether the current process/device request is allowed. The logic includes the plant/system conditions and transfer-device dependencies such as valve proof before pump start.

### `FC_OutputMapping [FC6]`
Final boundary between process request and mapped output state. Evidence: `../media/control/output-mapping-gate.png`.

### `FC_DriveP101 [FC9]`
P101 drive adapter around Siemens `SinaSpeed`:
- maps enable, acknowledgement and speed command,
- reads AxisEnabled, actual velocity, Error, Status and DiagId,
- derives Running / AtSpeed / TripFault for the application.

### `FC_OPC_UA_Interface [FC7]`
Copies selected process/status values to `DB_OPC_UA` so the external data model remains explicit and stable.

### `FC_PROFINET_Diagnostics [FC8]`
Provides a defined PLC-side destination for PROFINET diagnostic information.
