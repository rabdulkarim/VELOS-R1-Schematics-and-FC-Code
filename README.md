# VELOS-R1 Flight Computer

<p align="center">
  <img src="velospcb.png" alt="VELOS-R1 PCB Layout" width="45%" />
  <img src="velos3d.png" alt="VELOS-R1 3D Render" width="45%" />
</p>

---

## Origin

VELOS-R1 started as a question I couldn't stop thinking about: what does it actually take to build a rocket that controls its own trajectory mid-flight?

I was 15 when I first became absorbed by the engineering behind thrust-vector-controlled rockets — not the spectacle of launch, but the hardware underneath it. The custom PCBs, the sensor fusion, the closed-loop control running in real time on a microcontroller the size of a thumb. I wanted to understand it from the ground up, not by following a kit or replicating a tutorial, but by designing every part myself.

VELOS-R1 is the result of several months of self-directed work done alongside academics. I taught myself PCB design in EasyEDA from scratch, learned how I2C protocol wires sensors to a microcontroller, understood why pull-up resistors matter on those lines, and eventually produced a working schematic and Gerber files for a custom flight computer. The perfboard prototype — hand-soldered component by component — successfully reads attitude data from an MPU6050 IMU and barometric altitude from a BMP280, logs both to an SD card, and drives two servos simultaneously for 2-axis TVC control.

VELOS-R1 never flew. The firmware lacks a tuned PID controller and proper sensor fusion. Solid rocket motors are not accessible where I live, and without a propulsion solution the vehicle could not be completed in time. I made the decision to document what I had built honestly rather than let the work disappear into an unfinished folder.

That decision is what this repository is.

---

## What VELOS-R1 Is

VELOS-R1 is a custom flight computer designed for thrust-vector-controlled model rockets. It is not a commercial product, a finished system, or a flight-proven design. It is an honest first attempt at a hard problem by a self-taught developer — documented so that others starting the same journey have something real to reference, including its limitations.

The hardware runs on an Arduino Nano (ATmega328P) and integrates:

- **MPU6050** — 6-axis IMU over I2C for attitude estimation
- **BMP280** — barometric pressure sensor over I2C for altitude measurement
- **2-axis servo control** — for thrust vector deflection
- **SD card module** — for flight data logging

The PCB was designed in EasyEDA. Gerber files are included for fabrication. The perfboard prototype was hand-soldered and bench-tested.

---

## Known Limitations

These are not footnotes. They are the most important part of this documentation.

**No sensor fusion.** Raw MPU6050 output drifts. Without a Kalman filter or complementary filter fusing accelerometer and gyroscope data into a reliable attitude estimate, the IMU readings are not suitable for stabilisation. This is the primary unsolved problem in the firmware.

**No PID controller.** The servo control logic exists but there is no closed-loop feedback. The vehicle cannot stabilise itself in flight in its current state.

**ATmega328P processing constraints.** At 16MHz, the ATmega328P struggles to run a full sensor fusion loop at the update rates required for responsive TVC control. A faster microcontroller is the correct solution for future revisions.

**Not flight-proven.** This hardware has never been flown. Use any part of this design at your own risk and with appropriate safety precautions.

---

## Repository Contents

```
VELOS-R1-Schematics-and-FC-Code/
├── EasyEDA Schematics/       # Schematic JSON files, importable into EasyEDA
├── Gerbers and BOM/          # Gerber files for PCB fabrication, bill of materials
├── VELOS Software/           # Arduino firmware
└── README.md
```

---

## Using the Schematics

1. Install [EasyEDA](https://easyeda.com/) or open the online editor at easyeda.com
2. Import the `.json` files from the `EasyEDA Schematics` folder
3. Review the BOM in `Gerbers and BOM/` before ordering components

---

## What Comes Next

The problems left unsolved in VELOS-R1 are the starting point for VELOS-R2. Specifically: a working Kalman filter implementation for attitude estimation, a tuned PID controller closing the loop from attitude error to servo deflection, a faster microcontroller, and a propulsion solution that does not depend on imported solid motors. VELOS-R2 is in active development.

---

## Disclaimer

This project is for educational and development purposes only. The hardware has not been flight-tested. Rocketry involves real physical risk. Follow all applicable laws and safety guidelines in your jurisdiction before building or flying anything based on this design.
