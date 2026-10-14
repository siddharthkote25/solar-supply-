# Tiny Solar Supply: PCB Design

### ☀️ Solar-Charged 3.3 V Boost Power Supply

**Tiny Solar Supply** is a KiCad PCB design project: a compact power supply that charges a single **1.2 V Ni-MH AA cell** from a small solar panel and boosts it to a regulated **3.3 V** output using the **AP3015A** micropower boost converter. It switches automatically between charging (daytime) and boosting (night).

This repository contains the **KiCad 9 schematic, PCB layout, component symbols and footprints, and design images** for the board.

> **Credit:** The circuit is the original design of **Clemens Valens (Elektor)**, published as *"Tiny Solar Supply"* in *Elektor Circuit Special 2023* (project 220321-01). Original project files: <https://elektormagazine.com/labs/tiny-solar-supply>. This repo is my own KiCad re-implementation for learning and portfolio purposes.

---

## 🔧 Hardware

| Component | Purpose |
| --- | --- |
| **AP3015A (AP3015AKTR-G1)** | Micropower step-up (boost) DC/DC converter |
| **2N7002 (T1)** | Automatic day/night shutdown control for IC1 |
| **SS14 (D1, D2)** | D1: boost rectifier, D2: battery charging path |
| **ASPI-0630LR-100M-T15 (L1)** | 10 µH boost inductor |
| **1.2 V Ni-MH AA cell** | Energy storage |
| **Solar panel** | Charging source |
| **Resistors & Capacitors** | Output voltage feedback, input/output filtering |
| **Pin headers** | Solar, battery, ON/OFF and output connections |

---

## ⚙️ How It Works

* **Daytime:** the solar panel charges the battery through D2 and pulls the gate of T1 high. T1 conducts and pulls the `/SHDN` pin of IC1 low, so the boost converter is off.
* **Night:** the panel voltage drops, T1 turns off, `/SHDN` is released and IC1 boosts the battery voltage to 3.3 V.
* **Output voltage** is set by the feedback divider R1/R2:

```text
VOUT = 1.23 × (1 + R1 / R2) = 1.23 × (1 + 1 MΩ / 604 kΩ) ≈ 3.27 V
```

* An optional ON/OFF header (K3) gives manual control. Leave T1 out to disable the automatic switching.

---

## 📐 Schematic Design

The circuit schematic was designed in **KiCad 9**, covering the boost converter, the automatic shutdown stage, battery/solar connections and the feedback network.

### Tiny Solar Supply Schematic

![Tiny Solar Supply Schematic](./Solar%20Suply%20%28Schematic%29.png)

### Key Design Work

* Schematic capture and circuit connectivity
* Component symbol and footprint integration (AP3015A, SS14, ASPI inductor)
* Net labeling
* Power and ground connections
* Feedback divider (R1/R2) calculation for the 3.3 V output
* Automatic day/night shutdown using a MOSFET on the `/SHDN` pin
* Connector design for solar panel, battery, ON/OFF and output

---

## 🖥️ PCB Design

The project follows a complete **KiCad PCB design workflow**:

```text
Schematic → Symbol & Footprint Assignment → PCB Layout
→ Component Placement → Routing → Silkscreen
```

### PCB Layout

![Tiny Solar Supply PCB Layout](./Solar%20Supply%20%28Layout%20Design%20%29.png)

### 3D View

![Tiny Solar Supply 3D View](./3D%20View%20%28Solar%20Supply%29.png)

### PCB Design Work

* PCB layout and board organization
* Component placement for a short boost-converter switching loop
* SMD footprint management (0805, 1206, SOT-23, SOT-23-5, DO-214AC)
* Power and signal routing
* Feedback network placement close to the `FB` pin
* Ground connections
* Silkscreen labelling

---

## 🔌 Main Connections

```text
   Solar panel ──► D2 ──► Battery (1.2 V Ni-MH)
        │                      │
        └──► T1 (2N7002)       └──► L1 ──► IC1 (AP3015A) ──► D1 ──► 3.3 V OUT
               │                              ▲
               └────────── /SHDN ─────────────┘
                                              │
                               R1 / R2 feedback divider
```

---

## 🛠️ Tools & Technologies

**PCB Design**

* KiCad 9
* Schematic Capture
* PCB Layout
* Component Placement
* Routing
* Symbol and Footprint Integration

**Electronics**

* Boost (step-up) DC/DC conversion
* Solar charging
* MOSFET switching
* Feedback voltage setting
* Power & Signal Routing

**Version Control**

* Git
* GitHub

---

## 📂 Repository Contents

```text
solar-supply-/
│
├── AP3015AKTR-G1/                     # IC1 symbol & footprint
├── SS14/                              # Schottky diode symbol & footprint
├── ul_ASPI-0630LR-100M-T15/           # Inductor symbol & footprint
├── Tiny Solar Supply.kicad_sch        # Schematic
├── Tiny Solar Supply.kicad_pcb        # PCB layout
├── Tiny Solar Supply.kicad_pro        # KiCad project file
├── report.txt                         # KiCad report
├── Solar Suply (Schematic).png        # Schematic image
├── Solar Supply (Layout Design ).png  # PCB layout image
├── 3D View (Solar Supply).png         # 3D render
└── README.md
```

---

## 📋 Bill of Materials

| Ref | Value | Footprint |
| --- | --- | --- |
| R1, R4 | 1 MΩ | 0805 |
| R2 | 604 kΩ, 1 % | 0805 |
| R3 | 10 kΩ | 0805 |
| C1 | 4.7 µF, 50 V, X7R | 0805 |
| C2 | 22 µF, 10 V, X7R | 1206 |
| C3 | 10 pF, 50 V, X7R | 0805 |
| L1 | 10 µH (ASPI-0630LR-100M-T15) | SMD inductor |
| D1, D2 | SS14 | DO-214AC |
| IC1 | AP3015AKTR-G1 | SOT-23-5 |
| T1 | 2N7002 | SOT-23 |
| K1, K2, K3 | Pin header 1×2, 2.54 mm | Through-hole |
| K4 | Pin header 1×2, right-angle, 2 mm | Through-hole |

---

## 🎯 Skills Demonstrated

`KiCad 9` `PCB Design` `Schematic Capture` `PCB Layout`
`Symbol & Footprint Integration` `Component Placement` `Routing`
`Boost Converter Design` `Power Electronics` `Git` `GitHub`

---

### 📜 License & Credit

Original circuit design © Clemens Valens / Elektor. Please check the license on the original Elektor Labs project page before reusing or redistributing. My KiCad files are shared for educational purposes.

### 👨‍💻 Author

**Siddharth Kote**
siddharthkote129@gmail.com
[GitHub](https://github.com/siddharthkote25) · [Portfolio](https://siddharth-kote-pcb.vercel.app/)
