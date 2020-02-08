**Note** 
> Starting with L4T 32.2.1 (JetPack 4.2) on the NVIDIA Jetsons
> check kernel version 4.9 

clone buildLibrealsense2Xavier file
-----------------

$ cd $HOME

$ git clone https://github.com/jetsonhacks/buildLibrealsense2Xavier

$ cd buildLibrealsense2Xavier

Build Kernel and Modules
-----------------

**The first step is to build the needed modules and a new kernel. Also, in order to have the Xavier understand the different video formats, there are some patches to apply to the module source code.**

**There are no librealsense 2 patches available to directly patch the Xavier, because it is running kernel version 4.9. This is between the different kernel versions which have patch revisions. For this reason, the patches sub-directory contains artisan patches, exquisitely crafted to upgrade the Jetson Xavier to run the librealsense 2 libraries.**

**Some of the patches actually change header files which touch some modules which are built in to the kernel. That is why the kernel Image needs to be updated. If the kernel Image is not updated, the logs get cluttered with a large number of warnings about incorrect video formats being present.**

**In order to patch and rebuild the kernel Image, modules and then install the modules:**

$ ./buildPatchedKernel.sh

Flashing the Kernel Image
-----------------

**Once the script is finished, you can now flash the new Image on to the Jetson. Use a file transfer utility such as scp or ftp to transfer the new Image file to the host PC with the JetPack installer. Sneakernet works too.**

**Now shutdown the Xavier. Connect the Jetson to the host PC via the USB cable in the same manner as the original JetPack flash. Then place the Xavier into Force Recovery Mode. This is also the same procedure used when flashing JetPack.**

**Go to the host PC. Make sure you make a backup of the original Xavier kernel Image. The Image should be in the host JetPack directory, in the "Xavier/Linux_for_Tegra/kernel" directory. With the backup secure, you can then copy the new Image that was built on the Jetson in its place.**

**Then go up one level to the "Linux_for_Tegra" directory. You are then ready to flash the Image:**

$ sudo ./flash.sh -k kernel jetson-xavier mmcblk0p1

Build librealsense 2
-----------------

$ git clone https://github.com/jetsonhacks/installRealSenseSDK.git

$ cd installRealSenseSDK

$ ./installLibrealsense.sh

install finish,test realsense driver
-----------------

$ ./realsense-viewer

You may remove all the kernel source using the provided convenience script 'removeAllKernelSources.sh'.
-----------------

$ cd buildLibrealsense2Xavier

$ ./removeAllKernelSources.sh

realsense-ros using
-----------------

$ mkdir -p ~/catkin_ws/src

$ cd ~/catkin_ws/src

$ catkin_init_workspace .

$ git clone https://github.com/IntelRealSense/realsense-ros

$ cd realsense-ros

$ git checkout -b 2.2.7 2.2.7

$ sudo apt install ros-melodic-ddynamic-reconfigure

$ cd ~/catkin_ws

$ catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release

Intel Realsense D435i testing
-----------------

$ roslaunch realsense2_camera demo_pointcloud.launch




