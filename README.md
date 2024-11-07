# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# Setting Up Your Sonata Board
Note: The main source of information for this section is the [Updating the Sonata System](https://lowrisc.github.io/sonata-system/doc/guide/updating-system.html).
1. Browse to the [Sonata System Releases Page](https://github.com/lowRISC/sonata-system/releases).
2. Download the file with the name like `rpi_rp2_v0.X.uf2`.
3. Also download the file with the name like `sonata-vX.Y.bit.slot1.uf2`.
4. Ensure that SW3 is set to `1` to so that we run from slot 1 in a minute.
5. On the Sonata Board, hold down SW9, labelled `RP240 Boot`, and then connect to the `Main USB` port whilst still holding down the button.
6. A Flash drive will appear called `RPI RP2` (you can let go of the button now, if you haven't already).
7. Copy the `rpi_rp2_v0.X.uf2` file onto `RPI RP2`. Omnce it's finished copying it will disappear and a new drive will appear called `SONATA`
8. Now copy the `sonata-vX.Y.bit.slot1.uf2` onto the `SONATA` drive. Again, it will automatically attempt to install.
9. Watch the green LEDs on the Sonata board. Once `FPGA Config` and `Ibex Boot` are solid green, you know that it's programmed.
10. We used the file `sonata-v0.4.1.bit.slot1.uf2` and, after copying it across, the device automatically rebooted and played a simple Hello World application. The screen displays the lowRISC logo and the text, "Hello world!" and ULED0 to ULED 7 begin to display a pattern.
11. Next you will need to setup the IDE to program the device.

# Installing Development Environment on Windows
I'm not ashamed to admit that I use Visual Studio Code on Windows as my preferred IDE (I just have too many other programs on there to be constantly switching back and forth with Linux). There are some user guides for setting up the Sonata Board and the IDE but they don't all appear to be updated. I'm just documenting everything here so that I don't forget it for next time.

## Sources of help and information
1. [Getting started with the Sonata board](https://lowrisc.github.io/sonata-system/doc/guide/index.html) This is the main documentation for the Sonata board and should be kept up to date.
2. [Sonata Software - Getting started guide](https://lowrisc.github.io/sonata-software/doc/getting-started.html) This is the main documentation for the Sonata Software and should be kept up to date. This also includes a section on installing the IDE on Windows (using the Windows Subsystem for Linux).
3. [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) This is the first one that I treid and it didn't work for me. I was unable to get it to build and it assumed that I already had Docker installed (I did not at the time).

## Installing
We attempted to get the toolchain running on Windows following the [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) and found that it wasn't building and the connection to the drive wasn't working. A suggestion from [this page](https://github.com/orgs/CHERIoT-Platform/discussions/245) suggested using WSL (Windows Subsystem for Linux) for some parts of the install, so that's what we will be attempting. We're also going to attempt this on a fresh Windows 11 install to make life a bit easier.

### Installing on Windows
We need to install the Windows Subsystem for Linux.
1. From the Start menu open Windows PowerShell and run it as Administrator.
2. On a recent version of Windows it will defaul to installing Unbuntu when you run a following command. If you have any issues look at Mircosoft's detailed instructions that can be found [here](https://learn.microsoft.com/en-us/windows/wsl/install).
```
wsl --install
```
3. Wait fro the install to finish. It may take a long time.
4. After install, reboot your machine for it to finish the install.
5. Click Start and find Ubuntu, and click it to start.
6. The first time it will ask about a Unix username and password.
7. Now you can follow the Unix installation instructions using Ubuntu.

### Installing on Unix
