# FAT validation index

The public index keeps the original FAT identifiers for traceability and lists only the retained executed cases. The screenshot-rich PDF contains the individual records.

| FAT | Test | Result |
|---|---|---|
| FAT-01 | Cold startup | PASS |
| FAT-02 | Invalid Auto start | PASS |
| FAT-03 | Valid Auto start | PASS |
| FAT-04 | T101 fill | PASS |
| FAT-05 | Valve before P101 | PASS |
| FAT-06 | S40 transfer | PASS |
| FAT-07 | S40 completion | PASS |
| FAT-08 | Valve before P102 | PASS |
| FAT-09 | S60 cascade | PASS |
| FAT-10 | Normal completion | PASS |
| FAT-11 | Stop in S20 | PASS |
| FAT-12 | Stop in S40 | PASS |
| FAT-13 | Stop in S60 | PASS |
| FAT-14 | XV101 feedback failure | PASS |
| FAT-15 | XV102 feedback failure | PASS |
| FAT-16 | XV103 feedback failure | PASS |
| FAT-17 | P101 run-feedback failure | PASS |
| FAT-18 | P102 overload | PASS |
| FAT-19 | Simulated plant safety-chain trip | PASS |
| FAT-24 | Reset / persistent field-fault behavior | VERIFIED |
| FAT-25 | Fault recovery | PASS |
| FAT-26 | Manual XV101 command | PASS |
| FAT-27 | Manual pump interlock | PASS |
| FAT-29 | ResetSimulation during cycle | PASS |
| FAT-30 | Cycle restart | PASS |

`FAT-24` is kept as **VERIFIED** because its retained result documents the implemented controlled reset/restart behavior rather than silently rewriting the original draft acceptance line.

**Evidence report:** [ThreeTank_FAT_Validation_Report.pdf](ThreeTank_FAT_Validation_Report.pdf)
