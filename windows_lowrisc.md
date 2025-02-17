# Installing the lowRISC/sonata-software Development Environment on Windows
## Pre-requistites
Your Windows PC will need:
1. Windows Subsystem for Linux (WSL). We used Ubuntu and it works with that. Our instructions can be found [here](#installing-wsl) but it is discussed in the Sonata Software instructions too.
2. We've been using Visual Studio Code for the IDE and it works very well.
3. Turn on Windows Developer mode as described here. We're not 100% sure it is needed but it enables symlinks, which may be needed.
You do not need Docker for this version as it uses a system called Nix.
## Installation Procedure
1. Just follow these instructions. It will talk you through installing WSL, Nix, cloning the repo (make sure to use the V1.0 branch or later to get acces to all the hardware blocks) and building the examples. [https://lowrisc.github.io/sonata-software/doc/getting-started.html](https://lowrisc.github.io/sonata-software/doc/getting-started.html)
2. In the WSL (Ubuntu) you can browse to the folder and type ```code .``` to open the folder in VSCode.
3. The xmake doesn't automatically try to copy the file onto the SONATA drive but that has never worked correctly on Windows anyway.
4. You should follow the tutorial to the point that you can build the Example code. If you have time, do the exercises.
5. The documentation for accessing the Sonata's IO is a bit vague in places so we're documenting it as we go.
