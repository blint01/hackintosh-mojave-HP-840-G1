# Hackintosh 10.14.1 guide for HP Elitebook 840 G1

This is a detailed macOS Mojave 10.14.1 Vanilla install guide for HP Elitebook 840 G1 , based on [RehabMan's HP guide](https://www.tonymacx86.com/threads/guide-hp-probook-elitebook-zbook-using-clover-uefi-hotpatch.261719/). [This guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/) is also really useful.

**Contents**:
- [Creating the USB Installer](#creating-the-usb-installer)
- [BIOS Settings](#bios-settings)
- [Install](#install)
- [Post Install](#post-install)


My 840 G1 config:
- **CPU**: i5-4210U (Haswell)
- **SSD**: ADATA 128GB
- **RAM**: 8GB DDR3
- **DISPLAY**: 1600x900 (non-touch)
- **GPU**: Integrated, Intel HD Graphics 4400
- **AUDIO CODEC**: IDT92HD91BXX
- **ETHERNET**: Intel I218-LM
- **WIFI**: Intel Dual Band Wireless-N 7260NB 802.11 a/b/g/n (2 x2 ) WiFi  //  *It's not compatible*

Here you can check the different models: https://support.hp.com/lv-en/document/c03961746

But I recommend you to boot a live Linux Mint to see which config you have. Google the commands for it.

## Creating the USB Installer
> Prerequisites: at least 8GB+ pendrive, mine was USB2, but in theory USB3 going to work just fine too. Based on experiences, it's more likely to work without problems on USB2. Format it to MacOS Extended (Journaled) and GUID Partition Map. You need a macOS to config all the things that are we going to do, a Virtual one going to do the job, but not making the USB, see it later.

I think this one is the hardest part. You need the Mojave installer file from the AppStore, but it's only accessible from Macs. There are two things you can do: 
- Download and install a Hackintosh dmg file, from hackintoshzone or from Olarila. Maybe you could try to install a VirtualBox MacOS and make one there, but I never tried these. More experienced hackintoshers say that Virtual MacOS creates corrupted installers.
- Look for a friend, family member, anyone who has a real mac and ask them to make a bootable USB for you. This is what I did.
   - Follow [this tutorial](https://www.imore.com/how-create-bootable-installer-mac-operating-system). 
   - *From this part, you can do everything in a [MacOS VM](https://www.wikigain.com/install-macos-sierra-10-12-virtualbox/)!* After you are successfully created the bootable USB, install [Rehabman's Clover bootloader](https://bitbucket.org/RehabMan/clover/downloads/) on the USB as follows:<img src="/images/cloverinstall.png" width=600>
   
   - Here click on the "Change Install Location...", select the USB and then click on "Customize". In the Customize menu, select "Install for UEFI booting only" and "Install Clover in the ESP", among the Drivers64UEFI leave the ones that are already selected, but tick the VBoxHFS-64 and ApfsDriverLoader-64. Install and you are done.
   - Download the essential kexts from Rehabman's repo: [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/), [VoodooPS2Controller](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/), [FakeSMC](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/), [IntelMausiEthernet](https://bitbucket.org/RehabMan/os-x-intel-network/downloads/), [Lilu](https://github.com/acidanthera/Lilu/releases), [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases) unzip them, copy the .kext files from the Release folders.
   - Now we need to download [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/). Install it, then open it. Under the "Mount EFI" section find your USB's EFI Partition and click on "Mount Partition". Now you'll see a new EFI partition in your Finder. Open it and navigate to EFI->CLOVER->kexts->Other and copy the previously downloaded and extracted kexts.<img src="/images/cloverconfig.png" width=750>
   - Download the USBconfig.plist file from [my repository](/usbconfig.plist) and paste it to the USB's EFI Partition in EFI->CLOVER, then rename it to config.plist. You can delete the old one, or if you want to play it safe, rename it to something else.
   - If my usbconfig.plist doesn't work for you, you have to make one for yourself or download one from somewhere else.
   
## BIOS Settings  
###### *Spam the F10 key*

Go to Advanced
   - Device configuration
      - Don't check Fn key switch (it has to be unchecked in order to the volume and brightness hotkeys work)
      - Change Video memory to 128MB
      - Disable wake on USB
      - Disable Virtualization Technology (VT-x)
      - Set SATA to AHCI
   - Built in device option
      - Disable Wake on LAN

Save and reboot in UEFI.

## Install
###### *Spam the F9 key and select External Device*
1. Clover will greet you. Boot from the USB. If you can't boot, you can play with the boot parameters under the "gear" icon. -x (safe boot), -v (verbose), there are many more, google it.<img src="/images/cloverusbinstall.png" width=600>

2. For me it was super slow, probably because of the USB2, it took almost 10 mins to get here:<img src="/images/macinstall.jpg" width=600>

3. Choose Disk Utility, from View select "Show all Device". Then select your Drive, not the partition! Click on Erase, from Format select APFS, Scheme should be GUID Partition Map. Close Disk Utility.<img src="/images/diskutil.png" width=600>

4. Click on Install macOS. Accept everything and install it on your Drive. It will take a lot of time. The installer will reboot your system, but don't worry, boot from your USB and in Clover select the Boot from Drive option.

5. When this screen appears, just click on Next, Next, Next, set your location, set your username, etc. Don't forget to turn off sharing your analytic data with apple and your location.<img src="/images/installwelcome.jpg" width=600>

Congrats, you have a partially working MacOS Mojave on your Elitebook, it can't boot without your USB and a bunch of things don't work yet.

## Post Install

We are going to fix the trackpad, the sound, the brightness keys and the battery with Rehabman's SSDT patches and kexts.
You should have internet connection on LAN, because Clover injected the IntelMausi kext.
You can open this guide on your freshly installed Mac Mojave and copy every Terminal command.
Don't reboot your computer until you finish with every step!

1. First install [Rehabman's Clover bootloader](https://bitbucket.org/RehabMan/clover/downloads/) with the same settings as we did on the USB, but now on your Drive, not on the USB.  

2. We need the developer tools to use Rehabman's script. Since you have internet working, you can simply download it with this command. If prompted, select Install. Type in the Terminal:
```
xcode-select --install
```
3. After it's installed, we are going to make a new folder in the Downloads folder, where we clone the HP Probook repo from github, type in the Terminal:
```
mkdir ~/Downloads/Projects
cd ~/Downloads/Projects
git clone https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch probook.git
```
4. Now we are going to download and install kexts and tools from the repo.
```
cd ~/Downloads/Projects/probook.git
./download.sh
```
5. Wait until it finishes, then type in this:
```
./install_downloads.sh
```
6. We have to disable hibernation mode, because on hackintoshes it's not supported and can cause serious problems.
```
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```
7. Here is where the real magic happens, now we are going to build the SSDT and the config.plist files.
```
cd ~/Downloads/Projects/probook.git
./build.sh
```
8. These SSDT patches and the config.plist must be installed to the EFI partition of your Drive. To mount it, we need these commands:
```
cd ~/Downloads/Projects/probook.git
./mount_efi.sh
```
9. To install the SSDT patches, type in:
```
cd ~/Downloads/Projects/probook.git
./install_acpi.sh install_8x0g1_haswell
```
10. To install the config.plist, type in:
```
cd ~/Downloads/Projects/probook.git
cp ./config/config_8x0_G1_Haswell.plist /Volumes/EFI/EFI/Clover/config.plist
```
11. Make a copy of the config.plist and edit it with Clover Configurator. Go to the SMBIOS section and from there click on the up and down arrow from the right. Select MacBookAir6,2. At the Serial Number field, there is a Generate New button, hit it a couple of times, just to make sure it's totally random. Save it and close Clover Configurator. 

*Clover Configurator can erase important settings from the config.plist, this is why we don't edit the original config.plist*<img src="/images/cloverplist.png" width=750>
12. Now open the original and the edited config.plist with TextEdit and copy from the edited into the original config.plist. Save it. And reboot!
13. After all this, my sound card wasn't recognized at all, so I had to try other layouts. From the [AppleALC supported codecs site](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs) you can see that layout 3, 12, 13, 33, 84 is supported by IDT92HD91BXX, you have to try each one, to see which is the best for you. The patched SSDTs are located under your EFI partition EFI->CLOVER->ACPI->PATCHED, you have to edit SSDT-8x0G1h.aml file with MaciASL, it's probably the default program to open .aml files.<img src="/images/maciasl.png" width=650>
14. In MaciASL find "layout" with command+F, you will find 2 matches. The default value is ```0x0D```, that means layout 13. Switch both ```0x0D``` after layout to either of these: 
- layout 3 =  ```0x03```
- layout 12 = ```0x0C```
- layout 33 = ```0x21```
- layout 84 = ```0x54```
- For me the layout 84 worked perfectly!
After you made the changes to both value, save it and reboot your computer, to see if it worked or not.

I have tested the following features:
- UEFI booting via Clover
- built-in keyboard (with special function keys)
- built-in trackpad (basic gestures)
- HDMI video/audio with hotplug
- native USB3 with AppleUSBXHCI (USB2 works also)
- native audio with AppleHDA, including headphone
- built-in mic
- built-in camera
- native power management
- battery status
- backlight controls with smooth transitions, save/restore across restart
- accelerated graphics
- wired Ethernet
- Mac App Store working
- Messages/FaceTime, even though I didn't run through the guide 

For Wifi and Bluetooth you have to replace your Wifi half mini PCI card to something that is supported by Apple. The HP 840 G1 doesn't have a Wifi whitelist, so in theory every card should work, but it has to be compatible with MacOS.
