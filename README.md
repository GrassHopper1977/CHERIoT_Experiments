# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# General Notes
1. There is no floating point library at the minute. You need to work around this.

# Versions - IMPORTANT! Read this!
You must ensure that you use the correct version of the bitfile (the FPGA image) for the build environment that you are using. If you load Sonata with V1.0 and build for V0.2 it will not work.

Different repositories target different versions of the bitfile.

## Which Repository Should we Use?
This page was very helpful in working it all out: https://cheriot.org/rtos/sonata/git/2024/11/08/navigating-the-ecosystem.html

## Sources of help and information
1. [Getting started with the Sonata board](https://lowrisc.github.io/sonata-system/doc/guide/index.html) This is the main documentation for the Sonata board and should be kept up to date.
2. [Sonata Software - Getting started guide](https://lowrisc.github.io/sonata-software/doc/getting-started.html) This is the main documentation for the Sonata Software and should be kept up to date. This also includes a section on installing the IDE on Windows (using the Windows Subsystem for Linux).
3. [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) This is the first one that I tried and it didn't work for me on Windows (it worked on an Ubuntu Virtual Machine though and via the [WSL](#installing-wsl)). I was unable to get it to build and it assumed that we had some other packages installed already. Do read this one carfully though, you'll notice that it uses the V0.2 firmware. If you try to use V0.4.1 then the files that you build will not work! All that is wrong with this for Windows is that you need WSL and you can't use VSCode to clone the repo - you have to use Git from WSL (see below for the [full instructions](#building-the-code-examples)).
4. [Navigating the CHERIoT Ecosystem](https://cheriot.org/rtos/sonata/git/2024/11/08/navigating-the-ecosystem.html) This is a useful document that discusses the different repositories available and the pros and cons of each. It *should* (like this document) become largely obolete with the release of CHERIO-RTOS V1.0.

# Setting Up Your Sonata Board
This covers loading your Sonata board with a bitfile: [Setting Up Your Sonata Board](setup.md)

# Installing Development Environment on Windows
This can vary slightly between repositories.

## Using [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos)
There's 2 options here:
### 1. Run it Directly From Windows
This requires integrates with Visual Studio Code. You will need WSL & Docker the set up is described in detail here:
* [Using WSL](windows_setup.md)

Note: Docker desktop gets itself confused sometimes:
1. If it won't open you may need to update WSL from the command line with `wsl --update` (your machine will reboot without warning!)
2. After an update in may fail to launch saying it cannot find WSL so try this from the command line: `wsl --unregister docker-desktop`. When you restart Docker it should recreate the connection (a reboot may be required).
### 2. Run it from a Virtual Machine
This method requires running a full virtual machine  but it works pretty well. It's is described in detail here:
* [Using a Virtual Machine](windows_vm.md)
### Building the Code Examples
Once you have an installed [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos) development platform you can build the code sampels as described here:
* [Building the Code Examples on Windows](cheriot-rtos_build.md)

## Using [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software)
This is slightly different. It doesn't require Docker but still uses the Windows Subsystem for Linux running *nix. Detailed instructions can be found here:
* [Setting up Sonata-Software Branch on Windows](windows_lowrisc.md)


## Terminal Access
After connecting the Sonata, you may notice that 3 COM ports have appeared (On Windows, I have a look in Device Manager to get the names). For me they are COM8, COM9 and COM10. COM9 was the active debug port.

The buad rate depends on which repositiry and which version you use:
| Repository | Baud Rate |
| ---------- | --------- |
| [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos) >= V1.0 | 921600bps |
| [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos) < V1.0 | 115200bps |
| [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software) | 921600bps |


## Creating a Repo for Working With

These were some suggestions that we were given:
1. This note from Adam:
```
In terms of a repo for an empty project, it's fairly easy to make one. Add cheriot-rtos as a submodule and point xmake.lua to it (lines 9-10 in the repo I'm going to link). I need to bump RTOS to the latest version in this repo, just run "git submodule update --remote" if you try this out.
https://github.com/3bian/3bian-sonata-empty-project
```
2. This note from David Chisnall:
```
For things that don't live in the same repo as the RTOS, you can reuse the dev container (you don't need to fork the RTOS, just reference the same dev container).

But for device drivers, I like it when people upstream things, and so having that in the RTOS is nice.
```
3. This note from Phil:
```
I work in a fork of cheriot-demos, which comes with  dev container, submodules for rtos and the network stack, and a few examples of creating things that work with Sonata
```
4. I have worked in forks of both [lowRISC/Sonata-Software](https://github.com/lowRISC/sonata-software) and [CHERIoT-Platform/CHERIoT-RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos). It makes it easier to PR any changes back.

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
10. We will need to use the `pinmux` to ensure that the RPi header is connected to the GPIO internally, it is not the default on every pin. 
## Example of Accessing RPi IO and using the PWM functions
We've created a simple example of using these [here](https://github.com/GrassHopper1977/sonata-software/tree/IO_Experiments/experiments/01.general_io).
Note: On V1.0, only PWM0 works. This is due to be fixed in V1.1. There is some test code for testing all the PWM outputs [here](https://github.com/GrassHopper1977/sonata-software/tree/IO_Experiments/experiments/02.pwm_outputs). The code also demostrates using the Arduino, PMOD and mikroBUS connectors for PWM outputs.
