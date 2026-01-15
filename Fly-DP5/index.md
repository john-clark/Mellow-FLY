# Fly-DP5 Document Index

## **Quickly navigate.**

### 1\. **Preparations** ​

**Hardware List:**

*   **Motherboard** : Fly-DP5
*   **Upper mount**::

> Host Compatibility Instructions
> 
> *   **Recommended** models: Fly-PI-V3, Raspberry Pi 4/5 and other standard on-board
> *   **Non-supporting** models: **Redmi mobile phone, WiFi stick and set-top box**, all kinds of magic changes non-standard machine
> *   **Important:** Using a non-standard host may cause system instability, abnormal function, or malfunctioning normally
> *   **Data line** : Type-C data line
> *   **Connection mode** : Only type-C connection machine can be used
> *   **Power supply** : 12V-24V power adapter (please make sure the voltage is in the range)
> *   **Computer** : Windows computer (or other computer that can use SSH tools)

> Prepared for explanation
> 
> *   It is recommended to use a reliable Type-C data line, which can cause unstable connections
> *   Power adapter power needs to meet printer needs, recommended ≥300W
> *   Make sure the Klipper-related system (such as MainsailOS) is installed on the host

---

### 2\. **Use of guidelines and considerations** ​

**Core principles** : Most problems are caused by inconsistent configuration, connection, or power supply. Here are the highlights of Fly-DP5:

*   [Frequently Asked Questions](Usage/faq.md)

---

### 3\. **Firmware Burning and Device Identification** ​

#### 1\. Firmware Description and Pattern Selection ​

*   [Next Computer Firmware Burning Guide](Firmware/)

#### 2\. Burning USB or CAN firmware ​

Select the corresponding firmware according to the connection method:

*   [USB Firmware Burning](Firmware/usb) or [CAN Bridge Firmware Burning](Firmware/can)

> Note for this step
> 
> *   **Device recognition** : Be sure to record the device ID/UUID after burning, configure the necessary information for Klipper
> *   **Standby programme**: If the burning fails, try to burn the BL firmware first.
>     *   [BL firmware burning](Firmware/bl)

---

### 4\. **Hardware Connection and Klipper Configuration** ​

**Contains** content:

*   Functions of the motherboard interfaces
*   Electrical, heater, sensor and other wiring methods
*   Klipper Reference Profile

**Related Documents:**

*   [Hardware Connections and Interface Descriptions](Usage/wiring.md)
*   [Klipper reference configuration](Usage/klipper.md)

> Connection security tips
> 
> *   **Ensure that the power supply is completely disconnected before all wiring is operated**
> *   Power polarity must be confirmed correctly, the reverse may cause damage to the motherboard
> *   Motor wiring sequence reference to your printer frame drawings
> *   Please check the thermocouple connection before heating the hot bed and the hot end

---

### 5\. \*\*Advanced Features and Advanced Configuration (Optional)\*\*​

**Optional functions:**

*   **Marlin Firmware** : Replace Klipper with Marlin Firmware
*   **External tool board** : Support for multi-tool head system

**Related Documents:**

*   [Tool board connection and ID acquisition](Advanced/toolboard)
*   [Accelerometer Configuration](Advanced/adxl)
*   [Marlin firmware](Advanced/marlin)
*   [Driver LED configuration](Advanced/led)

> Extended Function Description
> 
> *   Extensions are recommended to configure and test one by one to avoid changing too many parameters at the same time
> *   Toolboard connection for multi-toolhead 3D printers

---

### 6\. **Technical information and resources** ​

**Contains** content:

*   Schematic download
*   PCB layout diagram
*   3D model file

**Related Documents:**

*   [Schematics and Resources Download](Advanced/schematic.md)

---

## **Use of recommendations**

### **New** Users: ​

1.  Start with **Preparatory ,** ensure that all hardware is compatible
2.  Read carefully **in the Notes** section to avoid common mistakes
3.  **Completed Firmware Burning** in Order → **Hardware Connections** → **Basic Testing**
4.  When you encounter a problem, first look at **the fault troubleshooting** related documents

### **Experienced Users:** ​

1.  View the specific features required documentation directly
2.  Reference **Technical Information** for Detailed Technical Parameters
3.  Configure **advanced features** on demand to improve print performance

4.  Try **Marlin firmware** or other extensions


