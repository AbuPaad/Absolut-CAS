# Cfx-port — the drop-in board

KiCad project for the NumOS drop-in PCB. It replaces the original board inside a stock Casio
fx-82 / fx-991 with an ESP32-S3, a colour screen interface and a proper keypad matrix — reusing
the calculator's own shell and buttons.

Firmware lives in `../../../firmware/AbsolutOS`. This folder is the hardware.

> [!WARNING]
> **Schematics only — the PCB files in this repo are stale.**
> Every schematic (`*.kicad_sch`) here is current and complete. Every board file (`*.kicad_pcb`)
> is out of date: it does not match the last fabricated board and will not work as drawn. The PCB
> is being reworked and will be pushed here soon — do not fabricate from anything in this folder
> until then.

---

## Components

- **U8 — ESP32-S3-WROOM-2** (LCSC `C2913202`). Does everything: keypad scanning, screen, Wi-Fi.
- **U7 — TCA9555DBR** (16-bit I²C expander, SSOP-24). Reads the keypad matrix, so the whole
  keypad costs the MCU two pins instead of fifteen.
- **S1 — 40-pin 0.5 mm FPC connector** (`CONN40_AFC01-S40FCA-00_JUS`; Omron `XF2M-4015-1A` in the
  symbol library). This is what the ILI9341 3.2" 320×240 panel plugs into. The panel itself is
  not on this board.
- **U5 — TP4056-42-ESOP8.** Li-ion charge management off the USB-C port.
- **Q1 — DMP3099L** P-channel MOSFET, plus a battery protection pair on SOT-23 (the two parts
  still valued `idk` in the schematic — they exist, they just were never named).
- **LDO — TPS73733DCQR.** Regulates the rails.
- **J2 — USB-C 16P receptacle**, USB 2.0 only, `5.1k` CC pull-downs on R3/R19.
- **J4 — JST PH 2-pin.** Battery connector.
- **J3 — Micro SD socket** (footprint `CONN_TF-01A_HRO`). Fitted. Doesn't work. See status.
- **B1/B2** — two tactile push buttons. **D5/D6** — LEDs. **D4** — 1N5819. **D1–D3** — SOD-123.
- Passives are all 0603.
- **No camera and no expansion header on this revision.** `sym-lib-table` points at an OV2640
  symbol library sitting in somebody's Downloads folder, but nothing from it is placed.

## The board itself

- **4 layers**: `F.Cu`, `In1.Cu` (power), `In2.Cu` (signal), `B.Cu`. The inner layers are
  actually routed — this is not a 2-layer board with the inner layers declared and unused.
- 1.6 mm.
- **Outline: 65 × 136 mm** as drawn in the board file.
- **Every component sits on `B.Cu`** — the whole board mounts bottom-side-up behind the keypad.
- Last DRC run: **0 violations**. Detail in the reports section below.

## What it can actually do

- Runs NumOS: CAS maths, natural display, graphing, apps, and a Game Boy emulator.
- A real 320×240 colour screen instead of a 7-segment LCD.
- Wi-Fi on board. The AI and notes features need it.
- Rechargeable Li-ion over USB-C. Untethered.
- Keypad-only input. No touch.
- OTA firmware updates are the eventual goal.
- Drop-in: same shell, same buttons, same feel.

---

## Files

| Path | What it is |
|---|---|
| `Cfx-port.kicad_pro` | Project settings. |
| `Cfx-port.kicad_sch` | Root schematic. Just holds the three sheets. |
| `Cfx-port.kicad_pcb` | The board. 95 footprints. |
| `keypad.kicad_sch` | Keypad matrix and the TCA9555 expander. |
| `power-ic.kicad_sch` | Charger, LDO, protection, USB-C. |
| `esp32-s3-pinout.kicad_sch` | Module pinout and connectors. |
| `production/` | Fabrication outputs. See below. |
| `jlcpcb/` | Order output folder. `gerber/` and `production_files/` are **empty**. |
| `plots/` | `Cfx-port__Assembly.pdf`. |
| `packages3D/` | 3D models. `Downloads/` holds the vendor STEP files. |
| `Cfx-port-backups/` | Dated zips. The project's own footprints live here in `Library.pretty`. |
| `KicadConductivePads/` | Third-party pad footprints. Own LICENSE, own nested `.git`. |
| `.history/` | KiCad autosaves. Contains its own nested `.git`. |
| `.portability/` | KiCad's own backup manifest. |
| `DRC.rpt` / `ERC.rpt` | Design rule and electrical rule reports. |
| `Cfx-port-all-pos.csv` | Pick-and-place data. |
| `fp-lib-table` / `sym-lib-table` | Footprint and symbol library pointers. |
| `NeoFX.kicad_sym`, `sch_data.json`, `fp-info-cache`, `fabrication-toolkit-options.json` | Support and scratch. |
| `untitled.kicad_sch`, `Cfx-port.kicad_sch_old` | Old copies, not part of the project. |

Everything else in this folder is KiCad scratch — `~*.lck` locks, zero-byte `tmp*.rpt`,
`#auto_saved_files#`, `.step`, and `model.stl.[sS][tT][eE][pP]` (a broken filename from a shell
glob that got committed). Safe to ignore.

### Fabrication outputs

- `production/bom.csv` — parts list with LCSC part numbers.
- `production/designators.csv` — every designator, for assembly.
- `production/*.svg` — the gerbers, exported as SVG: copper, mask, paste, silkscreen, edge cuts.
- `production/netlist.ipc` — netlist.
- `production/Cfx-port.zip` — the packaged set.

Generated **2026-09-02**. This is what went to the fab.

Readable versions live one level up: `../Docs/Matrix.pdf` for the keypad matrix drawing,
`../../../Docs/schematic.pdf` for the flattened schematic, and `../../../Reference/` for the
vendor datasheets.

---

## Status — v1

- Board ordered **August 2026**, delivered, built and running.
- **Working:** display, keypad, boot, calculator maths and graphing, the Game Boy emulator, the
  new Casio font, Wi-Fi.
- The display was DOA on the first build and has since been fixed.
- **Broken:** the SD slot, and only that. Fitted and dead. Board decision 2026-10-01 — no SD in
  v1, it comes back in v2.
- v2 is roughly a month out.

## Known issues and cleanup before the next order

- **The reports are stale.** DRC and ERC are dated 2026-07-31 / 2026-08-02, older than the last
  board edit (2026-10-03). Re-run both before trusting them.
- Last DRC: 0 violations, but **59 unconnected pads**.
- Last ERC: 0 errors, 5 warnings — two library symbol mismatches and three power pins not driven
  by any output power pin.
- **Two footprints are still unannotated** (`REF**`) and two SOT-23 parts are valued `idk`.
  Real parts, never named.
- **No title block in the project**, so no revision or date is stamped anywhere in the files.
- **`U7` is a `TCA9555DBR`, but the schematic symbol and the BOM line still read
  `TCA9535DBR`.** The 9535 and 9555 are pin-compatible siblings, so it works, but the BOM should
  name the part actually fitted before it goes out again.
- `bom.csv` and `designators.csv` still list **J3** even though the current board file does not
  place its footprint. The two have drifted apart.
- **`sym-lib-table` points at absolute paths** (`/home/fih/Downloads/...`). This breaks for
  anyone who is not on that machine. Fix before publishing.
- `fp-lib-table` points the `Library` entry at `Cfx-port-backups/Library.pretty`. The project's
  own footprints sitting in a backups folder is fragile.
- The firmware repo keeps its own hardware contract
  (`../../../firmware/AbsolutOS/docs/PRODUCTION_PCBA_HARDWARE_CONTRACT.md`). That document is
  written against a different revision and disagrees with the files here on the module
  (`WROOM-1U` vs `WROOM-2`) and the layer count (two-layer vs four). **The files in this folder
  are current.**

---

## Credit

Keypad pad footprints come from **KicadConductivePads** by nataliethenerd — public domain
(Unlicense), sizing loosely based on the Game Boy Color and Pocket pads. Everything else in this
folder is AbuPaad's.
