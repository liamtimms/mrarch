# MRArch Install and Setup
author: Liam Timms
date: 9/18/19
---

# Install Virtual Box
## Download
* Windows: https://www.virtualbox.org/
* Linux: it's in your repository (apt install, pacman -S, etc.)

## Install
* Windows: go through installer
* Linux: package manager will take care of it

# Set up the Virtual Machine (VM)
## Download

1. Go to https://www.archlinux.org/download/

2. Get most recent ISO

## Make a VM
1. Open virtualbox
2. click "new", each remaining item corresponds to one screen of options
3. **Name**: a name for your virtual machine AVOID CAPS AND SPACES, **Type**: Linux, **Version**: Arch Linux (64-bit)
4. **Memory size**: at least 4GB but up to 50-60% of your total RAM if you don't plan to run much concurrently with the VM.
5. **create virtual hard disk**
6. **VDI**
7. **Dynamically allocated** keeps the file from being huge
8. For **size**: enter at least 50GB, the full suite of neuroimaging analysis software is quite large, Ideally the VM should be closer to 100GB (though you will not use 100GB on your host machine if it is dynamically allocated)
9. to speed up the install process, it is a good idea to go into the **Settings** for the new VM, click on **System** -> **Processor** and increase the number of virtual cores available to the VM
10. Next in **Settings** you should go to **Display** and change the *Graphics Controller* option to be `VBoxVGA`
11. finish and hit the green start arrow to boot the machine

## Boot into the live environment
1. Choose the arch ISO you downloaded to boot from when prompted, choose to start the live environment
2. The live environment allows us to make changes formatting changes to the new VM disk
3. to speed things up we will use an install script: https://github.com/MatMoul/archfi , follow the instructions for downloading the script and running it in the live environment as explained on the page for that project.
5. For detailed help or background information about the install process (instructions for the full install without the archfi script) see: https://wiki.archlinux.org/index.php/Installation_guide

## Installing with Archfi in the VM
1. **Language**: English
2. **Keyboard Layout**: us
3. **Editor**: vim probably
4. **Disk Partitions**: Auto Partitions (dos), sda
5. **Select Partitions and Install:**
    * *select boot device*: /dev/sda1
    * *select swap device*: /dev/sda2
    * *select root device*: /dev/sda3
    * *select home device*: none
    * Yes
6. **Format Devices**:
    * *Select partition format for boot*: Fat32
    * *Select partition format for swap*: swap
    * *Select partition format for root*: ext4
7. **Mount Install or Config**
    * **Edit mirror list**: this opens the full mirror list using the editor we chose earlier (suggested vim), through a method of your choice move cc.columbia.edu to the top of the list and you may potentially wish to also move up NYU or comment out all the non-US servers (this step is Boston specific)
8. **Install arch linux pacstrap base**: this now actually installs arch onto the VM disk

## After the Install

We now have a base system with almost nothing installed, we have no desktop, very little software, no users, no password, next we will set up all of that

1. **Set Computer Name**: best to use the same name as what you named the virtual machine at the begining of its set-up (this is why I said no caps and spaces)
2. **Keyboard Layout**: us
3. **Set Locale**: en\_US
4. **Set Time**:
    * America, New York
    * *Use UTC hardware clock*: Yes
5. **Set root password**: a password you will remember which is short enough to enter very often
6. **Generate fstab**: UUID genfstab -U (this is about the file system table using the device ID to find things... just do the first option)
7. **Bootloader**: grub, install grub (installs the program "grub"), install bootloader grub-install (actually makes a bootloader (thing that loads the OS)), back
8. **Enable dhcpcd**: Yes
9. Do not click **archdi**, this will put you into a desktop environment set up and start installing a bunch of programs, instead click **Back**
10. **Unmount**, **Back**, **Reboot**
11. the reboot will take you back to the live environment so at the start screen choose **Power off**

## Booting into Arch for Real
1. with the virtual machine now off, go choose **Settings**, click **Storage**, click on the Arch ISO, **Remove from Drive**, hit OK
2. start the virtual machine,
3. **login**: root
4. **Password**: the one you choose earlier
5. Congrats we are now in a totally fresh, minimal arch Install

## Setting up your user account
1. Install git and sudo: `pacman -S git sudo`
2. because we are using virtualbox we will add a virtualbox user group: `groupadd vboxsf`
2. make a normal user account for yourself: `useradd -m -G wheel,vboxsf <your new username>`
3. set password, I recommend you just set it to be the same as the root password you chose during install so you don't have to remember two passwords to use the VM: `passwd <your new username>`
4. edit the sudoers file to allow *your new user* to "sudo": run `visudo` then uncomment `%wheel ALL=(ALL) ALL` to unlock sudoing for the `wheel` Group (which includes your new user)
4. logout of root: logout
5. login with your new account. Recommendation: before continuing, go to **machine** at the top of the VM's window and click "take snapshot", this will allow us to revert back to this clean state if we wish at a later point.

# Set up MRArch
## Running the first script
1. clone the MRarch repository so that you can run my install script for all the neuroimaging and desktop: `git clone https://github.com/liamtimms/mrarch.git`
2. `cd mrarch` to go into the folder you just downloaded then run: `./MRArch` to start the script
3. enter password, hit enter to continue as needed
4. when confronted with a ccmake prompt (will look different from the normal prompt), hit `c` , this will do an initial configuration for the compilation of ANTS (https://github.com/ANTsX/ANTs), hit `c` again, then press `g`
4. wait a long time as a lot will need to download, install and sometimes also compile
5. after the install script you should be able to boot into the GNOME desktop environment (DE), try rebooting now and login

## Install MATLAB
Because MATLAB is proprietary software whose obnoxious licsence prohibits installation through a package manager; you need to install it separately from all the other programs via a manual approach.

1. Go to the mathworks website and sign in with your NEU (or other school) account
2. download the MATLAB installer for Linux
3. unzip the MATLAB installer
4. navigate to the newly unzipped file in the terminal and run `sudo ./install`
5. login again with your MathWorks account
6. **DO NOT USE THE DEFAULT INSTALLATION FOLDER** instead choose: `/opt/matlab/R<edition of matlab>` so for when this was written I input `/opt/matlab/R2019b` this is important because it will allow SPM12 and Nipype to successfully interface with MATLAB.
7. In installing MATLAB, please select to also install the MATLAB Coder, MATLAB Complier and MATLAB SDK, if you receive an error indicating that you do not have enough space in `/tmp/` uncheck some of the other add-ons that are selected by default until you are able to install it. You can always install additional add-ons later by starting MATLAB with `sudo matlab` and then selecting addons.
8. Activate MATLAB and sign in for a 3rd time or just type your user account name depending on the prompt

## MRArch2

1. `yay --editmenu -S spm12` change the `_BUILDMATLAB` variable to true

