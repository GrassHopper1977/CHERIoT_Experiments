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

