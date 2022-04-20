# Prism

## 1. Region 区域管理

在Prism中，一个页面不再为其固定显示的内容，而这个概念变成了Region划分的概念。将页面显示划分为N个Region，此时每个Region变成动态分配区域。  

1. 可以使用XAML或代码定义Region  
RegionManager.RegionName (XAML)  
RegionManager.SetRegionName (Code)  

2. 通过IRegionManager接口设置对应区域显示内容  

https://www.cnblogs.com/zh7791/p/14086734.html （具体设置详情参考）

3. 什么是RegionAdapters  
假设在应用程序的某个区域，需要显示我们定义的视图，这个时候实际利用了RegionAdapter。  
该类负责将传入我们定义的视图到指定的Region中。

//TODO:: 没看懂

## 2. Module 模块
## 3. View Injection 视图注入
## 4. ViewModelLocationProvider 视图模型定位
## 5. Command 绑定相关
## 6. Event Aggregator 事件聚合器
## 7. Navigation 导航
## 8. Dialog 对话框

## 9. Prism 框架的初始化过程
![](https://img2020.cnblogs.com/blog/1161656/202012/1161656-20201208130618189-1834347254.png)  

在Prism初始化过程中，除了构建自身容器、服务、适配器及一些区域行为后，开始创建应用程序首页Shell及加载模块，最终呈现。  
在app.xmal.cs中实现了两个抽象方法
- CreateShell
该方法返回一个Windows类型的窗口，就是返回应用程序的主窗口。
- RegisterTypes
该方法用于在Prism初始化过程中，我们定义自身需要的一些注册类型，以便在Prism中使用。

https://www.cnblogs.com/zh7791/category/1893907.html （这个是整体Prism合集）
https://blog.csdn.net/zhudaokuan/article/details/114266491 (Prism 框架介绍和创建)

https://www.cnblogs.com/zh7791/p/14140662.html