# Jetson-Nano-2GB
<img src="https://github.com/MAli7319/Jetson-Nano-2GB/blob/main/j1.jpg" width="600" height="450">

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
   * <img src="https://github.com/MAli7319/Jetson-Nano-2GB/blob/main/j2.jpg" width="500" height="350">
   * <img src="https://github.com/MAli7319/Jetson-Nano-2GB/blob/main/j3.jpg" width="500" height="350">
   * <img src="https://github.com/MAli7319/Jetson-Nano-2GB/blob/main/j4.jpg" width="500" height="350">

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

## CSI Camera
* You have to connect the camera before booting the Nano
   * <img src="https://github.com/MAli7319/Jetson-Nano-2GB/blob/main/j5_b.jpg" width="500" height="350">
   * <img src="https://github.com/MAli7319/Jetson-Nano-2GB/blob/main/j6.jpg" width="500" height="350">
* When you type `ls /dev/video0` in terminal, you should see */dev/video0* as output
* Run for the test: `nvgstcapture-1.0 --orientation=2`
* We can also run python script to use the camera. This repo will help us: https://github.com/JetsonHacksNano/CSI-Camera (Accessed: 22th February 2024)
* `git clone https://github.com/JetsonHacksNano/CSI-Camera.git`
* `cd CSI-Camera`
* `gst-launch-1.0 nvarguscamerasrc sensor_id=0 ! 'video/x-raw(memory:NVMM),width=3280, height=2464, framerate=21/1, format=NV12' ! nvvidconv flip-method=2 ! 'video/x-raw, width=816, height=616' ! nvvidconv ! nvegltransform ! nveglglessink -e`
* For face detection demo, OpenCV installation needed.

## OpenCV Installation
* `git clone https://github.com/JetsonHacksNano/installSwapfile.git`
* `cd installSwapfile`
* `./installSwapfile.sh`
* reboot
* `sudo gedit /etc/fstab`
* comment *swapon* line

* https://www.ismaildurcan.com.tr/jetson-nano-bellek-swapfile-yukseltme-1/
* https://www.ismaildurcan.com.tr/jetson-nanoya-yolov5-icin-opencv-ve-pytorchun-cuda-destegiyle-kurulumu/
* 

## ROS Melodic Installation
* `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
* `sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`
* `sudo apt update`
* `sudo apt install ros-melodic-desktop`
* `echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc $ source ~/.bashrc`
* `sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential`
* `sudo rosdep init`
* `rosdep update`

* After these commands, you can reboot and run `roscore` command to see whether this process is successful or not. 


## References
* https://blog.openzeka.com/otonom-araclar/jetson-terminal-kurulumu/ (Accessed: 22th February 2024)
* https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup (Accessed: 22th February 2024)
* https://automaticaddison.com/how-to-set-up-a-camera-for-nvidia-jetson-nano/ (Accessed: 22th February 2024)
* https://www.stereolabs.com/blog/ros-and-nvidia-jetson-nano (Accessed: 22th February 2024)
