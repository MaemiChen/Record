## 简述

`借助"第三方"实现具有依赖关系的对象之间的解耦` 即`IOC容器`  

![](https://img-blog.csdnimg.cn/c9899cc96f1445cbb0e0ec785e03d77e.png)  

## 控制反转的由来
在没有引用IOC容器前，对象A依赖对象B，那么对象A在初始化或者运行到某一点的时候，自己必须主动去创建对象B或者使用已经创建的对象B。无论是创建还是使用对象B，`控制权都在自己手上`。  

在引用IOC容器后，情况改变。由于IOC容器的加入，对象A和对象B失去了直接联系。因此，当对象A运行到需要B的时候，`IOC容器会主动创建一个对象B注入到对象A需要的地方`

## IOC也叫DI（Dependency Injection）
控制被反转之后，获得依赖对象的过程由自身管理变为了由IOC容器主动注入。`所谓依赖注入，就是由IOC容器在运行期间，动态地将某种依赖关系注入到对象之中`

## Prism中的IOC容器
这个框架在一定程度上能够通过一个容器去管理整个框架中所有类的对象和生命周期。  
![](https://img2020.cnblogs.com/blog/695641/202111/695641-20211128213759041-689971682.png)

在prism.core中主要是定义一些基础的接口，这些接口是为了定义一种规范。Prism框架提供了两种具体的IOC依赖注入框架，分别为`DryIOC`和`Unity`

## IOC容器技术剖析
IOC中最基本的技术就是“反射（Reflection）”编程 TODO::待看两篇经典文章

## 参考链接
https://blog.csdn.net/ivan820819/article/details/79744797  (浅谈Ioc重要)
https://www.cnblogs.com/xingyukun/archive/2007/10/20/931331.html （经典文章-深度理解依赖注入Dependency Injection）  
https://www.cnblogs.com/zhenyulu/articles/641728.html （经典 理解依赖注入）
https://blog.csdn.net/qq_34202873/article/details/85042409
https://blog.csdn.net/zhudaokuan/article/details/111227077
https://www.cnblogs.com/loverwangshan/p/10485044.