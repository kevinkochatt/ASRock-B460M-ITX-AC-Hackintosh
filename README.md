# ASRock-B460M-ITX-AC-Hackintosh
**EFI files for OpenCore 0.7.0 on macOS Big Sur 11.4**

This EFI specifically has patches for:
- UHD 630 iGPU patch (to fix sleep)
- AppleALC Audio ID for ALC887
- Fixing USB 3.0 on 400 series controller
- Correct USB mapping (including front IO)
* I noticed my iGPU patch was different from other b460m itx/ac hackintoshes that could be related to displayport output, I do not have a displayport monitor so i cannot verify any issues realted to it.

#### Specifications
-Storage: WD Black SN750 1TB PCIe SSD
-CPU: Core i7 10700
-GPU: Intel UHD 630 (ordered a RTX 3070 for gaming in windows, couldn’t find or get a hold of high performance, compatible Radeon cards)
-RAM: XPG Spectrix D60G 16GB (2x8GB) 3200MHz running at 2933MHz
-Motherboard: ASRock B460M-ITX/AC
-Audio Codec: Realtek ALC 887
-Ethernet Card: Intel I219-V
-Wifi/BT Card: BCM94352HMB (adapted with a Mini-PCIe to M.2 adapter and U.FL antennas)
-BIOS Version: 1.60 

#### What works:
-WiFi & BT 4.1
 -ef
