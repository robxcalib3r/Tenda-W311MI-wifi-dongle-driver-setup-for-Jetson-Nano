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

4. Voila! You should find the wifi on the networking section (upper corner)
