# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# Versions - IMPORTANT! Read this!
There is a bit of a disconnect at the minute between lowRISC (how make the FPGA image) and Sonata (the development board). It should all be fixed in V1.0.0 but, until then, you have to be really careful to get the right firmware versions for things. For example:
* lowRisc produce demos of V0.4.1 for the Sonata board but that's using the lowRisc build instructions
* Sonata describes using containers and Visual Studio Code, but those examples seem to be using V0.2
It took me a week to realise that my code wouldn't work because I was using V0.4.1 on the board and V0.2 in the compiler. This page was very helpful in working it all out: https://cheriot.org/rtos/sonata/git/2024/11/08/navigating-the-ecosystem.html

# Setting Up Your Sonata Board
[Setting Up Your Sonata Board](setup.md)

# Installing Development Environment on Windows
This is how to setup Visual Studio code on Windows for development.
* [Using WSL](windows_setup.md#installing-development-environment-on-windows)
* [Using a Virtual Machine](windows_setup.md#installing-on-windows-with-a-virtual-machine)
* [Building the Code Examples on Windows](windows_setup.md#building-the-code-examples)


## Terminal Access
After connecting the Sonata, you may notice that 3 COM ports have appeared (On Windows, I have a look in Device Manager to get the names). For me they are COM8, COM9 and COM10. I used PuTTY to connect to them at 921600 buad, using the `Serial` connection type. I ticked 'Implicit CR in every LF' under Terminal settings (it eliminates any ambiguity caused by the whole [Windows/Linux line endings debate](https://en.wikipedia.org/wiki/Newline#History)).
The console output appears to be on COM9.

## Creating a Repo for Working With
I've not tried this yet but I've been given this note from Adam:
```
In terms of a repo for an empty project, it's fairly easy to make one. Add cheriot-rtos as a submodule and point xmake.lua to it (lines 9-10 in the repo I'm going to link). I need to bump RTOS to the latest version in this repo, just run "git submodule update --remote" if you try this out.
https://github.com/3bian/3bian-sonata-empty-project
```
I've been given this note from David Chisnall:
```
For things that don't love in the same repo as the RTOS, you can reuse the dev container (you don't need to fork the RTOS, just reference the same dev container).

But for device drivers, I like it when people upstream things, and so having that in the RTOS is nice.
```
This note from Phil:
```
I work in a fork of cheriot-demos, which comes with  dev container, submodules for rtos and the network stack, and a few examples of creating things that work with Sonata
```

# Using the Sonata hardware (before V1)
## The Joystick, LEDs and RGB LED
I have created an example [here](https://github.com/GrassHopper1977/cheriot-rtos-sonata-hardware/tree/main/experiments/01.general_io) that demostrates reading from the build-in hardware (in this case, the joystick controller) and writing to the built-in LEDs and RGB LED.
Note: These were written for V0.2. If you want to use the PinMux (pin multiplexor - used to switch to alternate pin functions) then we will need to use a more recent version of the FPGA design (the bitfile).
## Accessing the Raspberry Pi GPIO header
1. There is no access to this on V0.2 so we will need to move to teh pre-release of V1. The corresponding code for this can be found in lowRISC's fork of Cheriot-RTOS. We can start by cloning this repo and then switching to the Sonata branch: [https://github.com/CHERIoT-Platform/cheriot-rtos.git](https://github.com/lowRISC/cheriot-rtos.git)
Then we can check it out recursively from WSL and then switch to the "Sonata" branch:
```
git clone --recursive https://github.com/lowRISC/cheriot-rtos.git
cd cheriot-rtos
code .
```
2. As VSCode is opening the repo select the "reopne in container" option that you will be offered. Note: If you've been playing with anotehr fork of "cheriot-rtos" without renaming it and you left the container there you may get errors. Simply delete the offending container and it will create a new one and you're good to go.
3. Now we need to target for teh pre-release of V1. First let's check the board files to make sure that we have access to the Pinmux and RPi header. A quick check of sdk/boards/sonata.json and sdk/boards/sonata-prerelease.json show that teh RPi header is not listed in the IO map. We will need to look on a different branch.
4. Switch to the "sonata" branch. Looking in sdk/boards/sonata-prerelease.json we can now see the RPi, arduino and pmod header entries:
```
        "gpio_board" : {
            "start" : 0x80000000,
            "end"   : 0x80000010
        },
        "gpio_rpi" : {
            "start" : 0x80000040,
            "end"   : 0x80000050
        },
        "gpio_arduino" : {
            "start" : 0x80000080,
            "end"   : 0x80000090
        },
        "gpio_pmod0" : {
            "start" : 0x800000C0,
            "end"   : 0x800000D0
        },
        "gpio_pmod1": {
            "start" : 0x80000100,
            "end"   : 0x80000110
        },
        "gpio_pmodc": {
            "start" : 0x80000140,
            "end"   : 0x80000150
        },
```
5. We can also see access to the Pinmux:
```
        "pinmux": {
            "start" : 0x80005000,
            "length": 0x00001000
        },
```
6. We're going to want to use this file.
7. We will need to change our code to build using this board description file to access the GPIO. We will alos need to target the pre-release of V1.0 of the bitfile.
8. When we build we will use these instructions (notice how we've changed the board) whihc should allow us to build for the V1 prerelase bitfile:
```
xmake config --sdk=/cheriot-tools --board=sonata-prerelease
xmake
xmake run
```
9. 
