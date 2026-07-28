# Casio FX-Series ESP32-S3 Motherboard Upgrade

An open-source, drop-in replacement motherboard for classic Casio FX-series calculators (e.g., FX-9860G, FX-9750G). This mod replaces the low-res monochrome screen with a vibrant 2.8" TFT display and drops in a powerful ESP32-S3 with Wi-Fi/Bluetooth capabilities, while completely reusing the original calculator shell and keypad membrane.

## 🎯 Project Goals

- **Drop-In Fit**: The custom PCB must match the original Casio factory board mounting holes, screw locations, and battery terminal contacts perfectly.
- **Modern Hardware**: Power the system with an ESP32-S3-WROOM-1 module for high-performance computing, memory, and wireless connectivity.
- **Display Upgrade**: Route and mount a 2.8-inch SPI/Parallel TFT display inside the original screen viewing window.
- **Stock Keypad Reuse**: Leverage the factory keypad matrix via an onboard I2C GPIO expander to conserve ESP32 pins.

## 📐 Hardware Specifications & Architecture

- **Microcontroller**: ESP32-S3-WROOM-1 (Dual-core, 240MHz, 2.4GHz Wi-Fi & BLE).
- **Keypad Matrix Controller**: `TCA9535DBR` 16-bit I2C GPIO expander handling a 6x10 or similar switch matrix.
- **Display Interfacing**: 2.8" TFT Display (Targeting ST7789 or ILI9341 controller over high-speed SPI).
- **Power Delivery**: USB-C for charging/flashing + low-dropout (LDO) linear regulator to step down to 3.3V.

---

## 📝 Contributor To-Do List

We are looking for electrical engineers, PCB layout designers, and reverse-engineers to help finish the board design!

### 🛑 Schematics (High Priority)
- [ ] **Power Management Circuitry**: Design a reliable 3.3V power rail fed from both the USB-C VBUS and the battery contacts (including basic reverse-polarity protection).
- [ ] **TFT Display Connector**: Finalize the exact pinout/FPC connector footprint (0.5mm pitch) for the 2.8" TFT display panel.
- [ ] **I2C Pull-up Resistors**: Add required 4.7kΩ pull-up resistors to the `SDA` and `SCL` lines routing between the ESP32-S3 and the `TCA9535DBR`.
- [ ] **Auto-Program/Reset Circuit**: Implement the standard dual-transistor (EN/BOOT) circuit using USB-to-UART or direct native USB-C connections for easy firmware flashing.

### 📐 PCB Layout & Routing
- [ ] **Board Outline Verification**: Replicate the exact mechanical physical board outline of the Casio FX motherboard, matching all battery spring clips and alignment tabs.
- [ ] **RF Keep-out Zone**: Ensure no copper, ground planes, or components are placed directly underneath the ESP32-S3 onboard PCB antenna area.
- [ ] **Keypad Alignment**: Align the PCB pad layout perfectly with the conductive rubber buttons of the factory keyboard membrane.
- [ ] **High-Speed Routing**: Route the TFT display SPI/Parallel trace signals cleanly to prevent display noise or flickering.

### 🧪 Validation & BOM
- [ ] **BOM Scrubbing**: Select standard, easily-sourceable SMD component sizes (preferably 0603/0805 for resistors/capacitors to allow hand-soldering).
- [ ] **3D CAD Model**: Export a `.step` file of the board from KiCad to verify fitment inside an 3D-scanned or modeled Casio calculator plastic shell.

---

## 🛠️ Design Guidelines

- **KiCad Version**: Please use **KiCad v8.0** or newer. Do not commit backward-incompatible project files.
- **DRC Rules**: Keep traces at a minimum of 6mil width / 6mil spacing. Use a minimum 0.3mm via drill size.
- **Pull Requests**: Include a brief description of what you changed, an updated Electrical Rules Check (ERC) snippet, and a screenshot of the 3D viewer if you altered the board layout.

## 📄 License

This hardware design is open-source and licensed under the **CERN Open Hardware Licence Version 2 - Strongly Reciprocal (CERN-OHL-S)**.
