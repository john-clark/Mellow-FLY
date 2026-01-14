# Firmware Flashing Instructions

## **Host Connection Instructions**

> Core Connection Limitation
> 
> The Fly-D5 mainboard **can only communicate with the host via the Type-C USB interface** for Klipper.

![](host-usb.png)

**Connection Requirements**:

*   Use a **high-quality Type-C data cable** to ensure stable data transmission.
*   It is recommended to avoid using excessively long data cables (not exceeding 1.5 meters).
*   Connect to a standard USB-A port on the host computer.

---

## How to Select the Correct Firmware

> 🔧 Firmware Selection Rules
> 
> Please select the correct firmware according to your connection method and toolboard type, following these rules:

| Configuration Scenario | Mainboard Firmware | Toolboard Firmware | Key Notes |
| --- | --- | --- | --- |
| **Not using a toolboard** |   |   |   |
| (Direct USB connection) | `USB Firmware` | — | The most common configuration, suitable for most single-extruder printers. |
| **Using a CAN toolboard** | `CAN Bridge Firmware` | `CAN Firmware` | Communication via CAN bus. **Please note**: The mainboard must be flashed with CAN Bridge firmware; otherwise, a USB-to-CAN adapter (such as UTOC) is required. |