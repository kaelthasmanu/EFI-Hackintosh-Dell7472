# OpenCore EFI for Dell 7472 Hackintosh

This repository contains an OpenCore EFI configured for a Dell 7472 laptop running macOS Tahoe. It is tailored to a Skylake platform with Intel HD Graphics 520 and is not a universal EFI for every Dell laptop.

The current configuration includes a `config.plist.sequoia-before-tahoe-2026-09-12.backup` file, which preserves the pre-Tahoe setup for reference or rollback.

## Target System

| Component | Configuration |
| --- | --- |
| Laptop | Dell 7472 |
| Bootloader | OpenCore |
| Target OS | macOS Tahoe |
| Platform | Intel Skylake |
| Integrated graphics | Intel HD Graphics 520 |
| SMBIOS | MacBookPro15,2 |
| Audio | AppleALC, layout ID 13 |
| Wi-Fi | Intel Wireless 7265 via itlwm |
| Bluetooth | Intel Bluetooth 7265 |
| Ethernet | Realtek RTL8111 |
| Storage | NVMe, with NVMeFix |

## Included Support

The EFI enables the following hardware and laptop features through OpenCore, ACPI tables, and kernel extensions:

- Intel HD 520 graphics acceleration and framebuffer configuration
- Internal display backlight control and brightness hotkeys
- Audio through AppleALC
- Intel Wi-Fi and Bluetooth
- Realtek wired Ethernet
- NVMe storage support
- Battery reporting, Dell sensors, ambient light sensor, CPU monitoring, and Super I/O monitoring
- I2C/HID touchpad support and PS/2 keyboard, mouse, and trackpad support
- USB power properties and EC, SBUS, GPI0, MCHC, and CPU power-management SSDTs
- OpenCanopy graphical boot picker and Reset NVRAM entry

## EFI Layout

```text
EFI/
├── BOOT/BOOTx64.efi
└── OC/
	├── ACPI/       # Custom SSDTs for the Dell platform
	├── Drivers/    # OpenRuntime, OpenCanopy, HfsPlus, ResetNvramEntry
	├── Kexts/      # Hardware drivers and feature support
	└── config.plist
```

## Installation

1. Prepare a macOS Tahoe installer USB using Apple's supported method.
2. Mount the USB drive's EFI partition.
3. Copy this repository's `EFI` folder to the root of that partition, replacing its existing `EFI` folder.
4. Configure the BIOS for macOS/OpenCore use, then boot the installer from the OpenCore picker.
5. After macOS is installed, copy the same `EFI` folder to the internal drive's EFI partition.

## Important

- Generate your own SMBIOS values before using iCloud, iMessage, FaceTime, or other Apple services. Do not reuse the serial number, MLB, ROM, or UUID currently stored in `EFI/OC/config.plist`.
- Keep a backup of a known-working EFI before updating macOS, OpenCore, kexts, or BIOS settings.
- This configuration is specific to the hardware listed above. Review ACPI tables, device properties, USB mapping, and kexts before using it on a different Dell model or hardware revision.

## Credits

This EFI uses OpenCore and kexts from the Acidanthera, Voodoo, itlwm, and related Hackintosh communities. Please refer to each project's license and documentation when updating or redistributing their components.
