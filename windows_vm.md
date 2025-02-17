## Installing on Windows with a Virtual Machine
We used Ubuntu V24.04 for this.
1. Download & install Python (needed for VirtualBox).
2. Reboot the computer.
3. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
4. Start VirtualBox (it will probably start automatically).
5. There's detailed instructions on installing Ubuntu on VirtualBox [here](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview).
6. Log into the virtual Ubuntu (mine is generating lots of errors).
7. Open the terminal and type: `sudo apt update && sudo apt upgrade -y`
8. Now type: `sudo snap refresh` (If asked to reboot, wait until the snap update has finished installing.)
9. Open the terminal to install GIT for Linux:
```
sudo apt-get install git
```
10. There is an install menu. You want to install Visual Studio Code (the name is abbreviated to Code).
11. Install Docker too. We found the version from the package manager didn't work with VSCode. Instead from the terminal:
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```
12. There a note saying that Docker will only be availabel to root users. To change this we need to do the following from the Terminal:
```
sudo addgroup --system docker
sudo adduser $USER docker
newgrp docker
sudo snap disable docker
sudo snap enable docker
```
13. Reboot Ubuntu.
14. Open Visual Studio Code
15. From the start menu and select "Clone repository"
16. Clone the repo "https://github.com/CHERIoT-Platform/cheriot-rtos.git".
17. It will ask you if you want to open the repo as a project- do that.
18. It will ask you if you want to repone the project in the Container. Select Yes.
19. It takes a while to luanch but, onece it's finished, you can move on to the next section.
