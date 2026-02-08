# USB-Cellular-Modem: Obfuscated NB-IoT/Cat-M Terminal

## Overview
**USB-Cellular-Modem** is an open-hardware cellular terminal designed for to enable any device equipped with USB to interface with LTE-CAT-M1 or NB-IOT Cellular Networks. The modem is low-power enough to be used as a back-up cellular interface for any mobile device. 

**NOTE:** Due to Apple restrictions on USB Interface access, this device cannot interface with Apple Devices. There may be a bluetooth variant in the future.

The core of the device is the **SIMCOM 7080G**, providing global NB-IoT and Cat-M1 connectivity with a focus on a minimal RF footprint and user-controlled privacy.

---

## Key Security & Design Features
* **Active-Discharge Kill Switch**: A physical SPDT slide switch ($SW1$) that disconnects the buck converter and actively shorts the $+BATT$ rail to ground through a $10\Omega$ resistor ($R4$). This ensures the $1000\mu F$ bulk capacitance is drained in milliseconds, preventing any residual "heartbeat" transmissions.
* **UART Signal Isolation**: $1k\Omega$ series resistors ($R8$, $R9$, $R10$, $R11$) are placed on all data lines (TX, RX, RTS, DTR). This prevents the **CH343P** from "back-powering" the modem via internal ESD diodes when the master power is cut.
* **Automated Power Trigger**: The **DTR** line is tied to a **DTC143Z** digital transistor ($Q1$). Opening the serial port on the host machine automatically pulses the modem's **PWRKEY**, initiating the boot sequence.
* **Local RF Shielding**: The PCB includes grounded rectangular frames designed for soldering a metal Faraday cage over the antenna matching network ($J1$, $C2$, $R1$, $C3$) to mitigate local signal sniffing.
* **eSIM Integration**: Includes a dedicated footprint for the **ST4SIM-200M** GSMA-certified eSIM for tamper-resistant credential storage.

---

### Included Hardware
| Component | Part Number | Description |
| :--- | :--- | :--- |
| **Cellular Module** | **SIMCOM 7080G** | Global NB-IoT / Cat-M1 with GNSS. |
| **USB-UART Bridge** | **WCH CH343P** | High-speed USB to serial converter. |
| **Power Regulator** | **TPS62A01ADRL** | 1000mA Synchronous Buck Converter (5V to 3.8V). |
| **Embedded SIM** | **ST4SIM-200M** | Industrial-grade MFF2 eSIM. |
| **Nano SIM Slot** | **CUI Hinged NSIM-2** | Nano-SIM Card Slot. |

---

## Technical Specifications
* **Input Power**: USB VBUS (+5V, 500mA).
* **Logic Level**: $1.8V$ Internal ($VDD\_EXT$), isolated from $3.3V$ USB signals.
* **Stackup**: 4-Layer PCB with solid internal ground planes.
* **EDA Software**: **KiCad 9.0.7**.

## Usage
1.  **Enable Device**: Flip $SW1$ to the `ON` position.
2.  **Communication**: Open a serial terminal (e.g., `minicom` or `screen`) at `115200` baud. The DTR toggle will boot the modem.
3.  **Kill Session**: Flip $SW1$ to the `OFF` position. The modem is physically disconnected and the power rail is grounded.

---

## Front 3D Render
<img width="596" height="678" alt="image" src="https://github.com/user-attachments/assets/1bffacc3-353c-4d65-b23e-c0424c598829" /> 

## Back 3D Render
<img width="630" height="702" alt="image" src="https://github.com/user-attachments/assets/8cc08f2c-f3e8-4134-b98a-daa69c0bfc44" />

## License

Dimensions and Modem Placement Derived from [techstudio-design/sim7080_dev_board](https://github.com/techstudio-design/sim7080_dev_board)

All PCB design files and hardware are released under the [Creative Commons Attribution Share Alike 4.0](https://choosealicense.com/licenses/cc-by-sa-4.0/) license.
