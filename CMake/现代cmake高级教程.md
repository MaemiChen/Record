## cmake软件构建和安装
传统cmake
![传统cmake](./img/%E4%BC%A0%E7%BB%9Fcmake%E8%BD%AF%E4%BB%B6%E6%9E%84%E4%BB%B6%E5%92%8C%E5%AE%89%E8%A3%85.png)
现代cmake构建可以使用-B --build
![](./img/%E7%8E%B0%E4%BB%A3cmake%E6%9E%84%E5%BB%BA.png)

## cmake指定配置 -D
cmake的项目构建分为两步：
- cmake -B build 称为`配置阶段configure`，只检测环境并生成构建规则。`在build目录下生成本地构建系统能够识别的项目文件（Makefile 或 .SLN）`
- cmake --build build 称为`构建阶段build`，这个时候才`实际调用编译器来编译代码` 

1. 在`配置阶段可以通过-D设置缓存变量`，在第二次配置时，之前的-D仍会被保留。

## cmake指定要用的生成器 -G
这个也是要在配置阶段添加，如：cmake -B build -G Ninja。  
- 和操作系统绑定的构建系统（make、MSbuild）称为本地构建系统（native buildsystem）
- 负责从cmakelist.txt 生成本地构建系统构建规则文件的，称为生成器（generator）

## 添加源文件
### 1. 
add_executable(main main.cpp) 

### 2.   
add_executable(main)
target_sources(main PUBLIC main.cpp) 

### 3. 使用变量来存储  
```cmake
add_executable(main)
set(source main.cpp other.cpp)
target_source(main PUBLIC ${source})
```
### 4. 使用GLOB自动查找当前目录指定扩展名文件，实现批量添加源文件
```cmake
add_executable(main)
file(GLOB sources *.cpp *.h)
target_source(main PUBLIC ${sources})
```
1. CONFIGURE_DEPENDS 添加新文件时，自动更新变量
```cmake
add_executable(main)
file(GLOB sources CONFIGURE_DEPENDEDS *.cpp *.h)
target_source(main PUBLIC ${sources})
```

2. GLOB_RECURSE 递归查找当前目录及子目录
```cmake
add_executable(main)
file(GLOB_RECURSE sources CONFIGURE_DEPENDEDS *.cpp *.h) # 能自动包括所有子文件夹下的目录，但是会包含/build文件夹下的文件
target_source(main PUBLIC ${sources})
```

### 使用aux_source_directory，自动搜集需要的文件后缀名
```cmake
add_executable(main)
aux_source_directory(. source)  #.代表当前目录
aux_source_directory(mylib source)  # mylib下的文件
target_source(main ${source})
```

## 项目配置变量
### 1. CMAKE_BUILD_TYPE
cmake中的一个特殊的变量，用于控制构建类型，他的值有
- Debug 调试模式，完全不优化，生成调试信息
- Release 发布模式，优化程度最高，性能最佳，编译比debug慢
- MinSizeRel 最小体积发布，生成的文件比release更小，完全不优化，减少二进制体积
- RelWithDebInfo 带调试信息发布，生成的文件比release更大。
- 默认情况下CMAKE_BUILD_TYPE 为空，默认为Debug
```cmake
cmake_mininum_required(VERSION 3.15)
project(hellocamke LANGUAGES CXX)

set(CMAKE_BUILD_TYPE Release)

add_executable(main main.cpp)
```

### 2. project 初始化项目信息，并把cmakelist.txt文件所在目录作为根目录
- CMAKE_CURRENT_SOURCE_DIR 当前源码的目录
- CMAKE_CURRENT_BINARY_DIR 当前输出目录
- PROJECT_NAME 项目名
- PROJECT_SOURCE_DIR 当前项目的源码目录 （建议使用）
- PROJECT_BINARY_DIR 当前项目的输出目录（建议使用）

- PROJECT_IS_TOP_LEVEL 表示当前项目是否为最顶层的根项目
- CMAKR_PROJECT_NAME 根项目的项目名

- CMAKE_CXX_STANDARD 表示C++使用的标准
- CMAKE_CXX_STANDARD_REQUIRED 表示是否一定支持指定的C++标准
- CMAKE_CXX_EXTENSION on 表示启用gcc特有的一些扩展功能，off则关闭gcc的扩展功能，只能使用标准C++。要兼容其他编译器（MSVC），会设置为off
```cmake
cmake_mininum_required(VERSION 3.15)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED on)
set(CMAKE_CXX_EXTENSION on)

project(hello LANGUAGES C CXX)
add_executable(main main.cpp)
```

1. project VERSION
- PROJECT_VERSION_MAJOR 主版本号
- PROJECT_VERSION_MINOR 次版本号
- PROJECT_VERSION_PATCH 补丁版本号
2. 项目名会设置 <项目名>_SOURCE_DIR 等变量
3. ${} 表达式可以嵌套使用

### 3. 标准cmakelist.txt模板
```cmake
cmake_minimum_required(VERSION 3.15)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED on)

project(hello LANGUAGES C CXX)

if (PROJECT_SOURCE_DIR STRQUAL PROJECT_BINARY_DIR)
    message(WARNING "The binary directory of CMake cannot be the same as source directory")
endif

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif

if (WIN32)
    add_definitions(-DNOMINMAX -D_USE_MATH_DEFINES)
endif

if (NOT MSVC)
    find_program(CCMAKE_PROGRAM ccache)
    if (CCMAKE_PROGRAM)
        message(STATUS "find CCache: ${CCMAKE_PROGRAM} ")
        set_property(GLOBAL PROPERTY PULE_LAUNCH_COMPILE ${CCMAKE_PROGRAM})
        set_property(GLOBAL PROPERTY PULE_LAUNCH_LINK ${CCMAKE_PROGRAM})
    endif
endif
```

## 链接库
### 静态库
```cmake
add_library(mylib STATIC mylib.cpp)

add_executable(main main.cpp)

target_link_libraries(main PUBLIC mylib)
```
生成.a文件
### 动态库
```cmake
add_library(mylib SHARED mylib.cpp)

add_executable(main main.cpp)

target_link_libraries(main PUBLIC mylib)
```
生成.so文件
### 对象库
```cmake
add_library(mylib OBJECT mylib.cpp)

add_executable(main main.cpp)

target_link_libraries(main PUBLIC mylib)
```
不生成文件，只由cmake记住该库生成了哪些对象文件。
`对象库是cmake自创的。绕开了编译系统和操作系统的繁琐规则，保证跨平台的统一性，对象库仅作为代码组织方式`

## 对象属性
### set_property
```cmake
add_executable(main main.cpp)
set_property(TARGET main PROPERTY WIN32_EXECUTABLE on) # 在Windows系统，运行时启动控制台窗口（默认off）
set_property(TARGET main PROPERTY LINK_WHAT_YOU_USE on) # 告诉编译器不要自动剔除没有引用符号的链接库（默认off）
set_property(TARGET main PROPERTY LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib) # 设置动态链接库输出路径，默认为${CMAKE_BINARY_DIR}
set_property(TARGET main PROPERTY ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib) # 设置静态链接库输出路径，默认为${CMAKE_BINARY_DIR}
set_property(TARGET main PROPERTY RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib) # 设置可执行文件输出路径，默认为${CMAKE_BINARY_DIR}
```
### set_target_properties 批量设置多个属性
```cmake
add_executable(main main.cpp)
set_target_properties(main PROPERTIES
WIN32_EXECUTABLE on
LINK_WHAT_YOU_USE on
)
```

## 链接第三方库
### 1. 在Linux下使用tbb
```cmake
add_executable(main main.cpp)
target_link_libraries(main PUBLIC tbb)
```
cmake会在系统的库目录里查找tbb，他会找到/usr/lib/libtbb.so系统自带的，对于没有一个固定库安装位置的Windows系统并不适用。此外还要求tbb头文件就在/usr/include这个系统默认头文件目录。这样才能#include\<tbb/parallel_for.h>不出错。如果tbb头文件在其他地方，还需要加一个target_include_directories设置额外的头文件查找目录。
### 2. 在Windows下使用tbb
方法1 
```
add_executable(main main.cpp)
target_link_libraries(main PUBLIC C:/Windows/tbb.dll) # 指定lib路径
target_include_directories(main PUBLIC xxx) # 指定头文件路径
```
方法二  
find_package(TBB REQUIRED)会查找/usr/lib/cmake/TBB/TBBConfig.cmake这个配置文件，并根据里面的配置文件创建TBB::tbb这个伪对象（实际指向真正的tbb库文件路径），之后使用target_link_libraries
```cmake
add_executable(main main.cpp)
include_directories()   # 添加库文件的配置文件 .cmakes所在的文件夹
find_package(TBB CONFIG REQUIRED)  #查找库文件的配置文件
target_link_libraries(main PUBLIC TBB::tbb)
```
- 添加一个CONFIG选项，会优先查找tbbConfig.Cmake,而不是查找FindTBB.cmake。
- COMPONENTS 指定需要哪些组件
```cmake
find_package(Qt5 COMPONENTS Widgets Gui REQUIRED)
target_link_libraries(main PUBLIC Qt5::Widgets Qt5::Gui)
```

## 输出与变量

### message
- status 带这个参数表示信息类型是状态类型，有--前缀。
- warning 表示警告，以黄色字体显示。
- author_warning 和warning一样，但是可以通过-Wno-dev关闭。
- fatal_error 错误信息，会中止cmake运行
- send_error 表示错误信息，但不中止执行


## 变量与缓存
![](./img/cmake%E9%87%8D%E5%A4%8D%E6%89%A7%E8%A1%8C%E9%85%8D%E7%BD%AE.png)
删除缓存可以仅删除 /build/CMakeCache.txt 这个文件里面装的缓存的变量，删除它就可以让CMake强制重新检测一遍所有的库和编译器。

### 设置缓存变量
set(变量名 "变量值" CACHE 变量类型 "注释")
```cmake
set(strvar "hello" CACHE STRING "this is docstring")
```

当我们想要修改的时候，建议使用 `cmake -B build -Dstrvar=world`。
- linux中可以使用`ccmake -B build` 启动基于终端的可视化缓存编辑菜单。
- windows中可以使用`cmake-gui -B build`
- 也可以直接编辑 cmakecache.txt文件。

变量类型有哪些
- STRING 字符串
- FILEPATH 文件路径
- PATH 目录路径
- BOOL bool值

### 示例
`添加一个bool类型的缓存，用于控制要不要启用某特性`
```cmake
add_exeutable(main main.cpp)

set(WITH_TBB on CACHE BOOL "set on to enable tbb,off to disable tbb")
if (WITH_TBB)
    target_compile_definitation(main PUBLIC WITH_TBB)
    find_package(TBB REQUIRED)
    target_add_libraries(main PUBLIC TBB::tbb)
endif
```
### option
对于bool类型缓存的set指令的简写为option
```cmake
option(WITH_TBB "set on to enable tbb,off to disable tbb" on) # 等价于下一句
set(WITH_TBB on CACHE BOOL "set on to enable tbb,off to disable tbb")
```

## 跨平台与编译器

### 在cmake中定义一个宏
target_compile_definitation(mian PUBLIC MY_MACRO=233)

## 判断不同操作系统
CMAKE_SYSTEM_NAME 变量
```cmake
if (CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_compile_definitation(main PUBLIC MY_NAME="win")
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
    target_compile_definitation(main PUBLIC MY_NAME="linux")
endif
```

1. cmake 提供了一些操作系统的简写变量
- WIN32 对所有Windows都适用为TRUE
- APPLE 所有苹果类产品TRUE
- UNIX 对所有UNIX类系统都为TRUE

## 分支与判断
if 判断变量时不需要添加${}，会自动尝试作为变量名求值
## 变量与作用域
- 父模块里面定义的变量，会传递给子模块，但是子模块不会传递给父模块
- 子模块向父模块向上传递变量时，需要`set(MYVALUE ON PARENT_SCOPE)`

### 环境变量
```cmake
${MYVAR} # 表示局部变量
$ENV{PATH} #表示环境变量，表示获取PATH这个环境变量的值
```
### 缓存变量
`$CACHE{PATH}`，访问缓存中的PATH变量

## add_custom_target add_custom_command
https://blog.csdn.net/qq_38410730/article/details/102797448  
在cmake中，还需要创建一些目标，如`clean` `copy`等，这些需要通过add_custom_target来指定

## 地址
https://www.bilibili.com/video/BV16P4y1g7MH?spm_id_from=333.337.search-card.all.click