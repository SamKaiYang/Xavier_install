編譯python3版本的cv_bridge
=====
**因為xavier的深度學習工具包都是以python3版本發布的，想要在python3的環境下運行重ROS節點的話，ROS安裝包自帶的cv_bridge會報語法錯，因此考慮通過源代碼編譯的方式重新生成cv_bridge，這樣才能在python3環境中運行ROS節點**
**1)  主要參考教程 https://stackoverflow.com/questions/49221565/unable-to-use-cv-bridge-with-ros-kinetic-and-python3**

詳細步驟
-----

1. apt安裝需要的軟件, xavier平台將ros-kinetic-cv-bridge換為ros-melodic-cv-bridge

$ sudo apt install python-catkin-tools python3-dev python3-catkin-pkg-modules python3-numpy python3-yaml ros-melodic-cv-bridge

2. 生成catkin工作空間文件夾並初始化

$ sudo mkdir ~/catkin_workspace

$ cd catkin_workspace

$ sudo mkdir src

$ sudo catkin init

3. 配置catkin工作空間的python環境變量，需要根據平台的實際路經修改

$ sudo catkin config -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.6m -DPYTHON_LIBRARY=/usr/lib/aarch64-linux-gnu/libpython3.6m.so

$ sudo catkin config --install

4. 下載cv_bridge的源碼, 地址為https://github.com/ros-perception/vision_opencv.git, 注意注意源代碼存在多個版本分支，ubuntu1604下載kinetic分支; xavier是ubuntu1804，下載melodic.參考教程使用git控制分支，我實際是直接選擇不同的分支下載然後解壓的.這裡列出參考教程命令

$ git clone https://github.com/ros-perception/vision_opencv.git src/vision_opencv

$ cd src/vision_opencv/

$ apt-cache show ros-melodic-cv-bridge | grep Version (kinetic得到1.12.8, melodic得到1.13.0)

$ git checkout 1.13.0 

5. 編譯並安裝

$ cd ../../

$ sudo catkin build cv_bridge 

或   

$ sudo catkin build cv_bridge --pre_clean (用於清除安裝失敗遺留的包)

若編譯成功，生成的新庫在/home/<user_name>/catkin_worksapce/install/lib/python3/dist-packages/，文件結構如下
dist-packages
            ├── cv_bridge
            │   ├── boost
            │   │   ├── cv_bridge_boost.so
            │   │   └── __init__.py
            │   ├── core.py
            │   ├── __init__.py
            │   └── __pycache__
            │       ├── core.cpython-36.pyc
            │       └── __init__.cpython-36.pyc
            └── cv_bridge-1.13.0.egg-info

6. 添加python庫路徑

$ source install/setup.bash --extend (xavier成功)


**測試**

$ python3
>>> from cv_bridge.boost.cv_bridge_boost import getCvType 
>>> 
**無錯誤提示**


            
**常見錯誤解決**

1. 工作空間配置問題
提示：The catkin CMake module was not found, but it is required to build a linked workspace.  To resolve this, please do one of the following, and try building again.
        解決：$ sudo catkin config --extend /opt/ros/melodic
        得到工作空間配置如下所示
-------------------------------------------------- ------------------------------
        Profile:                     default
        Extending:                     /opt/ros/melodic
        Workspace:                   /home/dlteam/catkin_worksapce
-------------------------------------------------- ------------------------------
        Build Space:        [exists] /home/dlteam/catkin_worksapce/build
        Devel Space:        [exists] /home/dlteam/catkin_worksapce/devel
        Install Space:      [exists] /home/dlteam/catkin_worksapce/install
        Log Space:          [exists] /home/dlteam/catkin_worksapce/logs
        Source Space:       [exists] /home/dlteam/catkin_worksapce/src
        DESTDIR:            [unused] None
-------------------------------------------------- ------------------------------
        Devel Space Layout:          linked
        Install Space Layout:        merged
-------------------------------------------------- ------------------------------
        Additional CMake Args:       -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.6m -DPYTHON_LIBRARY=/usr/lib/aarch64-linux-gnu/libpython3.6m.so
        Additional Make Args:        None
        Additional catkin Make Args: None
        Internal Make Job Server:    True
        Cache Job Environments:      False
-------------------------------------------------- ------------------------------
        Whitelisted Packages:        None
        Blacklisted Packages:        None
-------------------------------------------------- ------------------------------
     

2. libboost_python3.so和libboost_python3.a找不到
        提示：CMake Warning at /usr/share/cmake-3.10/Modules/FindBoost.cmake:1626 (message):
              No header defined for python3; skipping header check
        解決: 解決辦法有兩種

1. 先查找平台是否有編譯完成的庫
            $ sudo find /usr/lib/ -name "libboost_python3.so" 或 "libboost_python-py36.so"
                a. 如果已經存在對應的庫文件，那麼只要將路徑添加到LD_LIBRARY_PATH中即可。 xavier平台可以使用這種方法
                    $ sudo vim ~/.bashrc
                    export LD_LIBRARY_PATH=/usr/local/aarch64-linux-gnu:$LD_LIBRARY_PATH
                    $ source ~/.bashrc    
                b. 如果庫文件不存在，需要重新編譯生成庫文件。台式機使用miniconda環境使用此方法解決，解決辦法參考https://blog.csdn.net/bodybo/article/details/79962814
                    $ sudo apt-get install python-dev
                    下載boost源碼，地址為https://www.boost.org/users/history/version_1_69_0.html，解壓並進入該文件夾
$ ./bootstrap.sh --with-python=/home/dlteam/miniconda3/envs/tf_py3/bin/python3 --with-python-version=3.6 --with-python-root=/home/dlteam/miniconda3/ envs/tf_py3/lib/python3.6
                    $ ./b2 cflags='-fPIC' cxxflags='-fPIC' --with-python include="/home/dlteam/miniconda3/envs/tf_py3/include/python3.6m/"
                    $ sudo ./b2 install
                    生成的庫文件在./stage/lib/中，名字為libboost_python36.a libboost_python36.so libboost_python36.so.1.69.0
                    $ sudo cp libboost_python36* /usr/local/lib
                    $ cd /usr/local/lib
                    $ sudo ln -s libboost_python36.so libboost_python3.so
                    $ sudo ln -s libboost_python36.a libboost_python3.a
                    $ sudo vim ~/.bashrc
                    export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
                    $ source ~/.bashrc
        
        3.  import_array( ) 類型轉換錯誤
        提示：warning: converting to non-pointer type ‘int’ from NULL [-Wconversion-null]
                import_array( );
                ^~~~~~~~~~~~
        解決：$ sudo vim /home/dlteam/.local/lib/python3.6/site-packages/numpy/core/include/numpy/__multiarray_api.h
              或(miniconda) $ sudo vim /home/dlteam/miniconda3/envs/tf_py36/lib/python3.6/site-packages/numpy/core/include/numpy/__multiarray_api.h
修改"#define import_array() {if (_import_array() < 0) {PyErr_Print(); PyErr_SetString(PyExc_ImportError, "numpy.core.multiarray failed to import"); return NUMPY_IMPORT_ARRAY_RETVAL; } }" 刪除返回值，改為" #define import_array() {if (_import_array() < 0) {PyErr_Print(); PyErr_SetString(PyExc_ImportError, "numpy.core.multiarray failed to import"); } } "
