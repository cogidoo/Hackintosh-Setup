# Hackintosh-Setup

#### Current hardware
* Gigabyte GA-B85M-D3H LGA Sockel 1150 Micro ATX
* Intel i5 4460 3,2 GHz
* Crucial Ballistix Sport XT 8GB
* Gigabyte 760 GTX Windforce OC Version with 2GB
* Samsung EVO 850 with 256GB
* be quiet! BQT L7-530W
* Apple Keyboard
* Apple Trackpad
* Dell U2515H
* Harman Kardon SoundSticks III
* Bluetooth Adapter Dongle, GMYLE Nano USB Broadcom BCM20702

#### Install macOS from USB
* download macOS via App Store or...
* download installer with [this tool](http://dosdude1.com/highsierra/)
* create bootable USB-installer with [Unibeast](http://www.unibeast.com/)
  * UEFI boot mode
  * no graphics checked
* put [Multibeast](http://www.multibeast.com/) at the USB drive
* *optional:* put [Little Snitch](https://www.obdev.at/products/littlesnitch)
* install macOS
* after install, use Multibeast with the following configuration:
  ```
  Quick Start > Clover UEFI Boot Mode
  Drivers > Misc > FakeSMC
  Drivers > Network > Realtek > RealtekRTL8111 v2.2.1 
  Drivers > USB > 7/8/9 Series USB Support 
  Bootloaders > Clover UEFI Boot Mode
  Customize > System Definitions > iMac > iMac 14,2 
  Drivers > Graphics > NVIDIA Web Drivers Boot Flag
  ```
  * select _no_ audio driver
  * with the nvidia-flag it is possible to install macOS-graphics-driver from nvidia 

#### Clover Bootloader and EFI parition
* use [clover configurator](http://mackie100projects.altervista.org/download-clover-configurator/)

#### No Audio ####
* after installation of macOS the ALC892 audio-chip does not provide any sound natively
* use [this guide](https://www.tonymacx86.com/threads/applehda-realtek-audio-guide.234732/#post-1606764) to enable sound
  * basically download [this file](audio/audio_clover.zip) (click `view raw`)
  * mount EFI partition
  * execute audio.command file
  * enter password and answer every question with *y*, except of `Patch AppleHDA.kext for HD4600 HDMI audio` (see cogidoo/Hackintosh-Setup#1 )
  * restart and there will be sound
