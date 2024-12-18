# Setting Up Your Sonata Board
Note: The main source of information for this section is the [Updating the Sonata System](https://lowrisc.github.io/sonata-system/doc/guide/updating-system.html). [This page](https://github.com/lowRISC/sonata-system/releases/tag/v0.2) is for an earlier release of the Sonata Baord but describes the process of getting the device to display test code in detail.
1. Browse to the [Sonata System Releases Page](https://github.com/lowRISC/sonata-system/releases). Please bare in mind that, if you are planning on using the [CHERIoT RTOS](https://github.com/CHERIoT-Platform/cheriot-rtos), [CHERIoT Network](https://github.com/CHERIoT-Platform/network-stack) or [CHERIoT Demos](https://github.com/CHERIoT-Platform/cheriot-demos) then you will need to use [V0.2.0](https://github.com/lowRISC/sonata-system/releases/tag/v0.2) instead of any of the later versions.
2. Download the file with the name like `rpi_rp2_v0.X.uf2`.
3. Also download the file with the name like `sonata-vX.Y.bit.slot1.uf2`.
4. Ensure that SW3 is set to `1` to so that we run from slot 1 in a minute.
5. On the Sonata Board, hold down SW9, labelled `RP240 Boot`, and then connect to the `Main USB` port whilst still holding down the button.
6. A Flash drive will appear called `RPI RP2` (you can let go of the button now, if you haven't already).
7. Copy the `rpi_rp2_v0.X.uf2` file onto `RPI RP2`. Omnce it's finished copying it will disappear and a new drive will appear called `SONATA`
8. Now copy the `sonata-vX.Y.bit.slot1.uf2` onto the `SONATA` drive. Again, it will automatically attempt to install.
9. Watch the green LEDs on the Sonata board. Once `FPGA Config` and `Ibex Boot` are solid green, you know that it's programmed. Also the `CHERI` LED should be lit and the `Legacy` LED should be off.
10. We used the file `sonata-v0.4.1.bit.slot1.uf2` and, after copying it across, the device automatically rebooted and played a simple Hello World application. The screen displays the lowRISC logo and the text, "Hello world!" and ULED0 to ULED 7 begin to display a pattern.
11. NOTE: It is important that the firmware that you load matches the firmware version of the devlopment environment. When this was written the CHERIoT-Platform/cheriot-rtos repository was still only working with V0.2 of the bitfile (the FPGA file). Any code built for V0.2 would not work on V0.4.1 (we tried). At the time of writing the latest firmware is V1.0.0 but the CHERIoT-Platform/cheriot-rtos repo doesn't support that. You can however use the lowRISC/sonata-software repo to build for V1.0.0 and that allows access to all of the hardware blocks such as the RPi header and the Pinmux.
12. Next you will need to setup the IDE to program the device.
