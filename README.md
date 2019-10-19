Logger1
=======

Tool for logging RGB-D data from the Microsoft Kinect and ASUS Xtion Pro Live. 

Should build on Linux, MacOS and Windows. Requires CMake, Boost, Qt4, OpenNI, ZLIB and OpenCV. 

Grabs RGB and depth frames which are then compressed (lossless ZLIB on depth and JPEG on RGB) and written to disk in a custom binary format. Multiple threads are used for the frame grabbing, compression and GUI. A circular buffer is used to help mitigate synchronisation issues that may occur. 

Uses OpenNI 1.x.

The binary format is specified in Logger::writeData() in Logger.cpp

<p align="center">
  <img src="http://mp3guy.github.io/img/Logger1.png" alt="Logger1"/>
</p>

#Installing Logger1 on Ubuntu 16

#Requires
 
#CMake, Boost, Qt4, ZLIB

sudo apt-get install -y cmake-qt-gui git build-essential libusb-1.0-0-dev libudev-dev freeglut3-dev libglew-dev libsuitesparse-dev libeigen3-dev zlib1g-dev libjpeg-dev libboost-all-dev qt4-default

sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-7-jdk

#OpenCV /Documents/deps/opencv-2.4.9/

#Building OpenCV from scratch without Qt and with nonfree
wget http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.9/opencv-2.4.9.zip
unzip opencv-2.4.9.zip
cd opencv-2.4.9
mkdir build
cd build
cmake -D BUILD_NEW_PYTHON_SUPPORT=OFF -D WITH_OPENCL=OFF -D WITH_OPENMP=ON -D INSTALL_C_EXAMPLES=OFF -D BUILD_DOCS=OFF -D BUILD_EXAMPLES=OFF -D WITH_QT=OFF -D WITH_OPENGL=OFF -D WITH_VTK=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_TESTS=OFF -D WITH_CUDA=OFF -D BUILD_opencv_gpu=OFF -D WITH_GTK=ON ..
make -j8
sudo make install
echo "/usr/local/lib" | sudo tee -a /etc/ld.so.conf.d/opencv.conf
sudo ldconfig
echo "PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig" | sudo tee -a /etc/bash.bashrc
echo "export PKG_CONFIG_PATH" | sudo tee -a /etc/bash.bashrc
source /etc/bash.bashrc
cd ../..

#OpenNI 1.x /Documents/deps/

git clone https://github.com/OpenNI/OpenNI
sudo apt-get install pkg-config libxmu-dev libxi-dev python
sudo apt-get install doxygen mono-complete graphviz

cd Platform/Linux/CreateRedist
chmod +x ./RedistMaker
./RedistMaker
cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.2.23/
sudo ./install.sh

#Building Logger1 /home/tll/Documents/Logger1

mkdir build
cd build
cmake ../src
make -j8
