# Engineering documentation index

The top-level README tells the project story. The documents in this folder are the technical reference layer behind it.

| Document | Purpose |
|---|---|
| [ARCHITECTURE.md](ARCHITECTURE.md) | PLC execution model, block ownership and data flow |
| [MIGRATION_AND_DESIGN_DECISIONS.md](MIGRATION_AND_DESIGN_DECISIONS.md) | Why the TIA version differs from the CODESYS baseline |
| [PLC_BLOCK_REFERENCE.md](PLC_BLOCK_REFERENCE.md) | Block-by-block responsibility and important interfaces |
| [EQUIPMENT_AND_DIAGNOSTICS.md](EQUIPMENT_AND_DIAGNOSTICS.md) | Valve/pump supervision, permissives, alarms and recovery |
| [CAUSE_AND_EFFECT_MATRIX.md](CAUSE_AND_EFFECT_MATRIX.md) | Fault stimulus, control reaction, retained diagnostics and FAT traceability |
| [DRIVE_AND_NETWORKING.md](DRIVE_AND_NETWORKING.md) | PROFINET, PROFIBUS, G120C, SinaSpeed and OPC UA |
| [ENGINEERING_TRACEABILITY.md](ENGINEERING_TRACEABILITY.md) | Requirement/feature -> implementation -> FAT evidence mapping |
| [RUNNING_IN_PLCSIM.md](RUNNING_IN_PLCSIM.md) | Practical simulation startup and test workflow |
| [FAT_VALIDATION.md](FAT_VALIDATION.md) | Public FAT result index |
| [ThreeTank_FAT_Validation_Report.pdf](ThreeTank_FAT_Validation_Report.pdf) | Screenshot-backed FAT records |
| [ENGINEERING_SCOPE.md](ENGINEERING_SCOPE.md) | Short validation-scope statement |

## Browser-readable implementation excerpts

Selected SCL excerpts are available in [`../plc-excerpts/`](../plc-excerpts/) for quick GitHub review. The `.zap20` archive published through GitHub Releases remains the authoritative editable project.
