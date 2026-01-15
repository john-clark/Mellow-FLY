## FAST System USB Backup Guide

This document details how to use a USB drive to back up system configurations, ensuring your printer settings and important files are securely stored.

> **📅 Feature Version History** :
>
> * **V1.3.2** : Added USB configuration backup feature

### 💾 Configuration Backup Guide

Back up system configuration using a USB drive to securely archive important settings and files, preventing accidental loss.

### Backup Preparation

#### USB Drive Requirements

* Formatted as the`FAT32` file system
* Recommended capacity of`8GB` or more
* Ensure the USB drive has sufficient available space

#### Folder Creation

* Create folders in the following structure under the USB drive's root directory:

1. First, create a`printer_data` folder in the USB drive's root directory
2. Then, create three subfolders inside the`printer_data` folder:

```plaintext
printer_data/
├── config/
├── logs/
└── gcodes/
```

> 💡 **Operation Tip** :
>
> 1. Right-click on the USB drive root directory → New → Folder, name it`printer_data`
> 2. Double-click to enter the`printer_data` folder
> 3. Create three new folders:`config`,`logs`, and`gcodes`

### Backup Execution Steps

1. **Insert USB Drive**
   * Insert the prepared USB drive into the device's USB port
   * The system will automatically detect and begin the backup process
2. **Automatic Backup**
   * The system will copy**all files** from the corresponding folders on the device
   * Automatically to the three corresponding subfolders under the`printer_data` folder on the USB drive
3. **Completion Verification**
   * After the backup completes, safely eject the USB drive
   * You can check the files on the USB drive using a computer to confirm the backup was successful

⚠️ Important Notes

* If there are already files with the same name in the USB drive's folders, they will be**overwritten**
* It is recommended to use an empty USB drive or ensure the existing files can be overwritten
* Do not remove the USB drive during the backup process to avoid data corruption
* Regular backups minimize the risk of data loss

### Backup Content Description


| Backup Folder | Contents                                       | Importance |
| --------------- | ------------------------------------------------ | ------------ |
| **config**    | Printer configuration files, macro definitions | ★★★★★ |
| **logs**      | System logs, print records                     | ★★★★★ |
| **gcodes**    | Print files, slicing data                      | ★★★☆☆ |

---

**💡 Backup Recommendation** : It is recommended to perform a backup after every significant configuration change and periodically transfer backup files to another secure location.

---

## USB Disk Printing User Guide

This document details how to use the USB disk printing function in the FlyOS-Fast system and provides solutions for common issues.

## Function Overview

The USB disk printing function allows users to store G-code files on a USB disk and execute print tasks directly via the device. This function is particularly useful in the following scenarios:

* Unstable or unavailable network connections
* Rapid deployment of print tasks
* Offline printing environments
* File transfers between multiple devices

## Detailed Usage Steps

### 1. USB disk Preparation and Formatting

* **Capacity Requirements** : It is recommended to use a USB disk with a capacity of 8GB or more
* **File System** : Must be formatted as`FAT32`
* **Formatting Methods** :
  * Windows: Right-click the USB disk → Format → Choose FAT32
  * macOS: Disk Utility → Erase → MS-DOS (FAT)

### 2. File Preparation and Storage

* **Supported Format** : Standard G-code files (`.gcode` extension)
* **File Placement** : Copy G-code files directly to the root directory or any folder on the USB disk
* **File Naming** : Use English names, avoiding special characters

### 3. Connection and Recognition

1. Insert the prepared USB disk into the device's USB port
2. The system will automatically recognize and mount the USB disk
3. The USB disk files will appear in the file manager on the Web interface

### 4. File Selection and Printing

1. Browse the USB disk files via the device's Web interface
2. Click the target G-code file to preview it
3. Click "Start Printing" after confirming the print parameters
4. The system will automatically load the file and begin the print task

## Important Notes

Important Reminder

* It is**safe to remove the USB disk during printing** as the system copies the file locally before starting the print
* Confirm the file integrity before printing to avoid interruptions
* Large G-code files may take longer to load; please be patient

## Frequently Asked Questions

### ❓ USB Disk Not Recognized

**Symptoms** : No response after inserting the USB disk, and its contents are not displayed in the file manager

**Solutions** :

1. Check if the USB disk is formatted as FAT32
2. Try using a different USB port
3. Test the USB disk's compatibility on other devices
4. Restart the device and reinsert the USB disk

### ❓ File Reading Failure

**Symptoms** : USB disk is recognized, but G-code files cannot be read

**Solutions** :

1. Confirm that the file extension is`.gcode`
2. Check if the file is damaged and attempt to re-slice it
3. Ensure the file has no read/write restrictions
4. Avoid using Chinese or special characters in filenames

### ❓ USB disk Removed During Printing

**Symptoms** : Concern about printing being affected after accidentally removing the USB disk

**Solutions** :

* **No need to worry** : The system has already copied the file locally before printing began
* The print task will continue normally, unaffected by the USB disk's connection status
* You can reinsert the USB disk for the next task after printing completes

### ❓ Slow Transfer Speed

**Symptoms** : Long file loading times affect printing efficiency

**Optimization Suggestions** :

* Use a USB disk with a USB 2.0 interface (best compatibility)
* Avoid using USB 3.0 devices (blue interface), which may have compatibility issues
* Regularly clean the USB disk to maintain sufficient free space
* Consider optimizing slicing settings for large G-code files based on their size

### ❓ File Format Compatibility

**Supported Formats** :

* ✅ Standard G-code files (.gcode)

**Unsupported Formats** :

* ❌ 3D model files (.stl, .obj, etc.)
* ❌ Slicing project files
* ❌ Other binary files

## Best Practice Recommendations

### File Management

* Create categorized folders on the USB disk to better manage multiple print files
* Regularly back up important G-code files
* Clean up completed print files promptly after printing

### Device Maintenance

* Safely eject the USB disk before physically removing it
* Regularly check whether the USB ports are loose or dusty
* Keep the system firmware updated to the latest version

### Print Preparation

* Preview the first layer before each print to ensure the file is correct
* Confirm that the filament is sufficient and the bed is leveled
* For critical print tasks, it is recommended to test a small section locally first

## Technical Support

If you encounter issues not covered in this document, please seek assistance through the following methods:

1. Check the system logs for detailed error information
2. Contact technical support with a clear problem description
3. Visit the official forum to view related discussions and solutions
