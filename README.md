# esp32-project-template <!-- omit in toc -->


## Table of Contents
- [Table of Contents](#table-of-contents)
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Using this template to create a new project](#using-this-template-to-create-a-new-project)
  - [Setting up the project](#setting-up-the-project)
    - [Docker](#docker)
    - [PlatformIO](#platformio)
- [Using Docker](#using-docker)
  - [Cloning the project into WSL](#cloning-the-project-into-wsl)
  - [Attaching the ESP32 COM-port to WSL](#attaching-the-esp32-com-port-to-wsl)
    - [Troubleshooting](#troubleshooting)
  - [Opening the project in a Dev Container](#opening-the-project-in-a-dev-container)
- [Resources](#resources)

## Overview

This project serves as a template for a project, in which an ESP32 platform is to be used. This guide will guide the users thorugh how to open the project in a docker container, how to pass in a COM-port, and any other ting, which might be relevant.

## Prerequisites

For this to work, it is nessesary to have a few tools installed. How to install these things will all be described here.

- **Visual Studio Code**  
https://code.visualstudio.com/download

- **Visual Studio Remote Extensions Pack**  
https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack

- **Docker Desktop**  
https://www.docker.com/products/docker-desktop/

- **WSL2 & Ubuntu**

    Open Windows PowerShell, and type

    >       $ wsl --install

    This will install wsl, and the default Linux distribution, which is Ubuntu.
- **Usbipd**

    This will have to be installed in both Windows and WSL.

    In PowerShell, run the following command:
    >       $ winget install --interactive --exact dorssel.usbipd-win    

    In WSL, run the following commands:

    >       $ sudo apt install linux-tools-generic hwdata
    >       $ sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20

## Using this template to create a new project

In Gitea, select "Use this template to create a new project". or something like that...


### Setting up the project

A few configuration should be made when first creating the project. This includes nameing, platform selection and adding any additional dependencies which might be needed for the specific project. 

#### Docker

To begin with, some configurations should be made to the *.devcontainer/devcontainer.json* file. This is the file which holds the configuration for which docker image is used, which extensions is installed in VSCode, and what the name of the container/workspace will be.

First of all, the name of the devcontainer should be changed to reflect the project name. Per default it is set as *esp32-template-project*, but this should be changed.

```json
"name": "esp32-template-project",
```


Then, if other extensions are desired and/or needed, these can be added to the JSON block shown below, which is found in the devcontainer.json file.

```json
"customizations": {
    "vscode": {
      "extensions": [
        ...
      ]
    }
}
```

All the extension specified here will automatically be installed, when the container is built. If any changes are made to this file, VSCode will prompt the user to rebuild the container, to make the changes take effect. This can however have some side effects. When rebuilding the container, all manually installed extensions, which has not been added to devconatiner.json, will be removed, since it is not specified that they are needed. Also, since devcontainer.json is tracked in git, if one user updates the extension list, the next time someone else fetches the updated version, they will also be promptet to rebuild their container, loosing whatever extensions they might have installed manually. The final annoyance with this, is that some of extenesions (PlatformIO) takes a little while to install and configure.

#### PlatformIO

The PlatformIO configuration might also need to be updated, to reflect the specific platform and framework used in this project. This is done in the platformio.ini file. Here, platform and framework information, programming and monitoring configurations, build options, library dependencies and other things can be specified. It is also possible to write multiple configurations, which all builds ontop of a base configuration, for easy switching between different run modes.

For more information and examples, see PlatformIO's own documentation: https://docs.platformio.org/en/latest/projectconf/index.html




## Using Docker

This repo is designed to be run in a **Dev Container**. However, there ary some steps that needs to be taken, to ensure functionallity and optimal performance.

### Cloning the project into WSL

To achevie the optimal performance, the project should be cloned directly into WSL. This can easily be done, by launcing the WSL console, navigating to the directory where you would like to place the git-repo, and executing the command:

```console
$ git clone REPO_URL
```

This might ask for credentials? Maybe git is passed from windows git configuratation, like in docker? will need to check.

Once the repo has been cloned to the Linux file system, you are ready for the next step. This is the only git operation that needs to be done from the CLI. From here on, every operation (commits, fecth, push, branch, merge etc.) can be done via the git tools in VSCode.

### Attaching the ESP32 COM-port to WSL

To enable flashing and monitoring of the ESP device from the Dev Container, it is nessesary to pass in access to the Windows COM-port, to which the ESP is connected. This is achived with the program Usbipd, installed in the beginning of the guide. This needs to be done, before openeing the Dev Container, otherwise the port will not be transfered over. The WSL terminal can be open however, since this will just register it as a USB being plugged in. 

To pass in a COM-port, you need to open Powershell and follow the following steps. Lines prepended with ***$*** is input, the rest is responses from the PowerShell terminal.

```console
## Print a list of connected devices, to see which busid they have.
$ usbipd
BUSID  VID:PID    DEVICE                                                        STATE
1-2    10c4:ea60  Silicon Labs CP210x USB to UART Bridge (COM22)                Not shared
1-3    0489:e0d8  RZ616 Bluetooth(R) Adapter                                    Not shared
...    ...        ...                                                           ...

## Attach the ESP (here on busid 1-2) to the WSL. The -a option is for automatically reattaching when the device is reconnected?
$ usbipd wsl attach --busid 1-2 -a
```

To verify that the COM-port has been forwarded into WSL, navigate to the ***/dev*** folder in the WSL terminal and type in ```ls tty*```:

```console
/dev$ ls tty*
tty    tty12  tty17  tty21  tty26  tty30  tty35  tty4   tty44  tty49  tty53  tty58  tty62  ttyS0
tty0   tty13  tty18  tty22  tty27  tty31  tty36  tty40  tty45  tty5   tty54  tty59  tty63  ttyS1
tty1   tty14  tty19  tty23  tty28  tty32  tty37  tty41  tty46  tty50  tty55  tty6   tty7   ttyS2
tty10  tty15  tty2   tty24  tty29  tty33  tty38  tty42  tty47  tty51  tty56  tty60  tty8   ttyS3
tty11  tty16  tty20  tty25  tty3   tty34  tty39  tty43  tty48  tty52  tty57  tty61  tty9   ttyUSB0
```

This will print something like the above example. If the one called ***ttyUSB0*** is present, the COM-port is activly passed into WSL.
#### Troubleshooting

The usbipd might complain, that the device needs to be force binded (check the actual message). In this case, PowerShell needs to be closed and reopened as Administrator. When in adminstrator mode, the following command can be used to solve the issue:

```console
$ usbipd bind --busid 1-2 --force
```

### Opening the project in a Dev Container

Once the project has been cloned from the git-repo, and the COM-port has been passed into WSL, it is time to actually open the project, and start working.

The first thing you need to do, is to make sure that the Docker Engine is running. This can be done by starting Docker Desktop, and from here the engine should start automatically. If it dosen't, press the playbutton in the buttom left corner. 

In the WSL terminal, navigate to the project folder, which has been cloned from git. From here type in "```code .```" This will open the current folder in a remote session in VSCode. To verify that the folder is actually open in a remote session, the buttom left corner should have a blue bar saying: ***WSL: Ubuntu***. After the project has been opened, VSCode will automatically show a message box in the buttom right corner, asking if you want to reopen the current folder in a Dev Container. This is exactly what we want to do, so press the blue button in the messagebox, saying ***Reopen in Container***. If you miss this messagebox, you can press ***Ctrl+Shift+Space***, which opens a command bar in the top of the window. In this command bar start typing ***Dev Container: Reopen in Container***.

If it is the first time you open the folder in a Dev Container, the container has to be built. This can take a while, since Docker first have to build the Image from the Dockerfile, and the configure the Dev Container with all the specified extensions. You might have to reload the VSCode window multiple times. Each time, a message box with a button for doing so will appear in the buttom right corner. If you miss it, and still need to reload, an extension for this has been installed, adding a ***Reload*** button to the buttom right toolbar. 

When reopening a recent project, you can right click the VSCode icon, and then select the repo in WSL. You cannot directly reopen the project in a Dev Container, since this will not pass through the COM-port.





## Resources
- WSL installation guide:   
https://learn.microsoft.com/en-us/windows/wsl/install


- Usbipd intallation and usage guide:    
https://learn.microsoft.com/en-us/windows/wsl/connect-usb

- PlatformIO .ini file docs:   
https://docs.platformio.org/en/latest/projectconf/index.html