# Ubuntu Dual Boot on Dell XPS 15

This document shows the steps for installing ubuntu on Dell XPS 15 on top of Windows, also running it in Dual Boot: Windows as well as Ubuntu

## Table Of Contents

- [Switching Windows from RAID to AHCI](#switching-windows-from-raid-to-ahci)
- [Windows Disk Partition For Ubuntu Installation](#windows-disk-partition-for-ubuntu-installation)
- [Bootable Ubuntu USB Stick Creation](#bootable-ubuntu-usb-stick-creation)
  - [Requirements](#requirements)
  - [Writing to USB Stick using Rufus](#writing-to-usb-stick-using-rufus)
- [Bios Changes For Booting Ubuntu](#bios-changes-for-booting-ubuntu)
- [Post Installation Steps](#post-installation-steps)

## Switching Windows from RAID to AHCI

- Click the Start Button and type Command Prompt.

- Right-click and select Run as Administrator.

- Run the command

```powershell
bcdedit /set {current} safeboot minimal
```

- Restart the computer and once the Dell logo comes up press F12 to enter BIOS Setup.

- From the System Configuration, change SATA Operation mode to AHCI from either IDE or RAID.

- Save changes and exit Setup and Windows will automatically boot to Safe Mode.

- Again run the Command Prompt as Administrator and run the command.

```powershell
bcdedit /deletevalue {current} safeboot
```

- Reboot again and Windows will automatically start with AHCI drivers enabled.

**Note:** This can be cross checked by going to Start Menu -> Device Manager -> IDE ATA/ATAPI controller, this should show the value as
"Inter(R) 100 Series/C230 Chipset Family SATA AHCI Controller"

## Windows Disk Partition For Ubuntu Installation

- On Windows, in the Start Menu search for "Create and Format Hard Disk Partitons"

- Find your C:\ Drive in the list of partitions, right click and select "Shrink Volume".

- Enter the amount of volume you wish to accomodate it to Ubuntu. Once the amount is entered, click on Shrink which will create a new partition of that amount and named as unallocated.

## Bootable Ubuntu USB Stick Creation

### Requirements

- A 4GB/larger USB Stick/flash drive
- Windows XP or later
- [Rufus](https://rufus.ie/), a free open source USB stick writing tool
- [Ubuntu ISO](https://www.ubuntu.com/download/desktop) file.

### Writing to USB Stick using Rufus

- Launch Rufus

- Insert your USB Stick

- Select your USB Stick from the Device drop down list.

- Select FreeDOS from the Boot Selection section as we are creating a bootable Ubuntu.

- Leave the Partition scheme and Target system option as is.

- Click the SELECT button and browse and select the Ubuntu ISO from the directory where it was initially downloaded, the Volume field will be updated to relect the ISO selected.

- Leave rest all fields as is and click START to initialise the write process.

- You may be alerted that Rufus requires additional files to complete writing the ISO, select Yes.

- You may again be alearted with a dialogue box ISOHybrid image detected, click the first radio buttion "Write in ISO Image mode (Recommended)"

- You will also be warned about all the data currently on the USB stick to be deleted, click OK.

- The ISO will now be written to the USB stick and progress can be seen from the bar.

- When Refus has finished writing, Status will be filled in green with the word READY. Select CLOSE to complete the process.

## Bios Changes For Booting Ubuntu

- Reboot Windows and press F12 once the Dell logo appears which will take you to Bios menu

- Go to Secure Boot -> Secure Boot Enable and Disable secure boot.

- Go to General -> Advanced Boot Options and tick the checkbox "Enable Legacy Options ROMs"

- Connect the USB Stick and then Save and Apply and Exit.

- Your system will reboot and again press F12 like before and select your media under the "UEFI Boot" section.

- This will boot the Ubuntu OS and follow the steps along.

**Note:** Check "Install third party softwares" on the install screen. Also, Select the first option when asked for Ubuntu installation which states that install along with the current Windows 10 (dual boot) which is auto detected by Ubuntu.
For more info: [Install Ubuntu Desktop Documentation](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop#0)

## Post Installation Steps

- Once Ubuntu is installed, you can eject your USB Stick. The OS will ask you to Reboot. At this stage the dual boot is ready but some post installation steps for ubuntu are required to be performed.

- Once rebooted, on the Grub Bios screen where it asks to select the OS you want to log into, on ubuntu hover press `e` and in the bios parameter on the second last line enter:

```shell
nouveau.modeset=0
```

**Note:** This is required because serveral versions of Ubuntu freeze on Dell XPS 15.

- Now press `F10` which will boot Ubuntu and provide you with the login screen.

- On the login screen, go to the command line by using `Cltr` + `Alt` + `F3` and install following dependencies:

```shell
# Install nvidia driver
sudo apt-get install nvidia-384

# Install graphics
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update

sudo reboot

---------------------- Only If Required -----------------------

# Fix Touchpad
sudo apt-get install xserver-xorg-input-libinput
sudo apt-get remove --purge xserver-xorg-input-synaptics

# Improved Battery Life
sudo apt install update
sudo apt install tlp tlp-rdw powertop
sudo tlp start
sudo powertop --auto-tune
sudo reboot
```

- Once all these dependencies are installed, you are good to go with dual boot Windows and Ubuntu! Enjoy!!!