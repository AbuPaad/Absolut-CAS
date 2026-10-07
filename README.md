# Absolut-CAS — the drop-in board

KiCad project for the NumOS Casio Port. It replaces the original board inside a stock Casio
fx-82 / fx-991 with an ESP32-S3, a colour screen interface and a oem compatible keypad matrix — reusing
the calculator's own shell and buttons.

---

## Components

- **U8 — ESP32-S3-WROOM-1**. Does everything/core MCU.
- **U7 — TCA9555DBR** (16-bit I²C expander, SSOP-24). Reads the keypad matrix, so the whole
  keypad costs the MCU two pins instead of fifteen.
- **S1 — 40-pin 0.5 mm FPC connector**. This is what the ILI9341 2.4" 320×240 panel plugs into.
- **U5 — TP4056-42-ESOP8.** Li-ion charge management for battery.
- **Q1 — DMP3099L** P-channel MOSFET, plus a battery protection pair on SOT-23. Power Path 
- **LDO — TPS73733DCQR.** Regulates the rails.
- **J2 — USB-C 16P connector**, USB 2.0 only, `5.1k` CC pull-downs on R3/R19.
- **J4 — JST PH 2-pin.** Battery connector.
- **J3 — Micro SD socket** (footprint `CONN_TF-01A_HRO`). Fitted. Doesn't work. See status.
- **B1/B2** — two tactile push buttons. **D5/D6** — LEDs. **D4** — 1N5819. **D1–D3** — SOD-123.
- Passives are all 0603.
- **Camera Header for an [OV2640 Module](https://www.amazon.com.au/Purpose-Modules-Compression-Applications-Streaming/dp/B0H9SBRZQX)
  - will use an [fpc-connector based smaller ov2640](https://www.alibaba.com/product-detail/TZ-Mini-OV2640-Camera-Module-CMOS_1601220633036.html?spm=a2700.prosearch.normal_offer.d_title.5f5e67af3gyMJ3&priceId=1afb448003314792b85f3ed40adf9f01) in v2 and have it embedded in unit rather than external.

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

## Status — v1

- Board ordered **August 2026**, delivered, built, partly tested.
- **Working:** keypad, calculator maths, boot, Wi-Fi.
- **Broken:** the SD slot. Fitted and dead. Board decision 2026-10-01 — no SD in v1, it comes
  back in v2.
- This is a first PCB. Nothing here is physically validated beyond "it boots".
- v2 is roughly a month out. No promises.

## Known issues and cleanup before the next order

- **The reports are stale.** DRC and ERC are dated 2026-07-31 / 2026-08-02, older than the last
  board edit (2026-10-03). Re-run both before trusting them.
- Last DRC: 0 violations, but **59 unconnected pads**.
- Last ERC: 0 errors, 5 warnings — two library symbol mismatches and three power pins not driven
  by any output power pin.
---

## Credit

Keypad pad footprints come from **KicadConductivePads** by nataliethenerd — public domain
(Unlicense), sizing loosely based on the Game Boy Color and Pocket pads. Everything else in this
folder is AbuPaad's
Keypad pad footprints come from **KicadConductivePads** by nataliethenerd — public domain
(Unlicense), sizing loosely based on the Game Boy Color and Pocket pads. Everything else in this
folder is AbuPaad's
