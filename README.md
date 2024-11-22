# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# Versions - IMPORTANT! Read this!
There is a bit of a disconnect at the minute between lowRISC (how make the FPGA image) and Sonata (the development board). It should all be fixed in V1.0.0 but, until then, you have to be really careful to get the right firmware versions for things. For example:
* lowRisc produce demos of V0.4.1 for the Sonata board but that's using the lowRisc build instructions
* Sonata describes using containers and Visual Studio Code, but those examples seem to be using V0.2
It took me a week to realise that my code wouldn't work because I was using V0.4.1 on the board and V0.2 in the compiler. 

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

## Creating a Blank Repo for Working With
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
