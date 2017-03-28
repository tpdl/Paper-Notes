#How to set up VPA-Faster RCNN Environment on Ubuntu 16.04 :

# 1. Install Nvida Driver

If you own an NVIDIA graphics card, see the instructions for the installation of NVIDIA Graphics Driver, Cuda Toolkit and CUDNN library at the end of this document, or by clicking https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide#the-gpu-support-prerequisites.
The GPU support prerequisites

In Ubuntu desktop, enable the use of proprietary drivers in the Software & Updates Center for your desktop and install the NVIDIA graphics driver from the main Ubuntu package repository. See https://help.ubuntu.com/community/BinaryDriverHowto/Nvidia

Discover which driver number you need with:
```
   sudo ubuntu-drivers devices
```

##2. Install Cuda

The LATEST version of Cuda Toolkit 8.0 is available from the NVIDIA website. Download the Cuda Toolkit 8.0 network installer and the CUDNN 5.1 package from the NVIDIA site, after registering and filling out the forms. https://developer.nvidia.com/cuda-downloads

Install the Cuda Toolkit 8.0 package manually in the terminal as instructed at the website. For example...
```
   sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
   sudo apt-get update
   sudo apt-get install cuda

```

##3. Install cuDNN

##4. install opencv3

For the instructions on how to use the OpenCV version 3.1, please see https://github.com/BVLC/caffe/wiki/OpenCV-3.1-Installation-Guide-on-Ubuntu-16.04
```
   sudo apt-get install --assume-yes build-essential cmake git
   sudo apt-get install --assume-yes build-essential pkg-config unzip ffmpeg qtbase5-dev python-dev python3-dev python-numpy python3-numpy
   sudo apt-get install --assume-yes libopencv-dev libgtk-3-dev libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev    sudo apt-get install --assume-yes libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
   sudo apt-get install --assume-yes libv4l-dev libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev
   sudo apt-get install --assume-yes libvorbis-dev libxvidcore-dev v4l-utils
```
In order to install the NVIDIA Cuda Toolkit with CUDNN library
The following guide includes the how-to instructions for the installation of BVLC/Caffe on Ubuntu 16.04 with Cuda Toolkit 8.0, CUDNN 5.1 library and OpenCV version 2 or 3. (A small record remains from the previous tutorial for Ubuntu 15.10 with the Cuda Toolkit 7.5, but that part will not be updated any further.) This guide also covers the KUbuntu distribution and the related distributions.
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
##5. Install caffe

