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
* latest MultiBeast should copy the *FakeSMC.kext* to the EFI partition. if not:
  * get latest *FakeSMC.kext* from [here](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/) or use [the version of this repo](boot/RehabMan-FakeSMC-2017-1017.zip)
  * mount EFI partition and copy *FakeSMC.kext* and any other extra necessary kexts to /EFI/CLOVER/kexts/Other/

#### No Audio ####
* after installation of macOS the ALC892 audio-chip does not provide any sound natively
* use [this guide](https://www.tonymacx86.com/threads/applehda-realtek-audio-guide.234732/#post-1606764) to enable sound
  * basically download [this file](audio/audio_clover.zip) (click `view raw`)
  * mount EFI partition
  * execute audio.command file
  * enter password and answer every question with *y*, except of `Patch AppleHDA.kext for HD4600 HDMI audio` (see issue [#1][i1])
  * restart and there will be sound

#### Graphics ####
* check [tonymacx86](https://www.tonymacx86.com) for latest NVIDIA alternate graphics drivers for macOS
* use [Native Display Brightness](https://github.com/KAMIKAZEUA/NativeDisplayBrightness) to adjust the brightness of the screen via F1/F2
  * in addition [FunctionFlip](http://kevingessner.com/software/functionflip/) is necessary to avoid pressing the fn-key  

#### iMessage ####
* use [this guide](https://www.tonymacx86.com/threads/an-idiots-guide-to-imessage.196827/) at tonymacx86 or view the [pdf version](imessage/an_idiots_guide_to_imessage.pdf) of it
* useful tools:
  * [DPCIManager](https://sourceforge.net/projects/dpcimanager/) or use [version from repo](imessage/DPCIManager_ML.zip)
  * iMessageDebugv2 from [here](http://www.tonymacx86.com/attachments/imessagedebugv2-zip.114403/) or [here](imessage/iMessageDebugv2.zip)

#### Mount synology drives ####
* create auto-mounter-file for smb-drives
  ```
  cd /etc
  sudo touch /etc/auto_smb
  sudo chmod +x /etc/auto_smb
  ```
* copy content from [this file](synology/auto_smb) to the newly created `auto_smb` and fill in the correct values for the two lines
  ```
  LOCAL_USER=[local_osx_user]
  SMB_SERVER=[hostname_of_the_synology_nas] 
  ```
* for the credentials of the synology login create the following file
  ```
  touch /Users/local_osx_user/.smb_credentials
  chmod 600 /Users/local_osx_user/.smb_credentials
  ```
* copy over and adjust the content from [this file](synology/.smb_credentials) to the newly created file
* create mount-point
  ```
  sudo mkdir -p /mnt/Storage
  sudo bash -c "echo '/mnt/Storage auto_smb' >> /etc/auto_master"
  sudo automount -vc
  ```
* list all desired directories with
  ```
  cd /mnt/Storage/video
  cd /mnt/Storage/music
  cd [...] 
  ```
* and now you can link the automatedly mounted drives
  ```
  mkdir /Users/local_osx_user/Storage
  for i in `ls -1 /mnt/Storage`; do ln -s /mnt/Storage/$i /Users/local_osx_user/Storage/$i; done
  ```

#### iTunes crash ####
* problem is described [here](https://www.tonymacx86.com/threads/half-success-itunes-12-7-constantly-crashing-random-messages.233135/) 
* solution: 
  * mount EFI partition and copy [lilu.kext](https://github.com/vit9696/Lilu/releases) and [shiki.kext](https://github.com/vit9696/Shiki/releases) to /EFI/CLOVER/kexts/Other/

[i1]: https://github.com/cogidoo/Hackintosh-Setup/issues/1
