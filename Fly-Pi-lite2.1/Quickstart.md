🚀 Quick start

🎯 Objective of this guide : Complete the system deployment and firmware burning of the Fly-Pi-lite motherboard within 30 minutes to enable it to connect to Klipper and perform basic testing.  
✅ Complete the logo : Access the Klipper interface via the web.  
Important tips

```
📦 Must be accessory : Fly-Pi-lite series must be used with TF card, otherwise the system cannot be started
⚡ Power supply requirements : Input voltage range 5V, do not exceed this range
🔌 Safe operation : Be sure to disconnect the power before all wiring operations! When burning the firmware, please also follow the instructions.
🔧 Tool preparation : Prepare basic tools such as screwdrivers, data lines, etc. in advance
```

Step 1: Prepare tools and resources ​

[Download File](/%E8%BD%AF%E4%BB%B6%E5%8C%85.zip)

📝 Contains content

```
MobaXterm(for connection to the host machine)
CH340(Series port driver for system connection)
Panasonic_SDFormatter(for formatting the TF card)
SetupSTM32CubeProgrammer(for burning the bitten bit of firmware)
```

Step 2: Install the system and the basic configuration ​

System mirror burning ​

*   System Mirror Burning Guide

Basic system configuration ​

*   System configuration modification

1.  Network and Remote Connection ​

*   Connecting with SSH
*   Connect to WiFi

1.  Display device configuration ​

*   Screen configuration

1.  Power supply on the receiver ​

*   Power supply on the receiver

📝 This step description

```
Before burning the image, make sure that the TF card or M2WE is formatted correctly
It may take 3-5 minutes to start the system for the first time, please be patient.
The default user name when connecting to SSH: root, password: memellow
Screen configuration only needs to be configured if there is a screen device
```

📈 ​

The following functions can be configured step by step as needed after the completion of the basic settings and the printer can operate normally:  
📝 Extensions and advanced configuration  
Use of camera ​

```
Camera configuration
```

External toolboard ​

```
Tool board connection and ID acquisition
```

Download the resource ​

```
Resource Download Center

Extensions are recommended to configure and test one by one to avoid changing too many parameters at the same time
Camera configuration requires additional USB camera device support
Resonance compensation requires accelerometer sensor
```