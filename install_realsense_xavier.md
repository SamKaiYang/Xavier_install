**Note** 
> test on (JetPack 4.4) NVIDIA Jetsons
> check kernel version 4.9 

## Building from Source using Native Backend
 
 **Use the V4L Native backend by applying the kernel patching**
 
   The method was verified with **Jetson AGX** boards with JetPack **4.2.3**[L4T 32.2.1,32.2.3], **4.3**[L4T 32.3.1] and **4.4**[L4T 32.4.3].
   
   The medhoe has not yet been verified on the **Jetson Nano** board.
  
  * **Prerequisite**
  
    * Verify the board type and Jetpack versions compatibility.  
    * Verify internet connection.  
    * Verify the available space on flash, the patching process requires **~2.5Gb** free space  
       >df -h
        
    * Configure the Jetson Board into Max power mode (desktop -> see the upper right corner)  
    * Disconnect attached USB/UVC cameras (if any).  
     
  * **Build and Patch Kernel Modules for Jetson L4T** 
  
    1. Navigate to the root of libreansense2 directory.  
    2. Run the script (note the ending characters - `L4T`)  
    ```
    ./scripts/patch-realsense-ubuntu-L4T.sh  
    ```
    * The script will run for about 30 minutes depending on internet speed and perform the following tasks:  
    
      a. Fetch the kernel source trees required to build the kernel and its modules.  
      b. Apply Librealsense-specific kernel patches and build the modified kernel modules.  
      c. Try to insert the modules into the kernel.  

  * **Build librealsense2 SDK**  
  
    1. Navigate to the SDK's root directory.  
    ```
    sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev -y
    ./scripts/setup_udev_rules.sh  
    mkdir build && cd build  
    cmake .. -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=release -DFORCE_RSUSB_BACKEND=false -DBUILD_WITH_CUDA=true && make -j$(($(nproc)-1)) && sudo make install
    ```
       The CMAKE `-DBUILD_WITH_CUDA=true` flag assumes CUDA modules are installed. If not, please reconnect the board to the Ubuntu Host PC and use NVIDIA `SDK Manager` tool to install the missing components.

  * **Connect Realsense Device, run `realsense-viewer` and inspect the results**
 

Reference:https://github.com/IntelRealSense/librealsense/blob/master/doc/installation_jetson.md


## install RealSense ROS
  * **clone**
    ```
    git clone https://github.com/jetsonhacks/installRealSenseROS.git
    ```
  * **fix content**
    ```
    cd installRealSenseROS
    gedit installRealSenseROS.sh
    #註解下面兩行
    line#13 REALSENSE_ROS_VERSION=2.2.11
    line#83 git checkout $REALSENSE_ROS_VERSION
    ```
  * **To install:**
    ```
    ./installRealSenseROS.sh <catkin_ws_name>
    ```
    The script 'disableAutosuspend.sh' simply turns off the USB autosuspend setting on the Jetson so that the camera is always available.


Reference:https://github.com/jetsonhacks/installRealSenseROS.git



