OpenCV3.4.3 for Xavier Installation
-----------------
先卸載原有的opencv

$ sudo apt-get purge libopencv*

輸入下面的代碼進行安裝

$ git clone https://github.com/jetsonhacks/buildOpenCVXavier

$ cd buildOpenCVXavier

**注意將buildOpenCV.sh裡的DOWNLOAD_OPENCV_EXTRAS設置為YES，不然後面會報錯**

**此外在161行添加-D PYTHON_DEFAULT_EXECUTABLE = / usr / bin / python3 \ 來給python3安裝opencv**

$ git checkout v1.0

$ ./buildOpenCV.sh

會經過約半小時的時間
再來安裝.deb

$ ./buildAndPackageOpenCV.sh

會經過約二十分鐘後完成

**

$ python3

$ >>import cv2

$ >>cv2.__version__


參考1. https://blog.csdn.net/qq_36076110/article/details/90482617
