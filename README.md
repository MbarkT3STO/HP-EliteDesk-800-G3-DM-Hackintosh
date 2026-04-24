# HP EliteDesk 800 G3 DM — Dual Boot Hackintosh EFI

OpenCore EFI for **HP EliteDesk 800 G3 DM 65W** supporting dual boot between **macOS Ventura** and **macOS Tahoe** from a single EFI partition.

---

## Hardware Specs

| Component | Details |
|-----------|---------|
| Model | HP EliteDesk 800 G3 DM 65W |
| CPU | Intel Core i5-7500T (Kaby Lake) |
| iGPU | Intel HD Graphics 630 |
| RAM | 24 GB |
| Audio | Realtek ALC221 |
| Ethernet | Intel I219-LM |
| WiFi / BT | Intel (itlwm / AirportItlwm) |
| Bootloader | OpenCore 1.0.7 |
| SMBIOS | iMac18,1 |

---

## Supported macOS Versions

| macOS | Version | Status |
|-------|---------|--------|
| macOS Ventura | 13.x | ✅ Working |
| macOS Tahoe | 26.x | ✅ Working |

---

## What Works

- ✅ Boot picker (OpenCanopy with BsxM1 theme)
- ✅ Intel HD 630 graphics (full acceleration)
- ✅ Audio (Realtek ALC221, layout-id 20)
- ✅ Ethernet (Intel I219-LM)
- ✅ WiFi (Intel — itlwm on Tahoe, AirportItlwm on Ventura)
- ✅ Bluetooth (IntelBluetoothFirmware)
- ✅ USB ports (mapped)
- ✅ NVMe power management (NVMeFix)
- ✅ Sleep / Wake
- ✅ CPU power management
- ✅ iServices (iMessage, FaceTime, AirDrop)

## Not Tested / Not Working

- ❌ Sidecar (no dGPU)
- ❌ AirPlay to Mac (hardware limitation)

---

## EFI Structure

A single unified OpenCore EFI boots both macOS versions. Kexts are gated by `MinKernel`/`MaxKernel` so the correct set loads automatically per OS — no swapping needed.

```
EFI/
├── BOOT/
│   └── BOOTx64.efi
└── OC/
    ├── ACPI/          — SSDTs for Kaby Lake desktop
    ├── Drivers/       — HfsPlus, OpenRuntime, OpenCanopy, AudioDxe, ResetNvramEntry
    ├── Kexts/         — See kext list below
    ├── Resources/     — BsxM1 theme + Acidanthera themes
    ├── Tools/         — OpenShell, CleanNvram, ResetSystem
    └── config.plist
```

### Kexts

| Kext | Version | OS |
|------|---------|-----|
| Lilu | 1.7.1 | Both |
| VirtualSMC | 1.3.7 | Both |
| AppleALC | 1.9.7 | Both |
| WhateverGreen | 1.7.0 | Both |
| IntelMausiEthernet | 2.5.5d0 | Tahoe only |
| IntelMausi | 1.0.7 | Ventura only |
| itlwm | 2.3.0 | Tahoe only |
| AirportItlwm | 2.2.0 | Ventura only |
| IntelBluetoothFirmware | 2.5.0 | Both |
| IntelBTPatcher | 2.5.0 | Both |
| BlueToolFixup | 2.7.1 | Both |
| USBToolBox | 1.1.1 | Both |
| USBPorts | 1.0 | Tahoe only |
| UTBMap | 1.1 | Ventura only |
| NVMeFix | 1.1.3 | Both |
| RestrictEvents | 1.1.7 | Both |
| FeatureUnlock | 1.1.9 | Both |
| SMCProcessor | 1.3.7 | Both |
| SMCSuperIO | 1.3.7 | Both |

---

## Installation

1. Format a USB drive as GPT with a FAT32 EFI partition
2. Create a macOS installer using `createinstallmedia`
3. Mount the EFI partition and copy the `EFI` folder from this repo into it
4. Boot from USB and install macOS to your target volume
5. After installation, mount the internal disk's EFI partition and copy the same `EFI` folder

> **Important:** Generate your own SMBIOS serials using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) before use. Do not use the serials included in this repo.

---

## BIOS Settings

### Disable
- Secure Boot
- Fast Boot
- VT-d (or enable `DisableIoMapper` in config — already set)
- CFG Lock (or keep `AppleXcpmCfgLock: true` — already set)

### Enable
- UEFI Boot Mode
- AHCI for storage

---

## Boot Theme

Uses the **BsxM1** theme by [blackosx](https://github.com/blackosx/BsxM1) — a clean, modern dark theme inspired by Apple's M1 design.

To switch themes, change `PickerVariant` in `config.plist`:
- `Blackosx\BsxM1` — current (modern dark)
- `Acidanthera\GoldenGate` — dark blue
- `Acidanthera\Syrah` — dark wine
- `Acidanthera\Chardonnay` — light/vintage

---

## Credits

- [Acidanthera](https://github.com/acidanthera) — OpenCore, Lilu, VirtualSMC, AppleALC, WhateverGreen, and more
- [OpenIntelWireless](https://github.com/OpenIntelWireless) — itlwm, AirportItlwm, IntelBluetoothFirmware
- [blackosx](https://github.com/blackosx/BsxM1) — BsxM1 theme
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) — OpenCore Install Guide
