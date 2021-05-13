# MRArch Install and Setup

author: Liam Timms
date: 9/18/19
revised: 5/12/21

## Introduction

The purpose of this guide is to help guide a new user through the process of setting up Arch Linux for MRI or Neuroimaging research (a project I've been calling "MRArch"). Personally, I use Arch Linux as my primary OS running on my hardware but I don't recommend brand new users take that plunge without experimenting in a virtual machine (VM) first. The project has two short scripts: `mrarch` and `mrarch2` which are agnostic to the underlying installation method or hardware.

## Install Virtual Box

While you can use whatever virtual machine software you are familiar with or install on bare metal, I've written a short guide here to help set-up Virtualbox. Virtualbox is free for non-commercial use, cross-platform and provides a good set of options/functionality.

### Download

- Windows: https://www.virtualbox.org/
- Linux: it's in every big distro's repository (use `apt install`, `pacman -S`, etc.)

### Install

- Windows: go through installer
- Linux: your package manager will take care of it.

## Set up the Virtual Machine (VM)

### Download

1. Go to https://www.archlinux.org/download/
2. Get most recent ISO (using the torrent magnet link will give you the fastest download if you have torrent software already).

### Make a VM

1. Open virtualbox
2. Click "new", each remaining item in this list corresponds to _one screen_ of options
3. **Name**: a name for your virtual machine (avoid capital letters and spaces), **Type**: Linux, **Version**: Arch Linux (64-bit)
4. **Memory size**: at least 4GB but more is better if you have RAM to spare on your machine.
5. click **create virtual hard disk**.
6. pick **VDI**.
7. choose **Dynamically allocated** (keeps the file from being larger than necessary).
8. For **size**: enter at least 50GB, the full suite of neuroimaging analysis software is quite large. Larger is generally better as you will not use the full size on your host machine if you set it to be dynamically allocated.
9. Next we need to open **Settings** by right clicking on the VM we just made.
10. To speed up the install process, click on **System** -> **Processor** and increase the number of virtual cores before proceeding. The number of virtual cores must be less than the total real cores on your real CPU hardware.
11. Next in **Settings** you should go to **Display** and change the _Graphics Controller_ option to be `VBoxVGA`.
12. Finally in **Settings** check the box to enable UEFI booting. This step is important for running the easy install script for Arch.
13. Finish and hit the green start arrow to boot the VM.

### Boot into the live environment

When prompted to pick an image to boot, choose the Arch `.iso` file you downloaded earlier, then choose to start the live environment. The live environment allows us to make changes formatting changes to the new VM disk and install the OS. Make you start in UEFI mode. It will then dump you into a commandline (technically `zsh`) shell environment with a cow giving some advice at the top.

## Installing Arch

### Option 1: Installing Arch manually (the classic way)

For official documentation on installing and setting up Arch Linux see the wiki page here: https://wiki.archlinux.org/title/installation_guide .
I strongly recommend skimming over that page before continuing as it will always be the most up-to-date guide and will always provide the most detail and power to customize your setup.

### Option 2: Installing with the official Archinstall script

At the prompt type `archinstall`. The guided installer will now start. The ability to use this installer is the reason we made sure to setup UEFI booting earlier as (as of 5/21) it does not support Legacy BIOS booting.

The installer is straightforward for the most part but I do strongly recommend choosing the "desktop" profile and then selecting GNOME as your desktop environment. This will give you the
most widely supported out-of-the-box functional experience. For the graphics driver, choose `vmware` if you are installing in a VM.

At the prompt to install additional packages I recommend typing:

```
firefox gnome-terminal base-devel virtualbox-guest-utils
```

After the install completes, reboot.

### Option 3: Installing with Archfi

Archfi is a 3rd party (not officially supported) install script but is reliable in my experience. This is a fallback option for cases where you are unable to boot in UEFI mode to use the official install script and also not interested in doing a more customized manual install. The following is geared at Virtualbox installs.

#### Archfi Initialization

1. **Language**: English
2. **Keyboard Layout**: us
3. **Editor**: `vim` if you know what how to use it or `nano` if you want something simple but less powerful
4. **Disk Partitions**: Auto Partitions (dos), sda
5. **Select Partitions and Install:**
   - _select boot device_: /dev/sda1
   - _select swap device_: /dev/sda2
   - _select root device_: /dev/sda3
   - _select home device_: none
   - Yes
6. **Format Devices**:
   - _Select partition format for boot_: Fat32
   - _Select partition format for swap_: swap
   - _Select partition format for root_: ext4
7. **Mount Install or Config**
   - **Edit mirror list**: this opens the full mirror list using the editor we chose earlier (suggested vim), through a method of your choice move cc.columbia.edu to the top of the list and you may potentially wish to also move up NYU or comment out all the non-US servers (this step is Boston specific)
     - this step may not be necessary for installs after August 2020.
8. **Install arch linux pacstrap base**: this now actually installs arch onto the VM disk

We now have a base system with almost nothing installed; we have no desktop, minimal little software, no users, no password, no specialized configurations. These next steps will set up all of that.

#### Archfi Chroot

1. **Set Computer Name**: best to use the same name as what you named the virtual machine at the begining of its set-up (this is why I said no caps and spaces)
2. **Keyboard Layout**: us
3. **Set Locale**: en_US
4. **Set Time**:
   - America, New York
   - _Use UTC hardware clock_: Yes
5. **Set root password**: a password you will remember which is short enough to enter very often
6. **Generate fstab**: UUID genfstab -U (this is about the file system table using the device ID to find things... just do the first option)
7. **Bootloader**: grub, install grub (installs the program "grub"), install bootloader grub-install (actually makes a bootloader (thing that loads the OS)), back
8. **Enable dhcpcd**: Yes
9. Do not click **archdi**, this will put you into a desktop environment set up and start installing a bunch of programs, instead click **Back**
10. **Unmount**, **Back**, **Reboot**
11. the reboot will take you back to the live environment so at the start screen choose **Power off**

#### booting into Arch after Archfi

1. with the virtual machine now off, go choose **Settings**, click **Storage**, click on the Arch ISO, **Remove from Drive**, hit OK
2. start the virtual machine,
3. **login**: root
4. **Password**: the one you choose earlier
5. Congrats we are now in a totally fresh Arch Linux installation

### Setting up your user account
This is taken care of by the `archinstall` script method and can be skipped.

1. Install git and sudo: `pacman -S git sudo`
2. because we are using virtualbox we will add a virtualbox user group: `groupadd vboxsf`
3. make a normal user account for yourself: `useradd -m -G wheel,vboxsf <your new username>`
4. set password, I recommend you just set it to be the same as the root password you chose during install so you don't have to remember two passwords to use the VM: `passwd <your new username>`
5. edit the sudoers file to allow _your new user_ to "sudo": run `visudo` then uncomment `%wheel ALL=(ALL) ALL` to unlock sudoing for the `wheel` Group (which includes your new user)
6. logout of root: logout
7. login with your new account. Recommendation: before continuing, go to **machine** at the top of the VM's window and click "take snapshot", this will allow us to revert back to this clean state if we wish at a later point.

## Set up MRArch

### Running the first script

1. clone the MRarch repository so that you can run my install script for all the neuroimaging and desktop: `git clone https://github.com/liamtimms/mrarch.git`
2. `cd mrarch` to go into the folder you just downloaded then run: `./MRArch` to start the script
3. enter password, hit enter to continue as needed
4. when confronted with a ccmake prompt (will look different from the normal prompt), hit `c` , this will do an initial configuration for the compilation of ANTS (https://github.com/ANTsX/ANTs), hit `c` again, then press `g`
5. wait a long time as a lot will need to download, install and sometimes also compile
6. after the install script you should be able to boot into the GNOME desktop environment (DE), try rebooting now and login

### Install MATLAB

Because MATLAB is proprietary software whose obnoxious licsence prohibits installation through a package manager; you need to install it separately from all the other programs via a manual approach.

1. Go to the mathworks website and sign in with your NEU (or other school) account
2. download the MATLAB installer for Linux
3. unzip the MATLAB installer
4. navigate to the newly unzipped file in the terminal and run `sudo ./install`
5. login again with your MathWorks account
6. **DO NOT USE THE DEFAULT INSTALLATION FOLDER** instead choose: `/opt/matlab/R<edition of matlab>` so for when this was written I input `/opt/matlab/R2019b` this is important because it will allow SPM12 and Nipype to successfully interface with MATLAB.
7. In installing MATLAB, please select to also install the MATLAB Coder, MATLAB Complier and MATLAB SDK, if you receive an error indicating that you do not have enough space in `/tmp/` uncheck some of the other add-ons that are selected by default until you are able to install it. You can always install additional add-ons later by starting MATLAB with `sudo matlab` and then selecting addons.
8. Activate MATLAB and sign in for a 3rd time or just type your user account name depending on the prompt

### MRArch2

1. `yay --editmenu -S spm12` change the `_BUILDMATLAB` variable to true
