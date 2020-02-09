OpenCV3.4.3 for Xavier Installation
-----------------
先卸載原有的opencv

$ sudo apt-get purge libopencv*

輸入下面的代碼進行安裝

$ git clone https://github.com/jetsonhacks/buildOpenCVXavier

$ cd buildOpenCVXavier

$ git checkout v1.0

$ ./buildOpenCV.sh

會經過約半小時的時間
再來進行測試

$ python3

$ >>import cv2

$ >>cv2.__version__


參考1. https://blog.csdn.net/qq_36076110/article/details/90482617
