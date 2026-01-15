## OTA System Update Guide

This document provides a detailed explanation of the OTA (Over-The-Air) update feature in the FlyOS-Fast system, helping you upgrade your system securely and conveniently.

## Version Support Information

* **V1.3.0 and above** : Supports OTA online update feature
* **V1.3.3 and above** : Adds support for updating the system via USB drive

## Choosing an Update Method

* 🔄 Online Network Update
* 💾 Offline USB Drive Update

## 🔄 Online Network Update

### Requirements for Network Update

* Device is connected to the internet
* System version is`V1.3.0` or higher

### Steps to Perform the Update

1. Access the following URL in your browser:`http://[device IP address]:9998`
2. The system will automatically check for available updates
3. If a new version is available, click the**"Update"** button to start the upgrade
4. Wait for the system to automatically download and install the update

> 💡 **Tip** : Do not power off the device or start any printing tasks during the update process

## 💾 Offline USB Drive Update

### Preparation

### Requirements for USB Drive Update

* System version is`V1.3.3` or higher

#### USB Drive Requirements

* **Interface Type** : Must use a USB 2.0 flash drive or card reader
* **Storage Capacity** : At least 8GB of available space
* **Compatibility Note** : USB 3.0 devices (blue interface) may cause slow upgrades

#### Downloading the System Package

Please download the corresponding full system update package based on your device model: [https://flyos.klipper.cn/](https://flyos.klipper.cn)

### Update Procedure

#### Step 1: Prepare the Update File

1. Visit the system download page and download the update package for your device
2. Copy the downloaded`<strong>.full.fup</strong>` file to the root directory of the USB drive

> ⚠️ File Matching Verification
>
> * Ensure the update package exactly matches your device model
> * If the file does not match, the system will not display any notification when the USB drive is inserted

#### Step 2: Trigger the Update

1. Insert the prepared USB drive into the device's USB port
2. After detecting the update file, the system will automatically display an update notification

> 🖥️ **Location of Update Notification** :
>
> * **Web Interface** : Mainsail/Fluidd interface
> * **WiScreen 7-inch Screen** : Dedicated serial screen
> * **KlipperScreen** :*No update notification is displayed, please use the web interface*

#### Step 3: Perform the Update

Click the **"Start Update"** button on the interface where the update notification appears. The system will automatically complete the entire update process.


## Important Notes for Updates

### General Safety Tips

* ✅ Pause all printing tasks before updating
* ✅ Ensure stable power supply to avoid unexpected power loss
* ✅ It is recommended to back up important configuration files in advance

### Post-Update Actions

* After completing the OTA update, please recompile the Klipper firmware
* It is recommended to restart the device to ensure all services are properly loaded

---

By following the steps above, you can easily complete the OTA update for the FlyOS-Fast system. If you encounter any issues, please contact our technical support team.
