# Kazooie | Animatronic Cartoon Bird

A self-contained, battery-powered animatronic figure of Kazooie from *Banjo-Kazooie*. Seven servos drive full-axis head articulation, a blinking eyelid, an opening jaw, and expanding wings. The figure is puppeteered live and runs preprogrammed animations through Bottango, with an Arduino managing motion over I2C.

<img width="8160" height="6144" alt="PXL_20251209_093654632 RAW-01" src="https://github.com/user-attachments/assets/a224e63a-33b3-486f-b893-092b091ffe02" />

---

## What it does

| Motion | Actuators | Notes |
|--------|-----------|-------|
| Head pitch, yaw, and tilt | 3x MG996R | Full three-axis head movement |
| Jaw open and close | 1x MG996R | Synced to performance |
| Eyelid blink | 1x MG90S | Top-lid-only motion for blink |
| Wings expand and retract | 2x 20kg servos | Lever-arm mechanism, highest torque demand |
| Live control + canned animation | Bottango over USB serial | Manual puppeteering plus preprogrammed playback |

The build was scoped as a foundational motion platform. The mechanical and electrical layout leaves headroom to add motion features beyond this first set.

---

## Skills demonstrated

- **Mechanical design and CAD.** Head, neck, jaw, eyelid, and wing mechanisms modeled in Fusion 360 and printed to fit.
- **Motion and controls.** Seven-channel servo motion coordinated through a single PWM driver on the I2C bus.
- **Power system design.** A regulated power chain sized so every servo holds a stable 6V under simultaneous load.
- **Embedded programming.** Arduino C++ handling I2C communication and PWM angle commands.
- **Animation and show programming.** Live puppeteering and animation authoring in Bottango.
- **Fabrication and finishing.** 3D printing, assembly, skinning, painting, and feathering to a finished, presentable figure.
- **Systematic troubleshooting.** Power, wiring, and motion issues diagnosed and resolved through the build.

---

## System architecture

```mermaid
flowchart LR
  BAT["3S LiPo<br/>11.1V 5200mAh"] --> BUCK["12A Buck Converter<br/>11.1V to 6V"]
  PC["PC running Bottango"] -->|USB serial| ARD["Arduino UNO R3"]
  ARD -->|"I2C (SDA / SCL)"| PCA["PCA9685<br/>16-ch PWM driver"]
  ARD -->|5V logic| PCA
  BUCK -->|"6V power rail (V+ / GND)"| PCA
  PCA --> S1["Head pitch (MG996R)"]
  PCA --> S2["Head yaw (MG996R)"]
  PCA --> S3["Head tilt (MG996R)"]
  PCA --> S4["Jaw (MG996R)"]
  PCA --> S5["Eyelid (MG90S)"]
  PCA --> S6["Wing L (20kg)"]
  PCA --> S7["Wing R (20kg)"]
```

Two separate paths keep the logic clean and the motion stable. Commands travel from Bottango to the Arduino over USB serial, then to the PCA9685 over I2C, where they become per-channel PWM angle signals. Power is kept off the logic path entirely: the LiPo feeds a buck converter that holds a steady 6V rail into the PCA9685 power terminals, so torque demand from the wings never browns out the controller.

---

## Bill of materials

| Component | Role | Why it was chosen |
|-----------|------|-------------------|
| Arduino UNO R3 | Microcontroller, the brain of the figure | Parses commands and drives PWM logic to the servo driver |
| PCA9685 | 16-channel PWM servo driver | Gives plug-and-play multi-channel servo control over I2C, well beyond the Arduino's own pin count |
| 12A DC buck converter | Voltage regulator, 11.1V down to 6V | Holds a consistent 6V to every servo regardless of torque demand, keeping motion stable and synchronized |
| 3S LiPo, 5200mAh, 11.1V | Power source | High current output for simultaneous servo movement, with headroom for the buck converter; swappable for portability |
| 2x 20kg servos | Wing actuation | Lever-arm mechanics need higher torque; metal gears, ball bearings, and metal cases handle frequent high-load motion |
| 4x MG996R servos | Head pitch, yaw, tilt, and jaw | Mid-range metal-gear servos with enough torque for lightweight load-bearing joints |
| 1x MG90S servo | Eyelid blink | Compact and light enough to mount inside the head, with low power draw |

---

## Build process

**Concept and schematic.** Motion goals and the full wiring layout were planned before any cutting or printing.

<img width="5331" height="6947" alt="PXL_20251209_095350764 RAW-01" src="https://github.com/user-attachments/assets/d2a5558f-0fe4-4a6c-8c4c-c1593df31350" />
<img width="1844" height="4000" alt="20250811_032910" src="https://github.com/user-attachments/assets/65300582-d53d-48a9-b5a3-aa17f2cabf70" />


**CAD.** The head, mechanisms, and mounting were modeled in Fusion 360 around the chosen servos.

<img width="551" height="689" alt="Kazooie CAD Side" src="https://github.com/user-attachments/assets/2521ce63-3920-47a0-9096-d625b2cee92b" />
<img width="1234" height="694" alt="Kazooie CAD Rear" src="https://github.com/user-attachments/assets/00553e58-7025-4abf-864d-652eb28ec708" />
<img width="1234" height="694" alt="Kazooie CAD Front" src="https://github.com/user-attachments/assets/359194c3-615f-4ea0-80e3-bedfbab8321e" />

**Printing and assembly.** Printed parts were assembled into the head and wing frames, then wired into the controller and power system.

<img width="4000" height="1844" alt="20250824_185037" src="https://github.com/user-attachments/assets/cf0216c8-0d32-4360-82a3-c8de3b026faf" />
<img width="1844" height="4000" alt="20251022_154354" src="https://github.com/user-attachments/assets/808e47b8-2d50-48c4-99eb-bced552c5e27" />

**Mechanism and skin.** The articulated head was skinned and the moving structure verified under the cover.

<img width="6144" height="8160" alt="PXL_20251124_093219824 RAW-01" src="https://github.com/user-attachments/assets/2e505678-5a5f-4755-8821-69e547705464" />
<img width="7994" height="6019" alt="PXL_20251201_011435102 RAW-01~3" src="https://github.com/user-attachments/assets/329bfaef-43bd-45f6-9b49-995a58798428" />

**Aesthetics.** Paint and feathering brought the figure up to its finished character look.

<img width="6144" height="8160" alt="PXL_20251129_230924693 RAW-01" src="https://github.com/user-attachments/assets/46ed3919-d754-423b-a65b-04f0f338d750" />
<img width="6144" height="8160" alt="PXL_20251129_091742788 RAW-01" src="https://github.com/user-attachments/assets/7ff69a24-9a37-4873-a319-28d1c5ffafcf" />
<img width="6144" height="8160" alt="PXL_20251205_205023124 RAW-01" src="https://github.com/user-attachments/assets/fb4d006f-1797-4aa4-8a14-63c07bcde5f9" />


**Finished figure.**

<img width="6144" height="8160" alt="PXL_20251209_093702068 RAW-01" src="https://github.com/user-attachments/assets/6ff1483b-21a1-45e7-915a-7d7fed50e127" />


---

## Demo videos

- Head and jaw motion: `add link`
- Wing expand and retract: `add link`
- Blink and full performance: `add link`

---

## Repository structure

```
kazooie-animatronic/
├── README.md
├── .gitignore
├── docs/
│   └── Final_Project_Report.docx
└── media/
    └── build and finished photos
```
---

## About

Built by Corey Hausterman for EET1035C, December 2025. Mechatronics and Robotics Engineering Technology, Seminole State College of Florida.

This is a non-commercial fan and portfolio project. Kazooie and *Banjo-Kazooie* are the property of Nintendo and Rare. No affiliation or endorsement is implied.
