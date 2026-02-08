# Secure-Cell: Obfuscated NB-IoT/Cat-M Terminal

## Overview
**Secure-Cell** is an open-hardware cellular terminal designed for highly secure and obfuscated communication. Unlike standard development boards, this project integrates hardware-level "kill" mechanisms and signal isolation to ensure the device remains completely silent and electrically "invisible" to the network when not in use.

The core of the device is the **SIMCOM 7080G**, providing global NB-IoT and Cat-M1 connectivity with a focus on a minimal RF footprint and user-controlled privacy.

---

## Key Security & Design Features
* **Active-Discharge Kill Switch**: A physical SPDT slide switch ($SW1$) that disconnects the buck converter and actively shorts the $+BATT$ rail to ground through a $10\Omega$ resistor ($R4$). This ensures the $1000\mu F$ bulk capacitance is drained in milliseconds, preventing any residual "heartbeat" transmissions[cite: 198, 217, 212, 214].
* **UART Signal Isolation**: $1k\Omega$ series resistors ($R8$, $R9$, $R10$, $R11$) are placed on all data lines (TX, RX, RTS, DTR)[cite: 239, 241, 247, 257]. This prevents the **CH343P** from "back-powering" the modem via internal ESD diodes when the master power is cut.
* **Automated Power Trigger**: The **DTR** line is tied to a **DTC143Z** digital transistor ($Q1$)[cite: 255, 256]. Opening the serial port on the host machine automatically pulses the modem's **PWRKEY**, initiating the boot sequence[cite: 61, 253].
* **Local RF Shielding**: The PCB includes grounded rectangular frames designed for soldering a metal Faraday cage over the antenna matching network ($J1$, $C2$, $R1$, $C3$) to mitigate local signal sniffing[cite: 34, 35, 36, 39].
* **eSIM Integration**: Includes a dedicated footprint for the **ST4SIM-200M** GSMA-certified eSIM for tamper-resistant credential storage[cite: 132].

---

## Included Hardware List

### Core Modules & Power
| Component | Part Number | Description |
| :--- | :--- | :--- |
| **Cellular Module** | **SIMCOM 7080G** | Global NB-IoT / Cat-M1 with GNSS[cite: 26, 47]. |
| **USB-UART Bridge** | **WCH CH343P** | High-speed USB to serial converter[cite: 261]. |
| **Power Regulator** | **TPS62A01ADRL** | 600mA Synchronous Buck Converter (5V to 3.8V)[cite: 197]. |
| **Embedded SIM** | **ST4SIM-200M** | Industrial-grade MFF2 eSIM[cite: 132]. |

### Security & Passive Components
* **Master Kill-Switch**: **SS-12D07** (or compatible) SPDT Slide Switch[cite: 198].
* **Pulse Transistor**: **DTC143Z** NPN Digital Transistor ($Q1$)[cite: 255, 258].
* **Isolation Resistors**: $4 \times 1k\Omega$ 0603 Series Resistors ($R8, R9, R10, R11$)[cite: 239, 241, 247, 257].
* **Discharge Resistor**: $10\Omega$ 0805 Resistor ($R4$)[cite: 217].
* **Bulk Capacitors**: $2 \times 500\mu F$ Low-ESR Capacitors ($C1, C18$)[cite: 212, 213, 214].
* **ESD Protection**: **USBLC6-2SC6** (USB) and **ESDA6V1W5** (SIM)[cite: 55, 113].
* **RF Connector**: **u.FL / IPEX** Surface Mount Connector ($J1$)[cite: 34, 39].

---

## Technical Specifications
* **Input Power**: USB VBUS ($+5V$)[cite: 56, 194].
* **Logic Level**: $1.8V$ Internal ($VDD\_EXT$), isolated from $3.3V$ USB signals[cite: 99, 102].
* **Stackup**: 4-Layer PCB with solid internal ground and power planes.
* **Development OS**: Optimized for **Ubuntu 25.10**.
* **EDA Software**: **KiCad 9.0.7**[cite: 30, 284].

## Usage
1.  **Enable Device**: Flip $SW1$ to the `ON` position[cite: 204].
2.  **Communication**: Open a serial terminal (e.g., `minicom` or `screen`) at `115200` baud. The DTR toggle will boot the modem.
3.  **Kill Session**: Flip $SW1$ to the `OFF` position. The modem is physically disconnected and the power rail is grounded[cite: 198].

---

## License & Credits
Designed by **Henry Cheung** | **Tech Studio Design, 2021**[cite: 21, 22].
Released under the CERN Open Hardware Licence.

## Front 3D Render
<img width="596" height="678" alt="image" src="https://github.com/user-attachments/assets/1bffacc3-353c-4d65-b23e-c0424c598829" /> 

## Back 3D Render
<img width="630" height="702" alt="image" src="https://github.com/user-attachments/assets/8cc08f2c-f3e8-4134-b98a-daa69c0bfc44" />

## License

Dimensions and Modem Placement Derived from [techstudio-design/sim7080_dev_board](https://github.com/techstudio-design/sim7080_dev_board)

All PCB design files and hardware are released under the [Creative Commons Attribution Share Alike 4.0](https://choosealicense.com/licenses/cc-by-sa-4.0/) license.
