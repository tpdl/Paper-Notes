# How to set up GPU-supported Environment for Caffe on Ubuntu 16.04 :

### Updated : 2017,3,28 by Po-Hsuan Huang

## 1. Install Nvida Driver

* The GPU support prerequisites https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide#the-gpu-support-prerequisites.

* In Ubuntu desktop, enable the use of proprietary drivers in the Software & Updates Center for your desktop and install the NVIDIA graphics driver from the main Ubuntu package repository. See https://help.ubuntu.com/community/BinaryDriverHowto/Nvidia

 
* *Discover which driver number you need with:*
```
   $sudo ubuntu-drivers devices
```
* Installation of Nvida driver can be done manually, but we will intall it when installing Cuda.
   


## 2. Install Cuda

* The LATEST version of Cuda Toolkit 8.0 is available from the NVIDIA website. Download the Cuda Toolkit 8.0 network installer and the CUDNN 5.1 package from the NVIDIA site, after registering and filling out the forms. https://developer.nvidia.com/cuda-downloads

* Install the Cuda Toolkit 8.0 package manually in the terminal as instructed at the website. For example...
```
   $sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
   $sudo apt-get update
   $sudo apt-get install cuda

```

* Install CUDA for Ubuntu
 http://askubuntu.com/questions/799184/how-can-i-install-cuda-on-ubuntu-16-04
 There is an Linux installation guide. However, it is basically only those steps:
    
    Download CUDA: I used the 15.04 version and "runfile (local)". That is 1.1 GB.
    ```
    Check the md5 sum: $ md5sum cuda_7.5.18_linux.run.
    ```
    Only continue if it is correct.
    
    Remove any other installation 
    ```
    $sudo apt-get purge nvidia-cuda*
    ```
    -if you want to install the drivers too, then 
    ```
    $sudo apt-get purge nvidia-*
    ```
    If you want to install the display drivers(*), logout from your GUI. Go to a terminal session and hold+press (ctrl+alt+F2)
    Stop lightdm: 
    ```
    $sudo service lightdm stop
    ```
    ```
    $sudo sh cuda_7.5.18_linux.run --override.
    ```
    Make sure that you say y for the symbolic link.
    Start lightdm again: 
    ```
    $sudo service lightdm start
    ```
    Follow the command-line prompts

    >  $nvidia-smi
    
    If there is no error, you should see a table showing the current devices.

## 3. Install cuDNN

* Step 0: Install cuda from the standard repositories. (See [How can I install CUDA on Ubuntu 16.04?](http://askubuntu.com/q/799184/10425))

* Step 1: Register an nvidia developer account and [download cudnn here](https://developer.nvidia.com/cudnn) (about 80 MB)

* Step 2: Check where your cuda installation is. For the installation from the repository it is `/usr/lib/...` and `/usr/include`. Otherwise, it will be `/urs/local/cuda/`. You can check it with `which nvcc` or `ldconfig -p | grep cuda`

* Step 3: Copy the files:
```
    $ cd folder/extracted/contents
    $ sudo cp -P include/cudnn.h /usr/include
    $ sudo cp -P lib64/libcudnn* /usr/lib/x86_64-linux-gnu/
    $ sudo chmod a+r /usr/lib/x86_64-linux-gnu/libcudnn*
```


## 4. install opencv3

* For the instructions on how to use the OpenCV version 3.1, please see https://github.com/BVLC/caffe/wiki/OpenCV-3.1-Installation-Guide-on-Ubuntu-16.04

```
   sudo apt-get install --assume-yes build-essential cmake git
   sudo apt-get install --assume-yes build-essential pkg-config unzip ffmpeg qtbase5-dev python-dev python3-dev python-numpy python3-numpy
   sudo apt-get install --assume-yes libopencv-dev libgtk-3-dev libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev    sudo apt-get install --assume-yes libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
   sudo apt-get install --assume-yes libv4l-dev libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev
   sudo apt-get install --assume-yes libvorbis-dev libxvidcore-dev v4l-utils
```
* In order to install the NVIDIA Cuda Toolkit with CUDNN library
  The following guide includes the how-to instructions for the installation of BVLC/Caffe on Ubuntu 16.04 with Cuda Toolkit 8.0, CUDNN 5.1 library and OpenCV version 3. 
 Execute these commands first:
 
   ```  
   sudo apt-get update
   sudo apt-get upgrade
   sudo apt-get install -y build-essential cmake git pkg-config
   sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler
   sudo apt-get install -y libatlas-base-dev 
   sudo apt-get install -y --no-install-recommends libboost-all-dev
   sudo apt-get install -y libgflags-dev libgoogle-glog-dev liblmdb-dev
   ```

   Download the latest source archive for OpenCV 3.1 from https://github.com/opencv/opencv. (Do not download it from http://opencv.org/downloads.html, because the official OpenCV 3.1 does not support CUDA 8.0.) You can git clone the archive or download the .zip file to downlodas fodler. Here we use .zip file as an example.
   
   > $unzip opencv-master.zip
   
   > $unzip opencv_contrib-master.zip
   
   Move the unpacked folders to your home dir ~/ , and enter opencv-master, Execute:
   
   > $mkdir build
   
   > $cd build/
   
   > $cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_CUBLAS=ON -DCUDA_NVCC_FLAGS="-D_FORCE_INLINES" -D OPEN_EXTRA_MODULES_PATH=~/opencv_contirb-master/modules .. 
   
 Don't forget the two dots at the end in the above command. The OPEN_EXTRA_MODULES_PATH option allows you to use the non-free SIFT and SURF algorithms stores in ~/opencv_contrib-master. It takes some time, and you should see your make setup in your output. https://github.com/opencv/opencv_contrib
  
 Then make file using with multiple threads (such as -j12) to accelerate.

 > $make clean
 
 > $make -j $(($(nproc) + 1))
  
 Note: Always **make clean** before **make -j12** (is not pass, sudo make)
 
 ### Integration with the Caffe

 Return to the Caffe directory and perform a cleanup operation with the command

 make clean

 (Read more here: https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide)

 First, edit the Makefile.config to include the OpenCV 3.1 library like this...
 
 > OPENCV_VERSION := 3

 > LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial /usr/local/share/OpenCV/3rdparty/lib/

 Then, recompile the entire Caffe project.
 
 
  
## 5. Install caffe

* Go to the https://github.com/BVLC/caffe and download the zip archive. Unpack it to ~/bin/ or any other location. Enter the caffe-master directory in the terminal window.

* Copy the Makefile.config.example to Makefile.config like this:

```
cp Makefile.config.example Makefile.config
```

* and open it for editing (with a text editor). I use the kate editor for this purpose, so the command that I execute goes as follows. You first need to install the kate editor with:

```
sudo apt-get install kate
```

* and then you can edit the configuration file with:

```
kate ./Makefile.config &
```

* The following line in the configuration file tells the program to use CPU only for the computations. 

 > CPU_ONLY := 1

* ** CPU_ONLY option is enabled for a computer without any NVIDIA graphics card**
 Change the line if needed, by commenting it out (# CPU_ONLY := 1) if you have an NVIDIA graphics card with the proprietary driver, CUDA toolkit and CUDNN installed. Jump to the end of this guide to read about how to install the GPU support prerequisites.

 The Makefile.config should contain the following lines, so find them and fill them in.

 > PYTHON_INCLUDE := /usr/include/python2.7 /usr/lib/python2.7/dist-packages/numpy/core/include  

 Uncomment:
 
  > uSE_CUDNN := 1


 > WITH_PYTHON_LAYER := 1

 > INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial

 > LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial

 (For ways to create an isolated Python environment, explore the topic of virtual environments here: http://docs.python-guide.org/en/latest/dev/virtualenvs/)

 If your CUDA is 8.0, find this in Makefile.config..

 > CUDA_DIR := /usr/local/cuda

 And replace it with this..

 > CUDA_DIR := /usr/local/cuda-8.0


 Now lets continue with the instructions for the Ubuntu 15.10 first, followed by the instructions for Ubuntu 16.04 users. 


 Now for both platforms lets return to the unpacked Caffe directory caffe-master and enter these commands:

  ```
  cd python

  for req in $(cat requirements.txt); do pip install $req; done
  ```


  --------------------------------------------------------------------------------------------------------------


 NOTE: If the Ubuntu operating system was updated, perhaps the Python layer needs to be updated and recompiled, because the Python module no longer works. Perform this step again in that case.

 ```
 for req in $(cat requirements.txt); do pip install $req; done
 ```

 In case of any problems, try:

 ```
 for req in $(cat requirements.txt); do sudo -H pip install $req --upgrade; done
 ```

 The default Python version is 2. You can edit the Makefile.conf to enable the Python 3, but this will fail during the linking phase: boost_python3 cannot be found on Ubuntu 16.04. Instead, this file should be /usr/lib/x86_64-linux-gnu/libboost_python-py35.so.1.58.0. This requires further testing. 

 --------------------------------------------------------------------------------------------------------------


 The next step is to build Caffe:

 ```
 cd ..
 ```
 (now you are in caffe-master directory)

 The build process will fail in Ubuntu 16.04. Edit the Makefile with an editor such as 

 ```
 vim ./Makefile
 ```
 and replace this line:
 ```
 NVCCFLAGS += -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
 ```
 with the following line
 ```
 NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
 ```

 (See the discussion at: https://github.com/BVLC/caffe/issues/4046)

 When compiling with OpenCV 3.0 or errors show `imread`,`imencode`,`imdecode` or `VideoCapture`
 open your Makefile with some text editor, add opencv_imgcodecs behind.
 ```
 LIBRARIES += glog gflags protobuf leveldb snappy \
  lmdb boost_system boost_filesystem hdf5_hl hdf5 m \
  opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs opencv_videoio
 ```
 (See the discussion at: https://github.com/BVLC/caffe/issues/1276)

 Then
 ```
sudo make all -j8
sudo make test
sudo make runtest
sudo make pycaffe      -should be finished already, so you can omit this one
sudo make distribute
 ```

 Note that the build process can be sped up by appending -j $(($(nproc) + 1)) to the above commands, which distributes the build across the available processors on your system. For example:

 `make all`

 can become

 `make all -j $(($(nproc) + 1))`

 In order to make the Python work with Caffe, open the file ~/.bashrc for editing in your favorite text editor. There, add the following line at the end of file:

 > export PYTHONPATH=/path/to/caffe-master/python:$PYTHONPATH

 You can also execute that same line immediately as a command for immediate effects.

 In order to use the Caffe binaries, libraries, or include files, they need to be reachable through the search path, so one solution is to copy them into their respective directories: from the distribute directory to the /usr/bin or /usr/lib or /usr/include.

 The binary models can be download with the following script. In caffe-master directory, 

 ```
cd scripts
./download_model_binary.py ../models/bvlc_alexnet/
./download_model_binary.py ../models/bvlc_googlenet/
./download_model_binary.py ../models/bvlc_reference_caffenet/
./download_model_binary.py ../models/bvlc_reference_rcnn_ilsvrc13/
./download_model_binary.py ../models/finetune_flickr_style/
 ```

* For more models, see https://github.com/BVLC/caffe/wiki/Model-Zoo *


 For most Linux programs compiled from source, you can attempt to build a package that can be installed and uninstalled with a single click.

 ```
 sudo apt-get install checkinstall
 ```

 Now, when you execute the:

 ```
sudo checkinstall
 ```

 and fill out a form with some easy questions, you will have the package made automatically. However, this uses the command "make install" in the background, which will fail, because the Caffe project does not have the target "install" configured in the Makefile.

------------------------------------------------------------------------------------------------------------------------
Note:

You may also need to manually update your libprotobuf2.6.1 to libprotobuf3.0.0 
https://packages.debian.org/sid/libprotobuf-dev


