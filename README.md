Hackintosh 10.14.1 guide for HP Elitebook 840 G1

This is a guide for HP Elitebook 840 G1 with macOS Mojave 10.14.1 Vanilla install, based on RehabMan's [HP guide](https://www.tonymacx86.com/threads/guide-hp-probook-elitebook-zbook-using-clover-uefi-hotpatch.261719/).

My 840 G1 config:
- **CPU**: i5-4210U 
- **SSD**: ADATA 128GB
- **RAM**: 8GB DDR3
- **DISPLAY**: 1600x900 (non-touch)
- **GPU**: Intergrated, Intel HD Graphics 4400
- **AUDIO CODEC**: IDT92HD91BXX
- **ETHERNET**: Intel I218-LM
- **WIFI**: Intel Dual Band Wireless-N 7260NB 802.11 a/b/g/n (2 x2 ) WiFi

Here you can check the different models: https://support.hp.com/lv-en/document/c03961746

## Creating the USB Installer
> Prerequisites: atleast 8GB+ pendrive, mine was USB2, but in theory USB3 going to work just fine too. Based on experiences, it's more likely to work without problems on USB2. Format it to MacOS Extended (Journaled) and GUID Partition Map.
Sadly you can't use a Virtual MacOS to create the USB, because it makes corrupted installers.

I think this one is the hardest part. You need the Mojave installer file from the AppStore, but here comes the problem, it's only accessible from Macs. There is two thing you can do: 
1. Look for a friend, family member, anyone who has a real mac and ask them to make a bootable USB for you. 
   - Follow [this tutorial](https://www.imore.com/how-create-bootable-installer-mac-operating-system). 
 From this part, you can do everything in a VM MacOS!
   - After you are successfully created the bootable USB, install [Rehabman's Clover bootloader](https://bitbucket.org/RehabMan/clover/downloads/) on the USB. 
   
   
   
   
2. Download and install a Hackintosh dmg file, I think hackintoshzone
