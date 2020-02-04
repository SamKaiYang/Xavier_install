#-------------Prerequisites and Dependencies---------------------
$ sudo apt-get update

$ sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev

$ sudo apt-get install python3-pip

$ sudo pip3 install -U pip testresources setuptools

$ sudo pip3 install -U numpy==1.16.1 future==0.17.1 mock==3.0.5 h5py==2.9.0 keras_preprocessing==1.0.5 keras_applications==1.0.8 gast==0.2.2 enum34 futures protobuf

$ sudo pip3 install --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v42 tensorflow-gpu==1.13.1+nv19.3

#---------------Verifying The Installation-------------------
To verify that TensorFlow has been successfully installed on Jetson AGX Xavier, you’ll need to launch a Python prompt and import TensorFlow.

$ python3
>>> import tensorflow

If TensorFlow was installed correctly, this command should execute without error.
