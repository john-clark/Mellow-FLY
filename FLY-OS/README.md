# FlyOS-Fast System Introduction

## Overview

FlyOS-Fast is a lightweight Linux system specifically optimized for
3D printing, providing a stable and efficient operating environment for
the entire Fly series of host platforms.

## Core Features

### 🏗️ System Architecture

* **Single User Mode** : Minimalist design, supporting only the`root` user to ensure system security and stability.
* **File System Optimization** : Only the`/etc` and`/data` directories are writable; core directories are read-only, significantly reducing the risk of power failure damage.
* **Fast Boot** : Optimized boot process, completing KlipperScreen startup within 30 seconds.

### 📦 Software Ecosystem

* **Complete Pre-installed Ecosystem** : Built-in mainstream services such as Klipper, RRF, Mainsail, Fluidd, Moonraker, and KlipperScreen.
* **Streamlined Package Management** : Removed traditional package managers, pre-installed essential tools, and maintained system lightweight.
* **Environment Optimization** : Global Python environment simplifies plugin installation and command execution.

### ⚡ Functional Features


| Feature Category         | Feature Description                                                                                                       |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **System Switching**     | Supports switching between Klipper and RRF dual systems                                                                   |
| **Interface Management** | One-click switching between Mainsail and Fluidd web interfaces                                                            |
| **Power Management**     | Power-off shutdown and power failure resume printing functions Choose one configuration, cannot be enabled simultaneously |
| **Data Management**      | USB port configuration file backup + OTA system updates                                                                   |
| **Firmware Management**  | Automatic firmware flashing on boot for devices like fly-c8, fly-c5, fly-c8p                                              |

**Getting Started**


| Document Topic                               | Description                                                                          |
| ---------------------------------------------- | -------------------------------------------------------------------------------------- |
| [**Usage & Notes**](faq)                     | Understand the core features, limitations, and basic operations of the FAST system.  |
| [**Image Flashing**](flash)                  | Flashing the system for the first time on a new device or reinstalling the system.   |
| [**Configuration**](config)                  | Configure your FAST system from scratch.                                             |
| [**SSH**](ssh)                               | Installing the system card and SSH connection methods.                               |
| [**WIFI**](wifi)                             | WIFI connection methods.                                                             |
| [**Power Loss Recovery**](power) | Configure automatic print recovery after power loss and power-off shutdown function. |

**Daily Maintenance & Updates**


| Document Topic         | Description                                                       |
| ------------------------ | ------------------------------------------------------------------- |
| [**OTA Updates**](ota) | Update the system online via the web page, convenient and secure. |
| [**USB Backup**](usb)  | Use a USB drive for configuration backup or printing.             |

---

> ### **Usage Suggestions**
>
> * **New Users** : It is recommended to start with "Getting Started" and read in order.
> * **System Upgrade** : Read the "OTA Online Update" document.
> * **Feature Configuration** : Select the corresponding document in "Feature Configuration" based on your needs.
