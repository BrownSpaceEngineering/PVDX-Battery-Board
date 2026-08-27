# Battery-Board

Battery subsystem board for PVDX — connects the battery stack, solar cells/solar PCB, heaters, and thermistors.

## Status
- Design phase: fabrication
- Current rev: rev-1.1
- Contributors: Jayla Hsiung, Kelly Lin, Finn Kehoe, Dean Zweiman

## System overview

The Battery Board is primarily structural/routing rather than active. It has no onboard charging or regulation ICs (that logic lives on EPS). Its job is to tie the battery stack into the rest of the power subsystem: routing to the regular (non-perovskite) solar cells and solar PCB, driving the battery heaters, and carrying thermistor signals back for battery temperature monitoring.

**Board sheets:**

- **Heater**: battery heater circuit.
- **Connectors**: harness/pinout for battery, solar, heater, and thermistor connections. These are being hardwired rather than connectorized — the footprints (pad patterns) stay on the board for wire attachment points, but the 3D models are hidden (Show unchecked in Footprint Properties → 3D Models) so they don't appear in the STEP export.

### Repo layout

```
PVDX-Battery-Board/
├── PVDX-Battery-Board.kicad_pro / .kicad_sch / .kicad_pcb
├── fp-lib-table
├── schematics/
│   ├── heater.kicad_sch
│   └── connectors.kicad_sch
├── libs/
│   ├── footprints.pretty/
│   ├── symbols/
│   └── 3d_models/
└── manufacturing/
    └── rev-<N>_<date>/
        ├── gerbers/
        ├── bom/
        └── plots/
```

**Why hardwire instead of connectors:** simplifies the harness at the battery/heater interface and removes a failure point (connector) from a joint that doesn't need to be serviceable in flight. KiCad has no dedicated "exclude from STEP" option, so per-footprint 3D model visibility is the mechanism used to keep the pad pattern without the connector body in mechanical exports.

## Key design notes

- **Thermistor (NXFT15XV103FEAB025):** custom footprint, kept in `libs/footprints.pretty/`. The original vendor export had a stray space in the filename — fixed during the repo reorg.
- **Connector footprints, 3D models hidden:** see above — keep an eye on this after any "Update PCB from Schematic" footprint refresh, which can reset visibility to the library default and re-show the model.
- **`PVDX_BatteryBoard_Board.kicad_sch`** was an orphaned, blank sheet not referenced anywhere in the hierarchy — deleted during the reorg.

## Manufacturing history
| Rev | Date | Notes | Location |
|-----|------|-------|----------|
| 1.1 | 8/22/26 | Current gerber set | `manufacturing/rev-1.1_2026-08-22/` |

## Open items
- Fill in BOM for rev-1.1 (`manufacturing/rev-1.1_2026-08-22/bom/` currently empty)
