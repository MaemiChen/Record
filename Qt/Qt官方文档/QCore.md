https://doc.qt.io/

https://doc.qt.io/qt-6/qtcore-index.html

# 1. Qt core
Qt core module 对C++增加模块如下  
1. 一个非常有效的机制对于对象的机制叫信号和槽
2. 可查询可设计的对象属性
3. 分层的可查询的对象树
4. 智能指针的自主管理[QPointer]()
5. 动态链接库是可以跨库执行的。动态链接是在运行时才会加载、动态转换也是在运行时才会加载。

包含以下属性  
1. [the meta-object system]()
2. [the property system]()
3. [object model]()
4. [object trees & ownership]()
5. [signals & slots]()

## 1. using the module
1. cmake
````C++
find_package(Qt6 COMPONENTS Core REQUIRED)
target_link_libraries(mytarget PRIVATE Qt6::Core)
````
2. qmake
Qt6 默认自动包含

## 2. 线程和并发设计
Qt 提供线程类模块[threading classes]()。一个安全的线程的方式是通过信号和槽进行跨线程。多线程设计在设计不冻结UI线程的同时执行耗时操作。  

[thread support in Qt]()提供了重载线程的操作。其他并发类可以参考[Qt Concurrent]()

## 3. 输入、输出、资源、容器
1. [container classes]()
2. [serializing Qt data types]()
3. [implict shared]()