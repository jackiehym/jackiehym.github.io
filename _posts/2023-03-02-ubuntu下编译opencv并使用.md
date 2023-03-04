---
title: 在Ubuntu20.04下编译OpenCV并简单使用
author: handsome boy
date: 2023-03-02 22:20:00 +0800
categories: [编程, 编译]
tags: [Cpp, C++, OpenCV]
toc: true
comments: false
---

## 第一步，安装依赖包

```terminal
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev 
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev liblapacke-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libatlas-base-dev gfortran 
sudo apt-get install ffmpeg
```

第四行会报错，所以

```terminal
sudo add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"
sudo apt update
sudo apt install libjasper1 libjasper-dev
```

## 第二步，编译安装

记得把contrib放在opencv下

```terminal
sudo mkdir build && cd build
sudo cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=~/developer-tools/Opencv/opencv-3.4.15/opencv_contrib-3.4/modules/ ..
sudo make -j${nproc} # 慎选，会使得电脑卡顿
```

## 第三步，配置文件

- `sudo vim /etc/ld.so.conf.d/opencv.conf`，在行尾添加`/usr/local/lib`  
- `ldconfig`让其生效  
- `sudo vim /etc/bash.bashrc`，在行尾添加`export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig`  
- `source /etc/bash.bashrc` 更新bash  
- 检验是否安装成功：`pkg-config opencv --modversion`  

## 第四步，运行

- g++ xxx.cpp -o xxx pkg-config --cflags --libs opencv
- ps.pkg前面为反引号  

```cmake
# 这个文件来自OpenCV源代码解压出来的文件夹下的/samples/cpp/example_cmake/文件夹下的
# CMakeLists.txt文件，并做修改


# cmake needs this line
cmake_minimum_required(VERSION 2.8)

# Define project name
project(Test)

# Find OpenCV, you may need to set OpenCV_DIR variable
# to the absolute path to the directory containing OpenCVConfig.cmake file
# via the command line or GUI
find_package(OpenCV REQUIRED)

# If the package has been found, several variables will
# be set, you can find the full list with descriptions
# in the OpenCVConfig.cmake file.
# Print some message showing some of them
message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

if(CMAKE_VERSION VERSION_LESS "2.8.11")
  # Add OpenCV headers location to your include paths
  include_directories(${OpenCV_INCLUDE_DIRS})
endif()

# Declare the executable target built from your sources
add_executable(Test test_opencv.cpp)

# Link your application with OpenCV libraries
target_link_libraries(Test PRIVATE ${OpenCV_LIBS})
```

然后移到终端

```terminal
cmake .  
make
./Test tifa.jpg
```

## 参考链接

- [Ubuntu18.04安装OpenCV依赖包libjasper-dev无法安装的问题](https://zhuanlan.zhihu.com/p/405446750)
- [Ubuntu下的Opencv的安装与初步使用](https://zhuanlan.zhihu.com/p/536514154  )
- [Ubuntu配置OpenCV终极解决方案](https://zhuanlan.zhihu.com/p/368573848)
- [在linux环境下编译运行OpenCV程序的两种方法](https://www.cnblogs.com/woshijpf/p/3840530.html)
