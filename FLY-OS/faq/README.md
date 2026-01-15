# User Guide and Notes

## System Features and Design Overview

### 1. System User Description

* FAST adopts a single-user mode to achieve extreme lightweight and security.
* **Only User** :`root`
* **Restriction** : The system does not support creating or switching to other user accounts.

### 2. Package Management

* To maintain its lightweight and fast-boot characteristics, the`FAST` system does not include traditional package managers such as`apt`, and therefore commands like`apt update` cannot be executed.
* **Pre-installed Software** : The system comes preloaded with the Klipper ecosystem and commonly used tools for daily maintenance.
* **Additional Software Needs** : If you require additional packages, please let us know. We will evaluate their general applicability and consider pre-installing them in future releases.

### 3. File System and Persistence

* **Writable Paths** : To ensure the integrity of the core system, the FAST system**only allows modifications** to files under the`/etc` and`/data` directories.
* **Restriction Notice** : All other directories in the system are read-only, and any changes made will not be preserved. Please store your custom configurations and data within the two designated directories mentioned above.

### 4. System Update Policy

The FAST system supports two update methods:

#### OTA Online Update (Recommended)

* Starting from`V1.3.0`, the`FAST` system supports`OTA` upgrades.
* **Update Entry Point** : You can access the system's OTA upgrade page by visiting the host computer's`<strong>IP address:9998</strong>`.
* **Important Notes** :
  * Before initiating a system update, please ensure that**all printing tasks are paused** .
  * During the update process, system services will restart.**Do not perform any printing operations** , as this may lead to print failures or hardware malfunctions.

#### Firmware Update (Reflash)

* **Applicable Scenario** : When the system fails to boot normally.
* **Important Warning** :
  * **Backup Configuration Before Flashing** : The flashing process will erase all user data. Please ensure you back up your printer system configuration in advance.
  * After flashing, you will need to manually restore the backed-up configuration files.
* **Flashing Method** : Please refer to the official flashing tutorial and tools provided.

## Path and Environment Differences

### Firmware Generation Location

* In the FAST system, after successfully compiling the Klipper firmware, the generated firmware file is located at:

```plaintext
/data/klipper/out/
```

### Configuration File Locations

* In the FAST system


| Firmware Version            | Klipper Configuration Path | RRF Configuration Path |
| ----------------------------- | ---------------------------- | ------------------------ |
| Firmware Configuration Path | /usr/share/printer_data/   | /opt/dsf/sd/           |

### Python Environment Differences

The FAST system does not use the `Python venv` virtual environment recommended by the official Klipper, but instead uses the global Python environment. This results in the need to adjust all commands that call Python scripts under the Klipper environment.

**Core Modification:** Replace `~/klippy-env/bin/python` in commands with `python`.


| Scenario Description | Standard System Command                                        | **FAST System Command**                       |
| ---------------------- | ---------------------------------------------------------------- | ----------------------------------------------- |
| Querying CANBUS UUID | ~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0 | python ~/klipper/scripts/canbus_query.py can0 |

**Common Issue** : When executing commands, you may encounter the following error:

```plaintext
-bash: ~/klippy-env/bin/python: No such file or directory
```

**Solution** : Replace the Python interpreter path in the command as shown in the table above.

## Component Installation Guide

This document outlines the general method for installing Klipper plugins in the FAST system.

### General Installation Steps

Most Klipper plugins can be installed using the following simple steps:

1. **Download the component to the ** `<strong>/data</strong>`** directory**
   ```plaintext
   cd /data
   git clone [plugin repository URL]
   ```
2. **Copy the necessary Python files to the Klipper extension directory**
   ```plaintext
   cp /data/[plugin directory]/[plugin main file].py /data/klipper/klippy/extras/
   ```
3. **Restart the Klipper service to apply changes**
   ```plaintext
   systemctl restart klipper
   ```

### Important Notes

* **File Selection** : Confirm the specific Python files that need to be copied based on the plugin documentation.**Do not blindly copy all ** `<strong>.py</strong>`** files.**
* **Dependency Check** : Verify whether the plugin has any special dependency requirements before installation.
* **Version Compatibility** : Ensure the plugin version is compatible with your Klipper version.

### Dependency Notes

**Note** : If you are using **FlyOS_FAST-V1.3.0 or later** , common dependencies are already built into the system, and additional installation is generally not required.

### Common Component Installation Examples

#### Beacon 3D Probe

```plaintext
cd /data && git clone https://github.com/beacon3d/beacon_klipper.git
cp /data/beacon_klipper/beacon.py /data/klipper/klippy/extras/
```

#### IDM or Scanner

```plaintext
cd /data && git clone https://gitee.com/NBTP/IDM.git
cp /data/IDM/idm.py /data/klipper/klippy/extras/
cp /data/IDM/scanner.py /data/klipper/klippy/extras/
```

#### Cartographer 3D

```plaintext
cd /data && git clone https://github.com/Cartographer3D/cartographer-klipper.git
cp /data/cartographer-klipper/idm.py /data/klipper/klippy/extras/
cp /data/cartographer-klipper/scanner.py /data/klipper/klippy/extras/
cp /data/cartographer-klipper/cartographer.py /data/klipper/klippy/extras/
```

### Notes

1. **Installation Location** : All custom components should be installed under the`/data` directory.
2. **File Confirmation** : Before copying files, confirm their purpose to avoid overwriting important files.
3. **Service Restart** : The Klipper service must be restarted after installation to take effect.
4. **Troubleshooting** : If issues occur after installation, check whether the copied files are correct.

# FAQ

## Frequently Asked Questions

### 1. What should I do if I cannot access fly-tools (port 9999) through the browser?

* The FAST system is a highly integrated core runtime environment and does**not** come with the`fly-tools` web utility pre-installed by default. Therefore, the 9999 port will have no service response. This is by design.

### 2. ⚠️ Important: Notes on System Component Updates

* Klipper, Moonraker, and other components within the FAST system are**customized versions** and differ from the official community versions.
* **[Core Principle] Do not manually update via command line or other unofficial channels** , as this will break system optimizations, leading to compatibility issues and functional anomalies.
* **[Correct Method] All component updates must be performed through the following official channels** :
  * Use the system's built-in**OTA online update** feature.
  * Or re-flash the**latest system image** .

### 3. What should I pay attention to during an OTA update?

**1. Configuration File Safety**

* The OTA update is specially designed not to overwrite or modify your personal configuration files.

**2. Notes During the Update Process**

* Ensure stable power supply to the device during the update.
* Unexpected power loss may cause the update to fail, and in severe cases, the system may need to be reflashed.

**3. Handling Custom Code**

* If you have modified Klipper configuration files or installed custom plugins
* The OTA update will automatically skip related files to ensure your changes are preserved.

**4. Important Reminder: Firmware Update**

* After each OTA update, be sure to manually compile the Klipper firmware and re-flash the microcontroller firmware.
