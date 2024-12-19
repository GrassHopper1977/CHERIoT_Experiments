# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# Versions - IMPORTANT! Read this!
There is a bit of a disconnect at the minute between lowRISC (how make the FPGA image) and Sonata (the development board). It should all be fixed in V1.0.0 but, until then, you have to be really careful to get the right firmware versions for things. For example:
* lowRisc produce demos of V0.4.1 and V1.0.0 for the Sonata board but that's using the lowRisc build instructions
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
After connecting the Sonata, you may notice that 3 COM ports have appeared (On Windows, I have a look in Device Manager to get the names). For me they are COM8, COM9 and COM10. 
The buad rate depnds on which repositiry you use. If you are using [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software) then it will be 921600bps. If you are using [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos) then it will be 115200bps. I used PuTTY to connect to them at the required bit rate, using the `Serial` connection type. I ticked 'Implicit CR in every LF' under Terminal settings (it eliminates any ambiguity caused by the whole [Windows/Linux line endings debate](https://en.wikipedia.org/wiki/Newline#History)).
The console output appears to be on COM9 for us.

## Creating a Repo for Working With
I've not tried this yet but I've been given this note from Adam:
```
In terms of a repo for an empty project, it's fairly easy to make one. Add cheriot-rtos as a submodule and point xmake.lua to it (lines 9-10 in the repo I'm going to link). I need to bump RTOS to the latest version in this repo, just run "git submodule update --remote" if you try this out.
https://github.com/3bian/3bian-sonata-empty-project
```
I've been given this note from David Chisnall:
```
For things that don't live in the same repo as the RTOS, you can reuse the dev container (you don't need to fork the RTOS, just reference the same dev container).

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

# Using the Sonata hardware (V1 and later)
## Accessing the Raspberry Pi GPIO header
1. There is no access to this on V0.2 so we will need to move to the pre-release of V1. For this to work we had to switch to a different repo using [these instructions](https://github.com/GrassHopper1977/CHERIoT_Experiments/blob/main/windows_setup.md#installing-the-lowriscsonata-software-development-environment-on-windows). 
2. Switch to the "V1.0" branch. Looking in sdk/boards/sonata-prerelease.json we can now see the RPi, arduino and pmod header entries:
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
3. We can also see access to the Pinmux:
```
        "pinmux": {
            "start" : 0x80005000,
            "length": 0x00001000
        },
```
4. We're going to want to use this file.
5. We create a new folder and copy an existing xmake.lua into it which we can then modfiy to suit our purpose.
6. In our example, we're going to execute `xmake` from within the source folder.
7. Our program entry point is called `main_entry()`, our first compartment is called `main_comp` (we couldn't think of anything else for this test) and our file is called `general_io.cc`.
8. We've altered our `xmake.lua` file to read as follows (we left the copyright in as we are "standing on the shoulders of giants"):
```
-- Copyright lowRISC Contributors.
-- SPDX-License-Identifier: Apache-2.0

set_project("Sonata General IO examples")
sdkdir = "../../cheriot-rtos/sdk"
includes(sdkdir)
set_toolchains("cheriot-clang")

includes(path.join(sdkdir, "lib"))
--includes("../../libraries")
includes("../../common.lua")

option("board")
    set_default("sonata-prerelease")

compartment("main_comp")
    add_deps("debug")
    add_files("general_io.cc")

firmware("general_io")
    add_deps("freestanding", "main_comp")
    on_load(function(target)
        target:values_set("board", "$(board)")
        target:values_set("threads", {
            {
                compartment = "main_comp",
                priority = 2,
                entry_point = "main_entry",
                stack_size = 0x200,
                trusted_stack_frames = 1
            },
        }, {expand = false})
    end)
    after_link(convert_to_uf2)
```
9. Let's break down the meaning of each section:
Theese lines gives it a title, includes the SDK directory in the cheriot-rtos sub module and sets up the `clang` toolchain.
```
set_project("Sonata General IO examples")
sdkdir = "../../cheriot-rtos/sdk"
includes(sdkdir)
set_toolchains("cheriot-clang")
```
The following lines include the library functions and the `common.lua` script which gives us access to the `convert_to_uf` which is called at the end.
```
includes(path.join(sdkdir, "lib"))
--includes("../../libraries")
includes("../../common.lua")
```
The following sets us to the pre-release board description file, giving us access to all the external IO (RPi header, Pinmux, etc).
```
option("board")
    set_default("sonata-prerelease")
```
The following says, "The compartment `main_comp` is dependant on `debug` and includes the file `general_io.cc`":
```
compartment("main_comp")
    add_deps("debug")
    add_files("general_io.cc")
```
These lines say, "Create file called `generl_io` depndant on the `main_comp` compartment and `freestanding` (I don't know what that is yet but it appears to be needed). Compile using the board description that we gave earlier. There is one thread based in the 'main_comp' compartment and it's entry point is `main_entry`." It also sets up the thread priority, stack size and `trusted_stack_frames` (not sure what that is yet). The last line calls the `convert_to_uf2` function in `common.lua` to build 3 versions of the UF2 file - one for each of the three memory slots.
```
firmware("general_io")
    add_deps("freestanding", "main_comp")
    on_load(function(target)
        target:values_set("board", "$(board)")
        target:values_set("threads", {
            {
                compartment = "main_comp",
                priority = 2,
                entry_point = "main_entry",
                stack_size = 0x200,
                trusted_stack_frames = 1
            },
        }, {expand = false})
    end)
    after_link(convert_to_uf2)
```
## Example of Accessing RPi IO and using the PWM functions
We've created a simple example of using these [here](https://github.com/GrassHopper1977/sonata-software/tree/IO_Experiments/experiments/01.general_io).
Note: On V1.0, only PWM0 works. This is due to be fixed in V1.1. There is some test code for testing all the PWM outputs [here](https://github.com/GrassHopper1977/sonata-software/tree/IO_Experiments/experiments/02.pwm_outputs). The code also demostrates using the Arduino, PMOD and mikroBUS connectors for PWM outputs.
