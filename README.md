# Tiny Solar Supply: KiCad Schematic & PCB Layout

A compact solar-charged 3.3 V supply built around the **AP3015A** micropower boost converter. A small solar panel charges a single 1.2 V Ni-MH AA cell during the day; when it gets dark, the boost converter switches on automatically and turns the battery voltage into a regulated **3.3 V** rail for microcontroller-based loads.

This repository contains my **KiCad schematic capture and PCB layout** of the circuit.

> **Credit:** The circuit is the original design of **Clemens Valens (Elektor)**, published as *"Tiny Solar Supply"* in *Elektor Circuit Special 2023* (project 220321-01). Original project files: <https://elektormagazine.com/labs/tiny-solar-supply>. This repo is my own KiCad re-implementation, made for learning and portfolio purposes. All credit for the circuit concept goes to the original author.

---

## Features

- Regulated **3.3 V output** from a 1.2 V Ni-MH cell
- **Automatic day/night switching:** the boost converter is shut down while the solar panel is producing power, and turned on when it is dark
- Optional **ON/OFF header** (K3) for manual control
- A-version (AP3015A): input from **1 V to 12 V**, up to **100 mA** output
- Non-A version (AP3015): starts at 1.2 V, up to **350 mA** output
- Small 2-layer board using mostly 0805 SMD parts

## How It Works

1. **Daytime:** the solar panel (K4) charges the battery (K2) through Schottky diode **D2**. The panel voltage also pulls the gate of **T1** (2N7002) high, so T1 conducts and pulls the `/SHDN` pin of **IC1** low, turning the boost converter off.
2. **Night:** when the panel voltage drops, charging stops, T1 turns off, and `/SHDN` is released. IC1 starts up and boosts the battery voltage to 3.3 V at the output (K1).
3. **Output voltage** is set by the feedback divider R1/R2:

   `VOUT = 1.23 × (1 + R1 / R2) = 1.23 × (1 + 1 MΩ / 604 kΩ) ≈ 3.27 V`

If you don't want the automatic switching, leave T1 out. A switch or jumper on K3 gives extra manual control.

## Schematic

![Schematic](docs/schematic.png)

## PCB Layout

| Top | Bottom |
|-----|--------|
| ![PCB Top](docs/pcb_top.png) | ![PCB Bottom](docs/pcb_bottom.png) |

### 3D View

![3D View](docs/pcb_3d.png)

## Bill of Materials

| Ref | Value | Package / Footprint | Notes |
|-----|-------|---------------------|-------|
| R1, R4 | 1 MΩ | 0805 | 0.125 W |
| R2 | 604 kΩ, 1 % | 0805 | Sets output voltage with R1 |
| R3 | 10 kΩ | 0805 | Pull-up for T1 drain / IC1 `/SHDN` |
| C1 | 4.7 µF, 50 V, X7R | 0805 | Input capacitor |
| C2 | 22 µF, 10 V, X7R | 1206 | Output capacitor |
| C3 | 10 pF, 50 V, X7R | 0805 | Feedback capacitor |
| L1 | 10 µH, 680 mA | Inductor (resistor-style or SMD) | Must pass the maximum load current |
| D1, D2 | SS14 | DO-214AC (SMA) | Schottky diodes |
| IC1 | AP3015 / AP3015A | SOT-23-5 | A-version: 1 V start, 100 mA; non-A: 1.2 V start, 350 mA |
| T1 | 2N7002 | SOT-23 | Optional (automatic switching) |
| K1 | Pin header 1×2, 2.54 mm | | Output (OUT) |
| K2 | Pin header 1×2, 2.54 mm | | Battery (BAT) |
| K3 | Pin header 1×2, 2.54 mm | | ON/OFF |
| K4 | Pin header 1×2, right-angle, 2 mm | | Solar panel (SOLAR) |

## IC1 Pinout (as used in the schematic)

| Pin | Name | Connection |
|-----|------|------------|
| 1 | SW | L1 / D1 anode |
| 2 | GND | Ground |
| 3 | FB | R1 / R2 divider, C3 |
| 4 | /SHDN | R3, T1 drain |
| 5 | IN | Battery / L1 input, C1 |

## Repository Structure

```
.
├── hardware/
│   ├── tiny-solar-supply.kicad_pro
│   ├── tiny-solar-supply.kicad_sch
│   └── tiny-solar-supply.kicad_pcb
├── gerbers/            # Fabrication outputs
├── docs/               # Schematic PDF, screenshots, 3D renders
├── bom/                # Bill of materials
└── README.md
```

## Design Notes

- **Tool:** KiCad <version>
- **Board:** 2-layer, <width> × <height> mm
- **Layout considerations:**
  - Keep the switching loop (IC1 `SW` → L1 → D1 → C2) as short as possible
  - Place C1 close to the IC1 `IN` and `GND` pins
  - Keep the feedback network (R1, R2, C3) close to the `FB` pin and away from the switching node
  - Use a solid ground plane / pour on the bottom layer

## Getting Started

1. Clone the repository:
```bash
   git clone https://github.com/siddharthkote25/<repo-name>.git
```
2. Open `hardware/tiny-solar-supply.kicad_pro` in KiCad.
3. Run ERC on the schematic and DRC on the PCB.
4. Generate Gerbers and drill files from **File → Fabrication Outputs**.

## Usage

| Connector | Connect to |
|-----------|------------|
| K4 (SOLAR) | Solar panel (+ / −) |
| K2 (BAT) | 1.2 V Ni-MH AA cell |
| K3 (ON/OFF) | Switch or jumper |
| K1 (OUT) | 3.3 V load |

> **Note:** Observe polarity on K2 and K4. Use a Ni-MH cell only, as the circuit is designed around a 1.2 V rechargeable cell.

## Status

- [x] Schematic capture
- [x] PCB layout
- [ ] DRC / ERC clean
- [ ] Gerbers generated
- [ ] Board fabricated and tested

## License

Original circuit design © Clemens Valens / Elektor. Please check the license on the original Elektor Labs project page before reusing or redistributing. My KiCad files in this repo are shared for educational purposes.

## Author

**Siddharth Kote**
Electronics & Communication Engineering
GitHub: [@siddharthkote25](https://github.com/siddharthkote25)
Email: siddharthkote129@gmail.com
Portfolio: <https://siddharth-kote-pcb.vercel.app/>

## References

- C. Valens, *"Tiny Solar Supply,"* Elektor Circuit Special 2023, pp. 6–7
- Elektor Labs project page: <https://elektormagazine.com/labs/tiny-solar-supply>
- Diodes Inc. AP3015 / AP3015A datasheet
