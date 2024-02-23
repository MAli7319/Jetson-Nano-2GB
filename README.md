# Jetson-Nano-2GB

## What you need?
* Jetson Nano 2GB
* Host PC with Ubuntu OS (Virtual machine is not recommended)
* Micro-USB cable to connect Jetson with your computer
* SD Card
* External monitor

## Installation
* You should download 2 files:
  * *L4T Driver Package (BSP)*: https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/t210/jetson-210_linux_r32.7.1_aarch64.tbz2 (Accessed: 22th February 2024)
  * *Sample Root Filesystem*: https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/t210/tegra_linux_sample-root-filesystem_r32.7.1_aarch64.tbz2 (Accessed: 22th February 2024)
   
* For clean setup purposes, you can move these files into a directory. Also, unzipping files (tar xf ...) may take some time. Example entire process:
  * `cd ~/Desktop`
  * `mkdir jetson_install`
  * `mv ~/Downloads/Jetson-210_Linux_R32.7.1_aarch64.tbz2 jetson_install` 
  * `mv ~/Downloads/Tegra_Linux_Sample-Root-Filesystem_R32.7.1_aarch64.tbz2 jetson_install` 
  * `tar xf Jetson-210_Linux_R32.7.1_aarch64.tbz2`
  * `cd Linux_for_Tegra/rootfs/`
  * `sudo tar xpf ../../Tegra_Linux_Sample-Root-Filesystem_R32.7.1_aarch64.tbz2`
  * `cd ..`
  * `sudo ./apply_binaries.sh`

 * At this stage, the SD card should be placed in Jetson Nano, and switch the Nano to the recovery mode by connecting FC REG and GND pins with jumper cable. After doing that, connect the Nano with your host PC by using micro-USB cable, and power the Nano.
 * When you run `lsusb` command, the devices connected to your PC are listed. If you notice something like *NVIDIA Corp. APX*, the previous step is successful.
 * Lastly, you should run this command to finalize the setup (This may take some time):
   * `sudo ./flash.sh jetson-nano-devkit mmcblk0p1`
 * If you connect the Nano with an external monitor, you should see the setup wizard on the screen. The rest is classic Ubuntu setup.

## VNC Setup
* Open the terminal of the Nano, and type:
  * `mkdir -p ~/.config/autostart`
  * `cp /usr/share/applications/vino-server.desktop ~/.config/autostart/.`
  * `gsettings set org.gnome.Vino prompt-enabled false`
  * `gsettings set org.gnome.Vino require-encryption false`
  * `gsettings set org.gnome.Vino authentication-methods "['vnc']"`
  * `gsettings set org.gnome.Vino vnc-password $(echo -n 'YOUR_PASSWORD'|base64)`
  * `sudo reboot`

* https://blog.openzeka.com/otonom-araclar/jetson-terminal-kurulumu/ (Accessed: 22th February 2024)
* https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup (Accessed: 22th February 2024)
* https://www.stereolabs.com/blog/ros-and-nvidia-jetson-nano (Accessed: 22th February 2024)

### CSI Camera
* https://github.com/JetsonHacksNano/CSI-Camera (Accessed: 22th February 2024)
* https://automaticaddison.com/how-to-set-up-a-camera-for-nvidia-jetson-nano/ (Accessed: 22th February 2024)
* https://automaticaddison.com/how-to-install-opencv-4-5-on-nvidia-jetson-nano/ (Accessed: 22th February 2024)
