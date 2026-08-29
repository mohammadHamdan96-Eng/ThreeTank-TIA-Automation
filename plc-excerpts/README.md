# PLC implementation excerpts

These files expose small, browser-readable excerpts from the implemented TIA Portal V20 blocks that are discussed in the main README. They were transcribed from the retained TIA implementation views included in `media/control/`; they are **not complete block exports** and are not intended to replace the final `.zap20` archive.

The excerpts are deliberately limited to two architecture decisions that are useful to inspect directly on GitHub:

- `FB_AutoSequence_trip_priority_excerpt.scl` — trip priority, controlled Stop and the initial Auto-start transition.
- `FB_AlarmManager_firstout_excerpt.scl` — reset acceptance/rejection and deterministic FirstOut mapping.

The authoritative editable project is the `.zap20` archive published through the repository's [GitHub Releases](https://github.com/mohammadHamdan96-Eng/ThreeTank-TIA-Automation/releases/latest). If full TIA V20 textual exports are added later, they should be kept as generated exports rather than reconstructed by hand.
