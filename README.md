# esp32-project-template

## Overview
This project serves as a template for a project, in which an ESP32 platform is to be used. This guide will guide the users thorugh how to open the project in a docker container, how to pass in a COM-port, and any other ting, which might be relevant.

## Docker Guide

This repo is designed to be opened in a **Dev Container**. However, there ary some steps that needs to be taken, to ensure functionallity and optimal performance.

### Install and setup WSL2
Open Windows PowerShell, and type 
```console
wsl --install
```

Alternatively, this can also be done via the Microsoft Store, where both Windows Subsystem for Linux and Ubuntu 22.04 can be installed.

For a more in depth guide, see: 
https://learn.microsoft.com/en-us/windows/wsl/install

### Install Usbipd
This will have to be done in both Windows and WSL. 

In PowerShell, run the following command: 
```console
winget install --interactive --exact dorssel.usbipd-win
```

In WSL, run the following commands:
```console
sudo apt install linux-tools-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20
```



### Fork this repo to the Linux filesystem


### Attach the USB device to WSL

Attach a USB device
Before attaching your USB device, ensure that a WSL command line is open. This will keep the WSL 2 lightweight VM active.

List all of the USB devices connected to Windows by opening PowerShell in administrator mode and entering the command:

**PowerShell**
```console
usbipd wsl list
```
Select the bus ID of the device you’d like to attach to WSL and run this command. You’ll be prompted by WSL for a password to run a sudo command. The Linux distribution to be attached must be your default distribution. (See the Basic commands for WSL doc to change your default distribution).

**PowerShell**
```console
usbipd wsl attach --busid <busid>
```
Open Ubuntu (or your preferred WSL command line) and list the attached USB devices using the command:

**WSL Bash**
```console
lsusb
```

You should see the device you just attached and be able to interact with it using normal Linux tools. Depending on your application, you may need to configure udev rules to allow non-root users to access the device.

Once you are done using the device in WSL, you can either physically disconnect the USB device or run this command from PowerShell in administrator mode:

**PowerShell**
```console
usbipd wsl detach --busid <busid>
```
### Opening the repo in a dev container
In WSL, navigate to where you would like to clone the project. From here, type:
```console
git clone "GIT REPO URL"
```
When the project has been cloned, enter the project using 
```console
cd "PROJECT FOLDER NAME"
```
From here, type: (Remeber the period ".")
```console
code .
```
This will open VSCode, connected to the remote WSL system, with the project open. To verify that everything is good so far, a blue bar in the buttom left corner saying "WSL: Ubuntu" should be visible. 

VSCode might ask you: **"Folder contains a Dev Container configuration file. Reopen folder to develop in a container"**, with a blue button which says **"Reopen in Container"**. If it does, press this.

If no such message box appears, click **ctrl+shift+space**. This will open a dropdown menu from the top of the window. In this, begin typeing **"Dev Containers: Reopen in Container"**. When it shows up, press enter. 

Either of these things will do the same, and reopen the project in a Docker Container. To verify that it worked,  the blue bar should now be saying "Dev Container: *Project Name*"

Once the Dev Container has been opened, VSCode should automatically install all nessesary extensions. 