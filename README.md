# EFI Folder 

This repository contains the **EFI** folder for a triple-boot setup on an **Asus X556UQ** laptop. It supports:

- macOS Ventura  
- Linux Mint 22.2  
- Windows 11  

Everything is preconfigured and validated for OpenCore 0.8.8.

## Table of Contents

- [Folder Structure](#folder-structure)  
- [Installation](#installation)  
- [Configuration Highlights](#configuration-highlights)  
- [Boot Entries](#boot-entries)  
- [Troubleshooting](#troubleshooting)  

## Folder Structure
EFI/
└── OC/
├── ACPI/ # SSDTs and patches
├── Drivers/ # Required UEFI drivers
│ ├── OpenRuntime.efi
│ ├── OpenCanopy.efi
│ ├── ResetNvramEntry.efi
│ ├── ext4_x64.efi # ext4 FS support for Linux
│ └── OpenLinuxBoot.efi # Auto-detect Linux kernel loader
├── Kexts/ # macOS kernel extensions
│ ├── Lilu.kext
│ ├── VirtualSMC.kext
│ └── WhateverGreen.kext
├── Resources/ # OpenCore UI assets
│ └── Image/
│ └── Acidanthera/
│ └── GoldenGate/
│ ├── *.icns # Built-in icons
│ └── background.png
├── Tools/ # Optional UEFI tools
├── config.plist # OpenCore configuration
└── version.plist # Tracks OpenCore build version

## Installation

1. Format a USB drive as **FAT32**.  
2. Copy this **EFI** folder to the root of the USB as `EFI`.  
3. Enter your BIOS/UEFI settings:
   - Disable **Secure Boot**  
   - Enable **AHCI** for SATA  
   - Set **Boot Mode** to **UEFI** only  
4. Boot from the USB in your firmware menu.  
5. In OpenCore’s picker, select **“Install macOS Ventura”** to install macOS.  
6. Similarly, use your Mint and Windows installer media as needed.  
7. After installations, mount your system’s EFI partition and replace its `EFI/OC` folder with this one.

## Configuration Highlights

- **SMBIOS** set to MacBookPro14,3 for Ventura compatibility.  
- Proper **NVRAM** serialization for OpenCore 0.8.8.  
- **APFS** drivers installed for macOS.  
- **ext4_x64.efi** + **OpenLinuxBoot.efi** enabled for Mint auto-detection.  
- **OpenCanopy** picker theme with a custom `background.png`.  
- Custom `mint.icns` icon for the Linux entry.

## Boot Entries

OpenCore now presents exactly **three entries**:

1. **Windows 11**  
2. **macOS Ventura**  
3. **Linux Mint**  

- The Linux entry loads via **OpenLinuxBoot** and boots the current kernel automatically.  
- Windows and macOS entries are managed via OpenCore’s `Misc → Entries`.  

## Troubleshooting

- **No boot entry**: Ensure your firmware has an **OC** entry pointing at `EFI/OC/OpenCore.efi`.  
- **Invalid config**: Run `ocvalidate` to catch serialization or driver-ordering errors.  
- **Linux not booting**: Confirm `ext4_x64.efi` and `OpenLinuxBoot.efi` are **Enabled** under **UEFI → Drivers**.  
- **Picker issues**: Verify `LauncherOption` in **Misc → Boot** is set to `Full` for Linux scanning or `Disabled` if using a single manual entry.

