# CHERIoT_Experiments
Our experiments with the Secure CHERIoT Ibex core on the Sonata Board from lowRISC. This is not intended to be any kind of replacement for the official documentation, it's more of a historical document for us during our experiments with the Sonata board but it may be of use to some.

# Installing Development Environment on Windows
I'm not ashamed to admit that I use Visual Studio Code on Winodws as my preferred IDE (I just have too many otehr programs on there to be constantly switching back and forth with Linux). There are some user guides for setting up the Sonata Board and the IDE but they don't all appear to be updated. I will document the entire process here with date and version information so that you can just for yourself.

## Sources of help and information
1. [Getting started with the Sonata board](https://lowrisc.github.io/sonata-system/doc/guide/index.html) This is the main documentation for the Sonata board and should be kept up to date.
2. [Sonata Software - Getting started guide](https://lowrisc.github.io/sonata-software/doc/getting-started.html) This is teh main documentation for the Sonata Software and should be kept up to date. This also includes a section on installing the IDE on Windows (using teh Windows Subsystem for Linux).
3. [From zero to CHERIoT in two minutes with Sonata](https://cheriot.org/fpga/ibex/2024/06/10/sonata-quick-start.html) This is teh first one that I treid and it didn't work for me. I was unable to get it to build and it assumed that I already had Docker installed (I did not at the time).

## Installing
We attempted to get the toolchain runnign on Windows and had no success so we will be installing this using WSL (Windows Subsystem for Linux) instead.

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
