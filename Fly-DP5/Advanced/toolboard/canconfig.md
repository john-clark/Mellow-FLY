# CAN problem collection

## Precautions before Searching for Equipment ​

*   Before searching for CAN ID, [connect to SSH.](https://docs.3dmellow.com/docs/DebugDoc/BasicTutorial/)
*   Please note that you need to make sure that you are logged in to SSH instead of serial port login using the network.
*   Make sure that you have a motherboard that connects to UTOC or brushes the CAN bridge firmware, and that the data line connected to the host computer has a data transfer function.

## Determine if there is equipment ​

*   Now you have logged in to the machine normally can enter`lsusb`Search equipment, will be one of the following situations
    *   Input`lsusb`The tip cannot be found.`ls`The instruction can be entered into the following command to install the instruction.

```
sudo apt-get install usbutils
```

*   Input`lsusb`After no reaction, this is a system problem here can not do, you need to replace the system or use to determine the normal system.
*   If the information in the image below appears, please note that this is just a reference. You just have to make sure it shows up.`1d50:606f`Can

![](606f.png)

*   `1d50:606f`The equipment you need this time. The following prompt you do not need to care, because the system problem may cause him to show incomplete or simply do not show
*   If there are many`1d50:606f`It is recommended to exclude one, otherwise it will affect the subsequent burning and firmware connection, such as`FLY MINI PAD`It is recommended to use on-board UTOC instead of other CAN bridging devices.
*   If not, please check whether the data line is connected and whether the firmware is brushed correctly.

> Considerations
> 
> Yes`1d50:606f`Search for CAN ID.

## Judging according to Errors ​

*   Below is a common error.
    *   OSError: \[Errno 19\] No such device
    *   CanError: Failed to transmit: \[Errno 100\] Network is down
    *   can.CanError: Failed to transmit: \[Errno 105\] No buffer space available
*   The first is that the host cannot find the CAN device (brush the motherboard of the USB bridge firmware or UTOC)
*   The second is that the host machine is not carried out or is configured incorrectly CAN0
*   Third, always the host cache is insufficient or system problems lead to cache crashes.
*   The second and third points can be seen below the configuration of CAN0, for problem investigation.
*   If you can't find the ID, please see the bottom.

## Check if the host supports CAN ​

*   If it is a FLY host machine, this operation is not required.
*   If your system is`Ubuntu`Needed`Ubuntu配置CAN0`This file has not been updated.
*   Enter the instructions below to determine if the system supports CAN

```
sudo modprobe can && echo "您的内核支持CAN" || echo "您的内核不支持CAN"
```

*   After entering the above command, if your kernel supports CAN, it will return:`您的内核支持CAN`If it is not supported, it will return:`您的内核不支持CAN`. .
*   If returned`您的内核支持CAN`The next step can be configured CAN0.

## Configure CAN0 ​

*   This command is overwritten with the original system CAN0 configuration, and the system needs to be restarted after execution is completed.
*   `FAST`The system does not need to do this!!
*   You need to choose one according to the actual situation.
*   Normal Linux system configuration method
*   Raspberry Pi system configuration method

---

### **Normal Linux system configuration method ​**

It needs to be configured according to the corresponding CAN rate selected by the device

1M rate input below command

```
sudo /bin/sh -c "cat > /etc/network/interfaces.d/can0" << EOF
allow-hotplug can0
iface can0 can static
    bitrate 1000000
    up ifconfig $IFACE txqueuelen 1024
    pre-up ip link set can0 type can bitrate 1000000
    pre-up ip link set can0 txqueuelen 1024
EOF
```

Enter the command below at 500K rate

```
sudo /bin/sh -c "cat > /etc/network/interfaces.d/can0" << EOF
allow-hotplug can0
iface can0 can static
    bitrate 500000
    up ifconfig $IFACE txqueuelen 1024
    pre-up ip link set can0 type can bitrate 500000
    pre-up ip link set can0 txqueuelen 1024
EOF
```

If similar prompts appear, please check the Raspberry Pi system configuration method

![](Untitled2.png)

*   Restart the equipment

```
sudo reboot
```

---

**Raspberry Pi system configuration method**

### Raspberry Pi system configuration method ​

*   For Raspberry Pi systems, other systems should use the normal Linux system configuration method
*   Creating`.network`Files in this configuration`BitRate`It can be modified according to the actual situation, for example`1000000`Replaced by`500000`

```
sudo tee /etc/systemd/network/99-can.network > /dev/null <<'EOF'
[Match]
Name=can*

[CAN]
BitRate=1000000
RestartSec=100ms
EOF
```

*   Creating`.link`Files in this configuration`TxQueueLength`It is a CAN cache and is not recommended to modify it.

```
sudo tee /etc/systemd/network/99-can.link > /dev/null <<'EOF'
[Match]
OriginalName=can*

[Link]
TxQueueLength=1024
EOF
```

*   Restart the equipment

```
sudo reboot
```

---

## After searching for ID, you need to pay attention to the following places. ​

*   If Klipper configures the corresponding ID, it is necessary to shield the ID and shut down the system configuration first, power off in the boot or tap the reset of the motherboard.
*   Whether the host CAN rate is consistent with the motherboard, toolboard, etc.
*   You can use the code below to determine the CAN rate of the host machine
*   Find out if there is a broken line.
*   Is there installation between the tool board and the device (the motherboard or UTOC that has been brushed with the USB bridging firmware)`120Ω`Jumper
*   If there is installation`120Ω`Jumper, which requires a multimeter to measure whether the resistance of CAN H and CAN L is in the case of a complete power failure of the device.`60Ω`Left and right
*   Find out if there is a broken line.

```
ip -details link show can0
```

*   The following diagram is the center of the CAN rate and cache of the host machine.
*   Above`1024`It is currently a CAN0 cache.
*   Below`1000000`The current CAN0 rate.

![](details.png)

If there is still no UUID query, you need to carefully check the following considerations.

*   Check that the motherboard or CAN panel is properly connected
*   Is the power supply correct, the use of the motherboard is recommended to connect to the VCC power supply
*   Does the host support CAN network?
*   Can resistance is in place`60Ω`Left and right
*   Is the firmware compiled correctly?

## Search for ID ​

*   Enter the command below to search for ID

```
~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```

*   If the ID appears and at the back`Application:`Show`Klipper`This ID can be used directly.
*   If the ID appears and at the back`Application:`Show`CANBOOT`Or maybe`Katapult`It means that you need to brush the firmware to use it.

![](canbus-uuid.png)