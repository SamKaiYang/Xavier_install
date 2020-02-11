ROS2 install:
-----------------

`sudo apt update && sudo apt install curl gnupg2 lsb-release`

`curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`

`sudo sh -c 'echo "deb [arch=amd64,arm64] http://packages.ros.org/ros2/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'`

>>go to https://github.com/ros2/ros2/releases

>>download "ros2-dashing-xxxxxxxx-linux-bionic-arm64.tar.bz2"

`mkdir -p ~/ros2_crystal`

`cd ~/ros2_crystal`

`tar xf ~/Downloads/ros2-dashing-xxxxxxxx-linux-bionic-arm64.tar.bz2`

`sudo apt update`

`sudo apt install -y python-rosdep`

`sudo rosdep init` 

>>if already initialized you may continue

`rosdep update`

`CHOOSE_ROS_DISTRO=dashing`

`rosdep install --from-paths ros2-linux/share --ignore-src --rosdistro $CHOOSE_ROS_DISTRO -y --skip-keys "console_bridge fastcdr fastrtps libopensplice67 libopensplice69 osrf_testing_tools_cpp poco_vendor rmw_connext_cpp rosidl_typesupport_connext_c rosidl_typesupport_connext_cpp rti-connext-dds-5.3.1 tinyxml_vendor tinyxml2_vendor urdfdom urdfdom_headers"`

`sudo apt install -y libpython3-dev`

`. ~/ros2_crystal/ros2-linux/setup.bash`

`sudo apt install python3-argcomplete`


try and test(此測試為官方測試,若無法執行建議執行建置ros2包且執行):
-----------------

>>在一個終端中，獲取安裝文件的源代碼，然後運行C ++ talker：

`. ~/ros2_crystal/ros2-linux/setup.bash`

`ros2 run demo_nodes_cpp talker`

>>在另一個終端源中，安裝文件，然後運行Python listener：

`. ~/ros2_crystal/ros2-linux/setup.bash`

`ros2 run demo_nodes_py listener`






