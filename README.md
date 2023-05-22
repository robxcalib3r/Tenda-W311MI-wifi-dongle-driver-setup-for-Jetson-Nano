# Tenda W311MI wifi dongle driver setup for Jetson Nano 4GB
Tenda W311MI [RTL8818GU] [0bda:b711(after ModeSwitch)] [0bda:0411 (before ModeSwitch)] successfully installed on Jetson Nano 4GB

## Instructions
1. You should have an internet connection to run the following commands. So connect the Jetson nano with ethernet cable.
2. Open up the terminal & run the commands
```
sudo apt-get install build-essential
git clone https://github.com/McMCCRU/rtl8188gu.git
cd rtl8188gu
make ARCH=arm64
sudo make install
sudo apt install --reinstall linux-firmware 
sudo reboot
```

3. After rebooting try giving the following commands in the terminal:
```
sudo usb_modeswitch -KW -v 0bda -p 1a2b 
```
This command switches the dongle to it's default CDROM mode (0bda:0411) to WIFI mode (0bda:b711)
to check type ```lsusb``` in the terminal.

4. Voila! You should find the wifi on the networking section (upper corner)
5. However this wifi mode won't persist after reboot or shutdown. So seperate script file is needed to execute switchmode after every startup. Create a separate script file using
``` touch tenda_wifi_startup_switchmode.sh ``` 
6. Edit this file of your favourite editor (ex: ``` gedit tenda_wifi_startup_switchmode.sh ```) and write these 
```
#! /bin/bash
echo This script automatically switchmodes to wifi mode
sudo usb_modeswitch -KW -v 0bda -p 1a2b 
```
7. Give execution rights
```
chmod u+x tenda_wifi_startup_switchmode.sh
```
8. Add this in ```/etc/rc.local``` to execute the script at startup
```
/path/to/your/script/tenda_wifi_startup_switchmode.sh || exit 1   # Starts the switchmode script
exit 0
```
9. Make the rc.local file executable 
```
sudo chmod +x /etc/rc.local
```
