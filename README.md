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
  5. However this wifi mode won't persist after reboot or shutdown. So seperate script file is needed to execute switchmode after every startup. Create a startup script by following
  ```
  echo $'#! /bin/bash \necho This script automatically switchmodes to wifi mode \nsudo usb_modeswitch -KW -v 0bda -p 1a2b' > tenda_wifi_switch.sh 
  ```
  7. Give execution rights
  ```
  chmod 0700 tenda_wifi_switch.sh
  ```
  8. Create a service file and edit the file:
  ```
  sudo vi /etc/systemd/system/tenda_wifi_switch.service
  ```
  Put these into the file:
  >[Unit]  
  >Description= Switch modes of the tenda wifi to its CDROM mode to WIFI mode  
  >
  >[Service]  
  >ExecStart=/home/green-soldiers/wifi_tenda_startup.sh  
  >
  >[Install]  
  >WantedBy=multi-user.target
  
  9. Check the service runs fine by running it one time
  ```
  sudo systemctl start tenda_wifi_switch.service
  ```
  10. Check the log
  ```
  sudo journalctl -u tenda_wifi_switch.service
  ```
  11. Enable the service to run every time it boots
  ```
  sudo systemctl enable tenda_wifi_switch.service
  ```
  12. You can also disable the service by
  ```
  sudo systemctl disable tenda_wifi_switch.service
  ```
      
