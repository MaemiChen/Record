# QT中CMake简述

## 简介
在QT中使用的CMake简述  
下载地址https://cmake.org/  
允许开发者编写一种平台无关的CMakeLists.txt制定整个编译流程，根据目标用户的平台进一步生成本地化的Makefile和工程文件  
编译成功后在QT Creator中显示如下
![image](CMakeLists/CMake_compiler.png)  


## CMake和qmake的区别
1. qmake，需要一个.pro文件
2. cmake，需要一个CMakeLists.txt

## Linux下编译CMake
1. 编写CMake配置文件CMakeLists.txt
2. 执行命令cmake path或ccmake path生成Makefile。其中path是CMakeLists所在的目录


## 示例
````C++

#CMake最低版本号要求
cmake_minimum_required(VERSION 3.5)

#项目信息
project(tms-ate LANGUAGES CXX)

#生成可执行文件
add_executable(
    main.cpp
    ...
)

#链接其他的库文件
target_link_libraries

````

## QT中CMake自动生成
![image](CMakeLists/QT_CMake_build.png)

