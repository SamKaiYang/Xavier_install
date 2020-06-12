install cudnn & Path setting
-----------------
**以下設定針對Xaiver NX欲對神經網路進行加速所需的cudnn設定,與nvcc的路徑設定(如果Xavier有遇到類似問題可以嘗試解決)**

#Install Cudnn

請至下列網站下載cudnn的tar檔
https://developer.nvidia.com/cuda-toolkit/arm

$ tar -xzvf cudnn-10.2-linux-aarch64-v7.x.x.x.tgz

$ sudo cp cuda/include/cudnn*.h /usr/local/cuda/include

$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64

$ sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*

**test**

$ cp -r /usr/src/cudnn_samples_v8/ $HOME

$ cd  $HOME/cudnn_samples_v8/mnistCUDNN

$ make clean && make

$ ./mnistCUDNN


At last,you can see

Test passed!

#Enable nvcc

$ vim ~/.bashrc

**尾末加上如下,xavier nx cuda版本為10.2**

export PATH=/usr/local/cuda-10.2/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

$ source ~/.bashrc

**test**

$ nvcc --version

you can see

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Oct_23_21:14:42_PDT_2019
Cuda compilation tools, release 10.2, V10.2.89


