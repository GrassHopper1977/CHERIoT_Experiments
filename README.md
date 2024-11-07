# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# Setting Up Your Sonata Board
Note: The main source of information for this section is the [Updating the Sonata System](https://lowrisc.github.io/sonata-system/doc/guide/updating-system.html). [This page](https://github.com/lowRISC/sonata-system/releases/tag/v0.2) is for an earlier release of the Sonata Baord but describes the process of getting the device to display test code in detail.
1. Browse to the [Sonata System Releases Page](https://github.com/lowRISC/sonata-system/releases).
2. Download the file with the name like `rpi_rp2_v0.X.uf2`.
3. Also download the file with the name like `sonata-vX.Y.bit.slot1.uf2`.
4. Ensure that SW3 is set to `1` to so that we run from slot 1 in a minute.
5. On the Sonata Board, hold down SW9, labelled `RP240 Boot`, and then connect to the `Main USB` port whilst still holding down the button.
6. A Flash drive will appear called `RPI RP2` (you can let go of the button now, if you haven't already).
7. Copy the `rpi_rp2_v0.X.uf2` file onto `RPI RP2`. Omnce it's finished copying it will disappear and a new drive will appear called `SONATA`
8. Now copy the `sonata-vX.Y.bit.slot1.uf2` onto the `SONATA` drive. Again, it will automatically attempt to install.
9. Watch the green LEDs on the Sonata board. Once `FPGA Config` and `Ibex Boot` are solid green, you know that it's programmed. Also the `CHERI` LED should be lit and the `Legacy` LED should be off.
10. We used the file `sonata-v0.4.1.bit.slot1.uf2` and, after copying it across, the device automatically rebooted and played a simple Hello World application. The screen displays the lowRISC logo and the text, "Hello world!" and ULED0 to ULED 7 begin to display a pattern.
11. Next you will need to setup the IDE to program the device.

# Installing Development Environment on Windows
I'm not ashamed to admit that I use Visual Studio Code on Windows as my preferred IDE (I just have too many other programs on there to be constantly switching back and forth with Linux). There are some user guides for setting up the Sonata Board and the IDE but they don't all appear to be updated. I'm just documenting everything here so that I don't forget it for next time.

## Sources of help and information
1. [Getting started with the Sonata board](https://lowrisc.github.io/sonata-system/doc/guide/index.html) This is the main documentation for the Sonata board and should be kept up to date.
2. [Sonata Software - Getting started guide](https://lowrisc.github.io/sonata-software/doc/getting-started.html) This is the main documentation for the Sonata Software and should be kept up to date. This also includes a section on installing the IDE on Windows (using the Windows Subsystem for Linux).
3. [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) This is the first one that I treid and it didn't work for me. I was unable to get it to build and it assumed that I already had Docker installed (I did not at the time).

## Installing on Windows
We attempted to get the toolchain running on Windows following the [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) and found that it wasn't building and the connection to the drive wasn't working. A suggestion from [this page](https://github.com/orgs/CHERIoT-Platform/discussions/245) suggested using WSL (Windows Subsystem for Linux) for some parts of the install, so that's what we will be attempting. We're also going to attempt this on a fresh Windows 11 install to make life a bit easier.

### Installing WSL
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

### Other Packages to have Installed
1. Got to [Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/) and follow the instructions.
2. Install Microsoft Visual Studio Code if you don't already have it.
3. Turn on Windows Developer mode as described [here](https://learn.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development). We're not 100% sure it is needed but it enables symlinks, which may be needed.

### Setting Up the Toolchain
We're attempting to follow teh instructions from [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html).
1. Open Visual Studio Code.
2. Select 'Clone Git Repository' and clone the `cheriot-rtos` repository (https://github.com/CHERIoT-Platform/cheriot-rtos.git).
3. Select a destination for the code and save. Visual Studio Code will ask you if you want to `Reopen in container` so select OK. Note: If Docker isn't running, it will return an error.
4. Contecting to the container can take a while the first time that you do it.
5. You may get a warning about unsafe repositories, you can click on it and choose to to allow all of them (I get two).
6. Now we need to edit the `.devcontainer\devcontainer.json` file (look in the solution explorer on teh left of the screen).
7. We want to add a mount section to the end of the file to allow us to automatically deploy any projects that we build.
8. The code that we are adding should look something like this:
```
"mounts": [
    "source=/Volumes/SONATA,target=/mnt/SONATA,type=bind"
]
```
8. `source` should remain as `/Volumes/SONATA`
9. `target` should be altered to point to the SONATA device. In my case it is mounted as drive D. If we place `D:` here it fails. This is what we intend to use WSL to try to solve.
10. When we save this file it will attempt to rebuild the container. We may have to use the option `Edit devcontainer.json Locally` option to get it to work. If you do that it will open the code locally and not in the container so changes won't automatically get rebuilt. I've beeb closing and reopening Visual Studio Code to get around this but I'm sure there's a shortcut for it - I just haven't had a chance to look yet. You can remove the `mount` section entirely until you are ready to have a go at fixing it. You'll just have manually copy the files across to the SONATA drive.

### Building the Code Examples
1. Open the `examples` folder.
2. Read `README.md` for an explanation of each of the examples.
3. We're going to try to run example 1: `01.hello_world`
4. Open the terminal at the bottom of Visual Studio Code.
5. You are probably in the root folder of the project so we want to browse to the `01.hello_world` folder: `cd examples/01.hello_world/`
6. Now execute the following:
```
xmake config --sdk=/cheriot-tools --board=sonata
xmake
xmake run
```
7. This should build the code for us. The `sdk` option should already be correct for the compartment that we are running in. If it fails for any reason, it is possible that we are no longer running in the comaprtment, this will happen if the mount point failed (you can remove the mount point from the devcontainer.json file to allow us to build. We can always add it back in once we've got it working on Windows.)
8. It fails on teh second step (`xmake`) with teh following error: 
```
error: /workspaces/cheriot-rtos/sdk/include/cheri.hh:12:10: fatal error: 'magic_enum/magic_enum.hpp' file not found
#include <magic_enum/magic_enum.hpp>
```
9. I guess I've still got soemthing wrong somewhere.

## Terminal Access
After connecting the Sonata, you may notice that 3 COM ports have appeared (have a look in Device Manager to get the name. For ne they are COM8, COM9 and COM10. I used PuTTY to connect to them at 115200 buad, using the `Serial` connection type. I ticked 'Implicit CR in every LF' under Terminal settings, as this is often needed.
There doesn't appear to be an output on any of these terminals so I may have the buad rate incorrect.
