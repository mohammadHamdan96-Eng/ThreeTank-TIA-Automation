# Engineering traceability matrix

This matrix links visible portfolio claims to the PLC area that implements them and to retained evidence.

| Requirement / engineering feature | PLC implementation | Evidence | FAT |
|---|---|---|---|
| automatic process is state controlled | `FB_AutoSequence` | auto-sequence screenshots, HMI cycle states | FAT-03 to FAT-10 |
| XV102 proves before P101 transfer | `FC_PermissiveLogic`, `FB_Valve`, `FB_Pump` | permissive logic + S40 watch table | FAT-05, FAT-15, FAT-27 |
| XV103 proves before P102 transfer | same equipment/permissive pattern | transfer permissive logic | FAT-08, FAT-16 |
| P101 missing RunFb becomes a pump fault | `FB_Pump`, `FB_AlarmManager` | P101 diagnostics / alarm chain | FAT-17 |
| P102 overload removes transfer demand and trips | `FB_Pump`, `FB_AlarmManager` | P102 diagnostic code 3 | FAT-18 |
| simulated plant safety-chain trip forces controlled fault state | sequence trip path + AlarmManager | S90 / FirstOut 1001 screenshot | FAT-19 |
| initiating trip remains visible | `FB_AlarmManager` first-out logic | first-out logic + HMI alarm page | FAT-14 to FAT-19 |
| manual operation remains interlocked | ModeManager + permissives + equipment FBs | manual watch tables | FAT-26, FAT-27 |
| reset does not create automatic process restart | fault/reset sequence path | reset/recovery evidence | FAT-24, FAT-25 |
| simulation reset cannot disturb active cycle | controller/simulation interface | S20 -> S40 continuity evidence | FAT-29 |
| completed process can start a fresh deliberate cycle | cycle reset + Start path | fresh S20 cycle evidence | FAT-30 |
| logical process output is distinct from final mapped layer | `DB_IO.ProcessOut`, `FC_OutputMapping`, `DB_IO.PhysicalOut` | output-mapping screenshot | architecture / FAT context |
| P101 process demand has a drive adapter | `FC_DriveP101`, `DB_SinaSpeed_P101` | SinaSpeed screenshots | integration evidence |
| selected plant data has a stable external interface | `FC_OPC_UA_Interface`, `DB_OPC_UA` | OPC UA mapping screenshots | interface evidence |
