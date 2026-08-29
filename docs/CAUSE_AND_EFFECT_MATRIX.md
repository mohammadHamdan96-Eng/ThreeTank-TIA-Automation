# Cause-and-effect matrix

This matrix summarizes the fault and recovery behavior that was exercised during the ThreeTank PLCSIM FAT. It is intentionally limited to behavior supported by the retained test records.

| Condition / stimulus | Controller reaction | Device / process consequence | Diagnostic context | FAT evidence |
|---|---|---|---|---|
| Simulated plant safety-chain input becomes unhealthy during operation | sequence enters S90 and normal process requests are removed | valves/pumps no longer receive normal process demand | FirstOut `1001` | FAT-19 |
| XV101 is demanded but OpenFb is not proved | valve supervision times out and faults | fill path is stopped; sequence enters S90 | XV101 device fault + FirstOut `1401` | FAT-14 |
| XV102 is demanded but OpenFb is not proved | P101 remains inhibited; valve supervision times out and faults | T101→T102 transfer cannot start; sequence enters S90 | XV102 device fault + FirstOut `1402` | FAT-15 |
| XV103 is demanded but OpenFb is not proved | P102 remains inhibited; valve supervision times out and faults | T102→T103 transfer cannot start; sequence enters S90 | XV103 device fault + FirstOut `1403` | FAT-16 |
| P101 is demanded but RunFb is not proved | pump start supervision times out and faults | P101 is treated as unavailable; sequence enters S90 | P101 failed-to-start fault + FirstOut `1501` | FAT-17 |
| P102 overload is injected | pump demand is removed and trip logic becomes active | P102 stops / remains inhibited; sequence enters S90 | P102 trip + FirstOut `1502` | FAT-18 |
| Operator Stop during S20, S40 or S60 | active automatic operation is terminated in a controlled path | process demand is removed without creating a process trip | no new first-out trip | FAT-11, FAT-12, FAT-13 |
| Manual P101 request without proved XV102 path | permissive remains false | P101 start is inhibited | HMI permissive/status remains unavailable | FAT-27 |
| XV102 path becomes proved in Manual mode | P101 permissive becomes available | manual pump path can proceed under the existing equipment rules | HMI `P101 Permitted` becomes active | FAT-27 |
| Fault cause cleared followed by Reset | latched fault state clears and controller returns to controlled idle/off state | no automatic process restart | alarm/trip memory clears according to reset logic | FAT-25 |

## Reset philosophy

Reset is an acknowledgement/recovery action, not an automatic restart command. After recovery, a deliberate Start is required before the process sequence runs again.

The test history also retains the implemented behavior for a persistent injected field fault (FAT-24): once S90 removes the device command, the instantaneous raw fault condition can disappear even though the injection remains selected. Reset may therefore clear the latch, but the same field problem is detected again when the affected device is demanded on the next deliberate start. That behavior is recorded as **VERIFIED** rather than rewriting the original draft acceptance wording.
