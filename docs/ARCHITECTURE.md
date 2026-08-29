# PLC software architecture

## Design objective

The V2 program was organized so that every important state/data domain has a clear owner. The automatic sequence does not write hardware outputs directly, the HMI does not own process state, and the simulation does not bypass the normal equipment logic.

## Cyclic organization

### `Main [OB1]`

OB1 is the main integration point. At a high level it:

1. obtains the mapped/selected controller inputs,
2. executes `FB_PlantController`,
3. updates HMI/controller status,
4. applies the final output mapping/gate,
5. calls the additional interface/diagnostic logic used by the project.

### `Startup [OB100]`

OB100 provides startup initialization so the application does not depend on arbitrary retained runtime values after a controller start.

### `CyclicInterrupt [OB30]`

OB30 supplies the fixed timing used by the process model. The simulation therefore advances on a known time base rather than once per variable OB1 scan.

## Core functional blocks

| Block | Ownership / role |
|---|---|
| `FB_PlantController [FB10]` | Coordinates the process-facing interfaces and sub-functions |
| `FB_ModeManager [FB5]` | Resolves Off / Manual / Auto mode |
| `FB_AutoSequence [FB6]` | Only writer of the automatic state |
| `FB_ConfigManager [FB2]` | Handles requested vs active configuration |
| `FB_Valve [FB3]` | Owns reusable valve supervision state |
| `FB_Pump [FB4]` | Owns reusable pump supervision state |
| `FB_AlarmManager [FB7]` | Owns alarm/trip aggregation and first-out memory |
| `FB_ProcessSimulation [FB9]` | Owns simulated process levels and feedback |

## Main FC boundaries

| FC | Boundary |
|---|---|
| `FC_InputMapping [FC1]` | Hardware/selected input -> controller data |
| `FC_HMIStatusMapping [FC4]` | Controller state -> HMI status interface |
| `FC_PermissiveLogic [FC5]` | Conditions -> equipment/process permissions |
| `FC_OutputMapping [FC6]` | Logical process request -> mapped output layer |
| `FC_OPC_UA_Interface [FC7]` | Internal state -> stable external data model |
| `FC_PROFINET_Diagnostics [FC8]` | Network diagnostic data -> diagnostic DB |
| `FC_DriveP101 [FC9]` | P101 process demand <-> G120C/SinaSpeed data |

## Command and feedback path

```text
WinCC / requested configuration
            |
            v
      DB_HMI / DB_Config
            |
            v
      FB_PlantController
       /      |       \
      /       |        \
 Mode   AutoSequence   Permissives
                 \       /
                  \     /
                 Valve / Pump FBs
                       |
                       v
                DB_IO.ProcessOut
                       |
                       v
                FC_OutputMapping
                       |
                       v
               DB_IO.PhysicalOut
```

Feedback follows the reverse conceptual direction through input/selected feedback and equipment supervision. The state machine consumes *proved* plant/device conditions instead of assuming that a requested command succeeded.

## Data ownership

| Domain | Owner / principal interface |
|---|---|
| automatic state | `FB_AutoSequence` |
| simulated tank levels and feedback | `FB_ProcessSimulation` / `DB_Simulation` |
| alarm/trip and first-out memory | `FB_AlarmManager` / `DB_Alarms` |
| logical process commands | process/device logic -> `DB_IO.ProcessOut` |
| final mapped outputs | `FC_OutputMapping` -> `DB_IO.PhysicalOut` |
| operator boundary | `DB_HMI` |
| external OPC UA boundary | `DB_OPC_UA` |

## Why the output layer is separate

The distinction between a logical command and a mapped output was useful both architecturally and during FAT. A test can establish that the sequence/device logic requested an actuator, then independently inspect whether the final output layer accepted that request under the current simulation/system conditions.
