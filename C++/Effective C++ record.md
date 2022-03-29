# Effective C++ Record
### 导读
1. extern
    > extern表示声明为一个外部变量，具有外部链接性，可以被外部文件引用。  
    > 但是外部文件中不能有相同名字的外部变量

2. size_t和int的区别
    > size_t在C/C++标准在stddef.h中定义的，表示对象的大小。<font color = red>size_t的真实类型和操作系统有关</font>
    ````C++
    //在32位架构中普遍定义为:
    typedef unsigned int size_t;
    //在64位架构中普遍定义为：
    typedef unsigned long size_t;
    ````
    
    