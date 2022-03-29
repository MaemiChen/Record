## 1. 理解垃圾回收机制
CLR（Common Language Runtime公共语言运行时），和Java虚拟机一样是一个运行时环境，核心功能包括：内存管理、程序集加载、安全性、异步处理和线程同步。  
CTS（Common Type System）把.net中的类型分为：引用类型和值类型。.net所有类型都间接或直接派生自system.object。所有值类型都是system.valuetype的子类，system.valuetype本身为引用类型。  

## 2. 托管资源
使用垃圾回收机制GC进行自动回收。托管资源由CLR管理，并存在于托管堆上。  

垃圾回收过程：
1. CLR开始GC时，会暂停进程中所有的线程、这样可以防止线程在CLR检查期间访问对象并更改状态。
2. 对象越新，生存期越短；对象越老，生存期越长；回收堆的一部分，速度快于回收整个堆。

### 2.1 代的概念  
CLR初始化时为第0代对象选择一个预算容量（以KB为单位），如果分配一个新的对象造成第0代超过预算，就必须启动一次垃圾回收。垃圾回收后存活的对象变为了第一代对象。新对象分配在第0代对象中，再次第0代垃圾回收后进入第一代中。如果第一代的增加超出了预算，则对第一代进行垃圾回收，同时将存活的对象变为第二代对象。

### 2.2 总结
<font color=yellow>GC的基本工作原理：通过最基本的标记清除原理，清除不可达对象；再像磁盘碎片整理一样压缩、整理可用内存；最后通过分代算法实现性能最优化</font>

### 2.3 Finalization Queue和Freachable Queue
    这两个队列和Finalize方法有关。队列不存储真正的对象，而是存储一组指向对象的指针。在程序中使用new在Managed Heap中分配空间时，GC对其进行分析，如果该对象中含有Finalize方法则Finalization Queue中添加一个指向该对象的指针。GC启动垃圾回收后，发现垃圾中存在被Finalization Queue中指针指向的对象，则将这个垃圾分离出来，并将指向它的指针移动到Freachable Queue中。这个过程被称为是对象的复生。 

    当Freachable Queue中添加指针后，它就会触发所指对象的Finalize方法执行，然后删除该指针，对象就可以安静的四去看。  
    .net中提供了控制Finalize的方法，ReRegisterForFinalize：请求系统完成对象的Finalize的方法。（其实就是将指向对象的指针重新添加到Finalization Queue）SuppressFinalize：请求对象不要完成Finalize方法。

## 3. 非托管资源
非托管资源不由CLR管理。例如：<font color=yellow>Image / Socket / StreamWriter / Timer / Tooltip / 文件句柄 / GDI资源 / 数据库连接  / .etc</font>

在.net中释放非托管资源主要有两种方式Dispose、Finalize
1. Dispose方法，对象继承IDisposable接口，就会自动调用Dispose方法。
2. MSDN上的定义是允许对象在“垃圾回收”回收之前尝试释放资源并执行其他清理操作。
它的本质就是析构函数

说明：在net中应尽可能少的使用析构函数释放资源。实现 Finalize 方法或析构函数对性能可能会有负面影响

## 4. .net的算法
### 4.1 Mark-Compact 标记压缩算法
1. Mark-Sweep 标记清除阶段 
假设heap中所有对象都可以回收，找出不能回收的对象，给这些对象打上标记，heap中没有打标记的对象是可以被回收的。
2. Compact 压缩阶段
对象回收后内存空间变得不连续，在heap中移动这些对象，使他们重新从heap基地址开始连续排列，类似于磁盘空间的碎片整理。

主要处理步骤：将线程挂起=>确定roots=>创建reachable objectsgraph=>对象回收=>heap压缩=>指针修复

### 4.2 Generational 分代算法
 1. .NET将heap分成3个代龄区域: Gen 0、Gen 1、Gen 2   

2. Gen 0和Gen 1比较小，这两个代龄加起来总是保持在16M左右；Gen2的大小由应用程序确定，可能达到几G，因此0代和1代GC的成本非常低，2代GC称为fullGC，通常成本很高。粗略的计算0代和1代GC应当能在几毫秒到几十毫秒之间完成，Gen 2 heap比较大时fullGC可能需要花费几秒时间。大致上来讲.NET应用运行期间2代、1代和0代GC的频率应当大致为1:10:100。

## 参考链接
1. https://www.jianshu.com/p/cc57aa7f3824
2. https://www.cnblogs.com/cuiyiming/archive/2013/03/26/2981931.html
3. https://www.cnblogs.com/qixuejia/p/3957966.html