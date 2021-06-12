# ASRock-B460M-ITX-AC-Hackintosh
**EFI files for OpenCore 0.7.0 on macOS Big Sur 11.4, iMac 20,1 SMBIOS**

![Screen Shot 2021-06-10 at 10 13 02 PM](https://user-images.githubusercontent.com/69612780/121631021-8b119a80-ca3b-11eb-914e-001e83697485.png)

This EFI specifically has patches for:
- UHD 630 iGPU patch (to fix sleep, use platform-id 00009B3E)
- AppleALC Audio ID for ALC887 (Use ID 12)
- Fixing USB 3.0 on 400 series controller with updated fork of XHCIUnsupported.kext
- Correct USB mapping (including front IO)
- Instant Wake (Use SSDT-GPRW + plist patch)

###### Special troubleshooting notes
1. I noticed my iGPU patch was different from other b460m itx/ac hackintoshes that could be related to displayport output, I do not have a displayport monitor so i cannot verify any issues related to it.
2. Including the nvmefix.kext on my system caused constant and random kernel panics at boot, requiring multiple boot loop cycles before entering macOS, so beware... I just removed it.

#### Specifications
- Storage: WD Black SN750 1TB PCIe SSD
- CPU: Core i7 10700
- GPU: Intel UHD 630 (ordered a RTX 3070 for gaming in windows, couldn’t find or get a hold of high performance, compatible Radeon cards)
- RAM: XPG Spectrix D60G 16GB (2x8GB) 3200MHz running at 2933MHz
- Motherboard: ASRock B460M-ITX/AC
- Audio Codec: Realtek ALC 887
- Ethernet Card: Intel I219-V
- Wifi/BT Card: BCM94352HMB (adapted with a Mini-PCIe to M.2 adapter and U.FL antennas)
- BIOS Version: 1.60 

#### What works:
- WiFi & BT 4.1
- Airdrop and all continutiy features
- iMessage & FaceTime (the reference config.plist does not have a serial number, MLB or ROM for these, you can enter your own)
- Native power managment
- Audio
- Full USB support including 3.0 speeds and port personalities
- Ethernet

#### What doesn't work (or untested):
- Motherboard DisplayPort (untested)
- Keyboard/ Mouse wake: working to solve this now, for now only power button can wake on my system...
- Hardware DRM (My system does not have a dGPU for this, just use chrome)

### Patch Guides

##### UHD 630 iGPU (Fix Sleep) & AppleALC Audio ID patches
![Screen Shot 2021-06-10 at 10 36 07 PM](https://user-images.githubusercontent.com/69612780/121631429-53572280-ca3c-11eb-9ea2-32326a3dc2e5.png)

Apply the device properties above via ProperTree to your Config.Plist

##### Fix USB 3.0 and port mapping
1. Download this fork of USBInjectAll, and only add the XHCIUnsupported to your OC kexts: https://github.com/daliansky/OS-X-USB-Inject-All
(This fork adds support for the unsupported 400 series controller found on this board, 8086:3af)
2. Add the USBPorts.kext from my repo (I mapped with Hackintool, my bluetooth adapter is in the comment for port HS09, you can change it if you want but doesn't do anything.)
3. OC clean snapshot your config and you are good to go, here is mine verified working:
![Screen Shot 2021-06-12 at 5 16 37 PM](https://user-images.githubusercontent.com/69612780/121790953-fa090380-cba1-11eb-9d28-e3a488776b18.png)
    (Hackintool reports a 300 series controller, but dont worry about that)
##### Instant Wake (Fix Sleep)
This is the issue where when your computer sleeps it wakes up a few seconds later, the fans spin back up and the RGB comes on again, sometimes in a cycle of the fans and RGB powering on and off during sleep... beware right now using the SSDT-GPRW breaks keyboard/mouse wake and you will have to wake using power button.
1. Add the SSDT-GPRW from my repo to your ACPI
2. Add this to the ACPI patch section of your config.plist:
![Screen Shot 2021-06-10 at 10 54 08 PM](https://user-images.githubusercontent.com/69612780/121632789-df6a4980-ca3e-11eb-9f87-f4faf61740db.png)
3. Now just OC clean snapshot your config and done.
 

