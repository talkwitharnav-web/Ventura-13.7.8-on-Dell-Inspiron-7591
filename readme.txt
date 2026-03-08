Ventura 13.7.8 on Dell Inspiron 7591
OpenCore EFI for macOS Ventura on the Dell Inspiron 7591

⚠️ This EFI is provided as-is. Use at your own risk. Always backup your data before installing or modifying your system.


Credits
This EFI was built with light reference to tctien342's Dell Inspiron 7591 repo:
👉 https://github.com/tctien342/dell-inspiron-7591-hackintosh
Credit to tctien342 for the backlight fixes as well as CPU profile fixes on this machine. The CPU power profile management via SmartCPU and xbar setup referenced in this repo is also sourced from their work — check their repo for setup instructions on that.

System Specs
CPU: Intel i5 9300H
GPU: Intel UHD 630
dGPU: n/A
RAM: 24GB
SSD: Samsung 850 evo plus
Screen: 1080p@60hz

What Works ✅

macOS Ventura boots and runs stably
Intel UHD 630 graphics acceleration
CPU power management (via SmartCPU + xbar — see Credits)
WiFi (Intel Wireless-AC 9560 via airportitlwm
General macOS functionality (App Store, iCloud, etc.)
ISERVICES WORKS


What Doesn't Work / Known Issues ❌
EC (Embedded Controller) Driver Problem
This is the big one. macOS cannot properly communicate with the laptop's Embedded Controller, which causes a cascade of issues:

Battery/AC state misread — macOS doesn't correctly detect whether the laptop is plugged in or on battery - it can sometimes, other times not.
Fan running at constant RPM — because the EC can't be read properly, fan control is broken and the fan just spins at a fixed speed constantly
Auto CPU profile switching is broken — SmartCPU/xbar can manually switch profiles, but automatic switching based on power state (plugged in vs. battery) doesn't work because macOS can't reliably detect AC state
Fixes include using the xbar and cpu profile files from Tctien's Big Sur/Monterey repo, along with the "Mac Fan Control" app with manual fan curves.

CPU Power Management (SmartCPU + xbar)
Since auto-switching is broken due to the EC issue, CPU profiles need to be switched manually using SmartCPU via xbar.

Install xbar: https://github.com/matryer/xbar
SmartCPU plugin sits in your menu bar and lets you toggle between power profiles manually
Switch to a low-power profile when on battery, high-performance when plugged in
See tctien342's repo (linked above) for the full SmartCPU setup guide


Dual Boot (Windows + macOS)
This machine is set up for dual booting Windows and macOS.

⚠️ Important: The storage controller must be set to AHCI mode in BIOS, NOT RAID. If you're coming from a Windows-only setup, use the bcdedit /set safeboot minimal Safe Mode method to switch from RAID to AHCI without breaking your Windows install.


Notes

This EFI was built from scratch — not a direct clone of tctien342's EFI
Serial numbers have been scrubbed from config.plist — you will need to generate your own using GenSMBIOS before booting. Preferably use current Mac model in SMBIOS
which should be MacBookPro16,x
Tested only on the i5-9300H variant with iGPU. The i7 variant with a dGPU (NVIDIA) will be different — the dGPU will not work under macOS regardless.
You may need to do your own research as to how to disable the dGPU, though it shouldn't be more work than adding an ACPI table and a kext or two.


Generating Your Own Serials
This config.plist ships with placeholder serials. You must generate your own before using:

Download GenSMBIOS
Use SMBIOS type MacBookPro16,x (check SMBIOS values in config plist via propertree or OCAT)
Fill in MLB, SystemSerialNumber, SystemUUID, and ROM in config.plist


Disclaimer
This is a personal build shared for reference. I'm not responsible for bricked laptops, dead SSDs, thermonuclear war, Apple hunting you down, or you getting fired because you spent too much time trying to get macOS working. Do your research, have backups, and I pray you succeed in hackintoshing!
