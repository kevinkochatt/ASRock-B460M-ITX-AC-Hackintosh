# ASRock-B460M-ITX-AC-Hackintosh
**EFI files for OpenCore 0.7.4 on macOS Monterey 11.6.5, iMac 20,1 SMBIOS**

<img width="698" alt="image" src="https://user-images.githubusercontent.com/69612780/160714600-249ba66f-cae3-490e-b002-6dbdaa34540f.png">

This EFI specifically has patches for:
- Fixing Wi-Fi (stuck on apple logo) for AirportBrcmFixup.kext [needed for BCM94352HMB card]
- UHD 630 platform-id (to fix DisplayPort, use platform-id 07009B3E)
- AppleALC Audio ID for ALC887 (Use ID 12)
- Fixing USB 3.0 on 400 series controller with updated fork of XHCIUnsupported.kext
- Correct USB mapping (including front IO)
- Instant Wake due to Bluetooth USB port (Use SSDT-GPRW + plist patch)

###### Special troubleshooting notes
1. Including the nvmefix.kext on my system caused constant and random kernel panics at boot, requiring multiple boot loop cycles before entering macOS, so beware... I just removed it.

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
- Intel UHD 630 acceleration and HDMI/DisplayPort outputs
- Airdrop and all continutiy features
- iMessage & FaceTime (the reference config.plist does not have a serial number, MLB or ROM for these, you can enter your own)
- Native power managment
- Audio
- Full USB support including 3.0 speeds and port personalities
- Ethernet

#### What doesn't work:
- Keyboard/ Mouse wake: due to my BT card, suspected to be due to BT not having seperate USB hub or controller... I have to wake with the power button.
- Hardware DRM (My system does not have a dGPU for this, just use chrome)

### Patch Guides

##### Fix Wi-Fi (stuck on apple logo) for AirportBrcmFixup.kext
You might have missed it in the notes, but for big sur and up:
1. Right-click the AirportBrcmFixup.kext and "show package contents"
2. In Contents/PlugIns, DELETE AirPortBrcm4360_Injector.kext
3. Now you can use the kext in opencore and boot fine.

##### UHD 630 iGPU (Fix Sleep & DisplayPort output) & AppleALC Audio ID patches
<img width="594" alt="image" src="https://user-images.githubusercontent.com/69612780/160714827-e231f987-c5c1-497f-8848-bb469a23b479.png">
Apply the device properties above via ProperTree to your Config.Plist, add igfxonln=1 to boot-args.

##### Fix USB 3.0 and port mapping
1. Download this fork of USBInjectAll, and only add the XHCIUnsupported to your OC kexts: https://github.com/daliansky/OS-X-USB-Inject-All
(This fork adds support for the unsupported 400 series controller found on this board, 8086:3af)
2. Add the USBPorts.kext from my repo (I mapped with Hackintool, my bluetooth adapter is in the comment for port HS09, you can change it if you want but doesn't do anything.)
3. OC clean snapshot your config and you are good to go, here is mine verified working:
![Screen Shot 2021-06-12 at 5 16 37 PM](https://user-images.githubusercontent.com/69612780/121790953-fa090380-cba1-11eb-9d28-e3a488776b18.png)
    (Hackintool reports a 300 series controller, but dont worry about that)
##### Instant Wake (Fix Sleep)
This is the issue where when your computer sleeps it wakes up a few seconds later, the fans spin back up and the RGB comes on again, sometimes in a cycle of the fans and RGB powering on and off during sleep... beware that using the SSDT-GPRW breaks keyboard/mouse wake and you will have to wake using power button.
1. Add the SSDT-GPRW from my repo to your ACPI
2. Add this to the ACPI patch section of your config.plist:
![Screen Shot 2021-06-10 at 10 54 08 PM](https://user-images.githubusercontent.com/69612780/121632789-df6a4980-ca3e-11eb-9f87-f4faf61740db.png)
3. Now just OC clean snapshot your config and done.
 

