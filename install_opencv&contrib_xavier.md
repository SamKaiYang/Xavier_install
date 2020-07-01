OpenCV for Xavier Installation
-----------------
1. 先卸載原有的opencv
```
sudo apt-get purge libopencv*
```
2. 安装opencv前置作業
```
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy python3-dev python3-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev  libdc1394-22-dev
```

3. Install Qt on Ubuntu
```
sudo apt-get install build-essential
sudo apt-get install qtcreator
sudo apt-get install qt5-default
sudo apt-get install qt5-doc
sudo apt-get install qt5-doc-html qtbase5-doc-html
sudo apt-get install qtbase5-examples
```
4. Download & install opencv

**然後下載[opencvx.x.x]和對應的[opencv_contribx.x.x]，都解壓。(根據自己需求要安裝哪個版本，路徑如下)**

請詳閱[opencvx.x.x](https://github.com/opencv/opencv/releases)

請詳閱[opencv_contribx.x.x](https://github.com/opencv/opencv_contrib/releases)

**在這裡，我們opencv解壓在〜/ opencvbuild / opencv-x.x.x路徑中，opencv_contrib解壓在〜/ opencvbuild / opencv_contrib-x.x.x路徑中。**

**以版本3.4.9為例**

```
cd ~/opencvbuild/opencv-3.4.9
mkdir build
cd build
```
# 上述是基本操作
```
cmake -D CMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local/ -DINSTALL_PYTHON_EXAMPLES=ON -DINSTALL_C_EXAMPLES=ON -DPYTHON_EXCUTABLE=/usr/bin/python -DOPENCV_EXTRA_MODULES_PATH=~/opencvbuild/opencv_contrib-3.4.9/modules -DWITH_CUDA=OFF -DWITH_CUFFT=OFF -DWITH_CUBLAS=OFF -DWITH_TBB=ON -DWITH_V4L=ON -DWITH_QT=ON -DWITH_GTK=ON -DWITH_OPENGL=ON -DENABLE_PRECOMPILED_HEADERS=OFF -DBUILD_EXAMPLES=ON ..

make -j8
sudo make install
```
6. 修改cv_bridge中opencv路徑

**修改 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake 文件,在 94,96,119 行修改opencv头文件和库文件的路径为/usr/local/include/opencv和/usr/local/lib/libopencv_core.so.3.4.9等。**
如下
```
sudo vim /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake
第94行
if(NOT "include;/usr/local/include;/usr/local/include/opencv2 " STREQUAL " ")
第96行
  set(_include_dirs "include;/usr/local/include;/usr/local/include/opencv2")
第119行
set(libraries "cv_bridge;/usr/local/lib/libopencv_core.so.3.4.9;/usr/local/lib/libopencv_imgproc.so.3.4.9;/usr/local/lib/libopencv_imgcodecs.so.3.4.9")
```
**測試**
```
$ python3
$ >>import cv2
$ >>cv2.__version__
```

7. 若須卸載opencv
```
cd ~/opencvbuild/opencv-3.4.9/build
sudo make uninstall
```

