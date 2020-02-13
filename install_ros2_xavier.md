Installing ROS 2 via Debian Package:
-----------------

Setup Locale
===

>>Make sure you have a locale which supports UTF-8. If you are in a minimal environment, such as a docker container, the locale may be something minimal like POSIX. We test with the following settings. It should be fine if you’re using a different UTF-8 supported locale.

`sudo locale-gen en_US en_US.UTF-8`

`sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8`

`export LANG=en_US.UTF-8`

Setup Sources
===

>>You will need to add the ROS 2 apt repositories to your system. To do so, first authorize our GPG key with apt like this:

`sudo apt update && sudo apt install curl gnupg2 lsb-release`

`curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`

>>And then add the repository to your sources list:

`sudo sh -c 'echo "deb [arch=amd64,arm64] http://packages.ros.org/ros2/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'`

Install ROS 2 packages
===

>>Update your apt repository caches after setting up the repositories.

`sudo apt update`

>>Desktop Install (Recommended): ROS, RViz, demos, tutorials.

`sudo apt install ros-dashing-desktop`

>>ROS-Base Install (Bare Bones): Communication libraries, message packages, command line tools. No GUI tools.

See specific sections below for how to also install the [ros1_bridge](https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/?fbclid=IwAR1MANzhK0A3O8fnkXVW03DvhLBLJ6IENph8a5Ebwa_mGy2evg8TtM4mj8E#dashing-linux-ros1-add-pkgs), TurtleBot packages, or alternative RMW packages.

Environment setup
===

>>Sourcing the setup script
>>Set up your environment by sourcing the following file.

`source /opt/ros/dashing/setup.bash`

>>Install argcomplete (optional)
>>ROS 2 command line tools use argcomplete to autocompletion. So if you want autocompletion, installing argcomplete is necessary.

`sudo apt install python3-argcomplete`


try and test(此測試為官方測試:
===

>>在一個終端中，獲取安裝文件的源代碼，然後運行C ++ talker：

`source /opt/ros/dashing/setup.bash`

`ros2 run demo_nodes_cpp talker`

>>在另一個終端源中，安裝文件，然後運行Python listener：

`source /opt/ros/dashing/setup.bash`

`ros2 run demo_nodes_py listener`

Install additional RMW implementations(可選)
===

>>By default the RMW implementation FastRTPS is used. If using Ardent OpenSplice is also installed.

To install support for OpenSplice or RTI Connext on Bouncy:

`sudo apt update`
`sudo apt install ros-dashing-rmw-opensplice-cpp # for OpenSplice`
`sudo apt install ros-dashing-rmw-connext-cpp # for RTI Connext (requires license agreement)`

>>By setting the environment variable RMW_IMPLEMENTATION=rmw_opensplice_cpp you can switch to use OpenSplice instead. For ROS 2 releases Bouncy and newer, RMW_IMPLEMENTATION=rmw_connext_cpp can also be selected to use RTI Connext.

>>If you want to install the Connext DDS-Security plugins please refer to [this page](https://index.ros.org/doc/ros2/Installation/Install-Connext-Security-Plugins/).

>>[University, purchase or evaluation](https://index.ros.org/doc/ros2/Installation/Install-Connext-University-Eval/) options are also available for RTI Connext.

Install additional packages using ROS 1 packages
===

>>The ros1_bridge as well as the TurtleBot demos are using ROS 1 packages. To be able to install them please start by adding the ROS 1 sources as documented [here](http://wiki.ros.org/Installation/Ubuntu?distro=melodic).

>>If you’re using Docker for isolation you can start with the image ros:melodic or osrf/ros:melodic-desktop (or Kinetic if using Ardent). This will also avoid the need to setup the ROS sources as they will already be integrated.

>>Now you can install the remaining packages:

`sudo apt update`

`sudo apt install ros-dashing-ros1-bridge`

**The turtlebot2 packages are not currently available in Dashing.**

Uninstall
===

`sudo apt remove ros-dashing-* && sudo apt autoremove`




