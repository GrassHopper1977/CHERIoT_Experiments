# Windows Development Environment and Setup
At teh time of writing, there are several repositories being maintained by different groups. They are all forked from [CHERIoT-Platform/cheriot-rtos](https://github.com/CHERIoT-Platform/cheriot-rtos). It can be difficult to work out which repositiory you should be using. Our current understanding is:
* CHERIoT-RTOS seem to be working on the operating system. The version also builds for other targets, such as the simulator (which we haven't played with yet).
* lowRISC seem to be working on the hardware. The lowRISC repos and being updated for new version of the FPGA's bitfile (the CBHERI processot running in the FPGA) and for the Sonata Demo Board. If you want to use the latest hardware then these are the branches to use.
At the time of writing, lowRISC had published a bitfile for V1.0 but the CHERIoT repos are still working on V0.2.
  he plan is for the repositories to merge with the release of V1.0.
  
## Which Repository Should we Use?
There is some discussion about this in this blog post from November 2024: https://cheriot.org/rtos/sonata/git/2024/11/08/navigating-the-ecosystem.html
From our point-of-view it depends on what you are wanting to do (at the time of writing):
* If you need teh external IO e.g. RPi header and the Pin Multiplexor (Pinmux) then you will need to use [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software) following our instructions [here](#installing-the-lowriscsonata-software-development-environment-on-windows).
* If you want to play with the build-in hardware: LCD, switches, joystick, RGB LED, status LEDs, networking (but not SD Card yet) then you can use [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos) repository that we discuss [here](#installing-the-cheriot-platformcheriot-rtos-development-environment-on-windows).

Our project requires accessing CAN-FD modules using the two SPI modules on the Raspberry Pi header. This means that we need access to the RPi header and to the Pinmux to make it work so we are now using [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software). In terms of usability, I currently have a preference for the Nix based system of the [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software) repository to work very well.

## Sources of help and information
1. [Getting started with the Sonata board](https://lowrisc.github.io/sonata-system/doc/guide/index.html) This is the main documentation for the Sonata board and should be kept up to date.
2. [Sonata Software - Getting started guide](https://lowrisc.github.io/sonata-software/doc/getting-started.html) This is the main documentation for the Sonata Software and should be kept up to date. This also includes a section on installing the IDE on Windows (using the Windows Subsystem for Linux).
3. [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) This is the first one that I tried and it didn't work for me on Windows (it worked on an Ubuntu Virtual Machine though). I was unable to get it to build and it assumed that we had some other packages installed already. Do read this one carfully though, you'll notice that it uses the V0.2 firmware. If you try to use V0.4.1 then the files that you build will not work! All that is wrong with this for Windows is that you can't use VSCode to clone the repo - you have to use Git from WSL (see below for the [full instructions](#building-the-code-examples)).
4. [Navigating the CHERIoT Ecosystem](https://cheriot.org/rtos/sonata/git/2024/11/08/navigating-the-ecosystem.html)

## Installing WSL
We need to install the Windows Subsystem for Linux.
1. From the Start menu open Windows PowerShell and run it as Administrator.
2. On a recent version of Windows it will defaul to installing Unbuntu when you run a following command. If you have any issues look at Mircosoft's detailed instructions that can be found [here](https://learn.microsoft.com/en-us/windows/wsl/install).
```
wsl --install
```
3. Wait for the install to finish. It may take a long time.
4. After install, reboot your machine for it to finish the install.
5. Click Start and find Ubuntu, and click it to start.
6. The first time it will ask about a Unix username and password. We may need this for later.

# Installing the CHERIoT-Platform/cheriot-rtos Development Environment on Windows
I'm not ashamed to admit that I use Visual Studio Code on Windows as my preferred IDE (I just have too many other programs on there to be constantly switching back and forth with Linux). There are some user guides for setting up the Sonata Board and the IDE but they don't all appear to be updated. I'm just documenting everything here so that I don't forget it for next time. Sadly, it's not as easy as cloning from Visual Studio Code as that doesn't work, instead we have to clone from Windows Subsystem for Linux.

## Installing on Windows
We attempted to get the toolchain running on Windows following the [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) and found that it wasn't building and the connection to the drive wasn't working. A suggestion from [this page](https://github.com/orgs/CHERIoT-Platform/discussions/245) suggested using WSL (Windows Subsystem for Linux) for some parts of the install, so that's what we will be attempting. We're also going to attempt this on a fresh Windows 11 install to make life a bit easier. The WSL installation is discussed [here](#installing-wsl).

### Other Packages to have Installed
1. Got to [Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/) and follow the instructions.
2. Install Microsoft Visual Studio Code if you don't already have it.
3. Turn on Windows Developer mode as described [here](https://learn.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development). We're not 100% sure it is needed but it enables symlinks, which may be needed.

### Setting Up the Toolchain
1. If you haven't installed WSL go back and do that.
2. Make sure that Docker is already running (open it from the Start menu).
3. Got to Start menu, type "Ubuntu" and open it.
4. We're just going to make sure that the line ending don't get messed with by excuting: `git config --global core.autocrlf false`
5. You will be in your home folder. Create a folder for GitHub repos, move to that folder and clone the code there, move into that folder and run VSCode:
```
mkdir github
cd github
git clone --recursive https://github.com/CHERIoT-Platform/cheriot-rtos.git
cd cheriot-rtos/
code .
```
6. The last command may install a package in order to be able to launch VSCode before it continues but it will launch VSCode.
7. Visual Studio Code will ask you if you want to `Reopen in container` so select OK. Note: If Docker isn't running, it will return an error.
8. Contecting to the container can take a while the first time that you do it.
9. You shouldn't get a wrning about unsafe repositories if you open it this way (if you just open it through Windows then you will - it probably won't work if this happens, even if you accept the repositories as safe).
10. We're just going to check that we can build first. Go to the terminal at the bottom of the screen and enter the following:
```
cd tests/
xmake config --sdk=/cheriot-tools --board=sonata
xmake
xmake run
```
11. You now will find a firmware.uf2 file in the foldr that can be manually copied onto your Sonata board.
12. We should be able to do better than that though by getting VSCode to mount the Sonata baord and automatically copying the file across after compiling.
13. FROM HERE IT IS NOT YET TESTED - We still haven't got the mount working properly on Windows.
14. Now we need to edit the `.devcontainer\devcontainer.json` file (look in the solution explorer on the left of the screen).
15. We want to add a mount section to the end of the file to allow us to automatically deploy any projects that we build.
16. The code that we are adding should look something like this:
```
"mounts": [
    "source=/Volumes/SONATA,target=/mnt/SONATA,type=bind"
]
```
17. `source` should remain as `/Volumes/SONATA`
18. `target` should be altered to point to the SONATA device. In my case it is mounted as drive `D`. If we place `D:` here it fails. We should be able to create a mount point here to point to `D:` and get around this issue.

## Installing on Windows with a Virtual Machine
We used Ubuntu V24.04 for this.
1. Download & install Python (needed for VirtualBox).
2. Reboot the computer.
3. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
4. Start VirtualBox (it will probably start automatically).
5. There's detailed instructions on installing Ubuntu on VirtualBox [here](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview).
6. Log into the virtual Ubuntu (mine is generating lots of errors).
7. Open the terminal and type: `sudo apt update && sudo apt upgrade -y`
8. Now type: `sudo snap refresh` (If asked to reboot, wait until the snap update has finished installing.)
9. Open the terminal to install GIT for Linux:
```
sudo apt-get install git
```
10. There is an install menu. You want to install Visual Studio Code (the name is abbreviated to Code).
11. Install Docker too. We found the version from the package manager didn't work with VSCode. Instead from the terminal:
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```
12. There a note saying that Docker will only be availabel to root users. To change this we need to do the following from the Terminal:
```
sudo addgroup --system docker
sudo adduser $USER docker
newgrp docker
sudo snap disable docker
sudo snap enable docker
```
13. Reboot Ubuntu.
14. Open Visual Studio Code
15. From the start menu and select "Clone repository"
16. Clone the repo "https://github.com/CHERIoT-Platform/cheriot-rtos.git".
17. It will ask you if you want to open the repo as a project- do that.
18. It will ask you if you want to repone the project in the Container. Select Yes.
19. It takes a while to luanch but, onece it's finished, you can move on to the next section.

## Building the Code Examples
1. Ensure that you have Docker running
2. Open the `examples` folder.
3. Read `README.md` for an explanation of each of the examples.
4. We're going to try to run example 1: `01.hello_world`
5. Open the terminal at the bottom of Visual Studio Code.
6. You are probably in the root folder of the project so we want to browse to the `01.hello_world` folder: `cd examples/01.hello_world/`
7. Now execute the following:
```
xmake config --sdk=/cheriot-tools --board=sonata
xmake
xmake run
```
8. You will find the output file (firmware.uf2) in the following location: ```\\wsl.localhost\Ubuntu\home\[USERNAME]\github\cheriot-rtos\examples\01.hello_world\build\cheriot\cheriot\release```
9. If you copy that on to the SONATA drive and it will automatically reprogram the device and the code will run.

# Installing the lowRISC/sonata-software Development Environment on Windows
## Pre-requistites
Your Windows PC will need:
1. Windows Subsystem for Linux (WSL). We used Ubuntu and it works with that. Our instructions can be found [here](#installing-wsl) but it is discussed in the Sonata Software instructions too.
2. We've been using Visual Studio Code for the IDE and it works very well.
You do not need Docker for this version as it uses a system called Nix.
## Installation Procedure
1. Just follow these instructions. It will talk you through installing WSL, Nix, cloning the repo (make sure to use the V1.0 branch or later to get acces to all the hardware blocks) and building the examples. [https://lowrisc.github.io/sonata-software/doc/getting-started.html](https://lowrisc.github.io/sonata-software/doc/getting-started.html)
2. In the WSL (Ubuntu) you can browse to the folder and type ```code .``` to open teh folder in VSCode.
3. The xmake doesn't automatically try to copy the file onto the SONATA drive but that has never worked correctly on Windows anyway.
4. You should follow the tutorial to the point that you can build the Example code. If you have time, do the exercises.
5. The documentation for accessing teh Sonata's IO is a bit vague in places so we're documenting it as we go.
