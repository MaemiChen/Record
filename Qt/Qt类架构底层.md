![](https://images2015.cnblogs.com/blog/1071697/201705/1071697-20170518111722963-605387953.png)
# 信号与槽机制
多态的底层实现机制：1. 按照名称查找；2. 按照位置查找。  
C++虚函数是按照位置查表，QT信号槽是按照名称查表。都是函数回调。  

## MOC
全称：Meta-Object Complier,元对象编译器。  
Qt程序在交给标准编译器编译之前，要先使用moc分析C++源代码。如果头文件中包含Q_OBJECT，则生成另一个C++源文件，这个源文件包含Q_OBJECT宏的实现代码，新文件名在原文件前加moc_构成。这个新的文件同样进入编译系统，最终被链接到二进制代码中。新的文件不是替代旧的文件，而是和原文件一起参与编译，moc的执行是在预编译之前。  
qt将源码交给C++标准编译器之前需要将扩展的语法更改为标准语法，这里使用的是moc  
![](https://images.cnblogs.com/cnblogs_com/swarmbees/1454266/o_cl.png)  

Qt元对象QMetaObject，这个类里面存储了父类的源对象、我们当前类的描述、函数描述、qt_static_metacall函数地址
### qt_static_metacall
根据函数索引调用槽函数，在这个回调中，信号和槽都可以被回调，信号也可以当做槽函数一样使用。

## connect
![](https://images.cnblogs.com/cnblogs_com/swarmbees/1454266/o_connect.png)  
````C++
QScopedPointer<QObjectPrivate::Connection> c(new QObjectPrivate::Connection);
c->sender = s;   //发送者
c->signal_index = signal_index;//信号索引
c->receiver = r;//接收者
c->method_relative = method_index;//槽函数索引
c->method_offset = method_offset;//槽函数偏移 主要是区别于多个信号
c->connectionType = type;//连接类型
c->isSlotObject = false;//是否是槽对象 默认是true
c->argumentTypes.store(types);//参数类型
c->nextConnectionList = 0;//指向下个连接对象
c->callFunction = callFunction;//静态回调函数，也就是qt_static_metacall

QObjectPrivate::get(s)->addConnection(signal_index, c.data());
````

## 信号触发
Qt提供了5种连接方式
Qt::AutoConnection 自动连接，根据sender和receiver决定使用哪种连接方式，同一个线程使用直连，否则使用队列连接  
Qt::DirectConnection 直连  
Qt::QueuedConnection 队列连接  
Qt::BlockingQueuedConnection 阻塞队列连接 跨线程的，希望槽执行完后，才能执行信号的下一步代码  
Qt::UniqueConnection 唯一连接  
![](https://images.cnblogs.com/cnblogs_com/swarmbees/1454266/o_emit_signals.png)

### 直连
同一线程中处理的，即函数回调，在connect对象中存储了回调函数地址  

### 队列连接
槽函数抛出QMetaCallEvent事件，经过Qt的事件循环达到异步的效果。

### 总结
Qt实现方式是函数回调。直连是直接调用，队列连接是使用Qt的事件循环隔离一次达到异步


# 事件循环机制
## 常见的事件队列
 鼠标事件(QMouseEvent) 键盘事件QKeyEvent 绘制事件QPaintEvent 窗口尺寸改变QResizeEvent 滚动事件QScrollEvent 控件显示QShowEvent 控件隐藏QHideEvent 定时器使劲QTimeEvent

 ## Qt事件接收
 QObject是所有类的基类，是Qt对象模型的核心，QObject的功能就是事件处理，通过event()函数调用获取事件。

 ## 事件处理流程
 大多数情况被分发到队列中，队列中有事件就发送给QObject对象，队列为空时阻塞等待事件。  
 QCoreApplication::exec()开启循环，到QCoreApplication::exit()终止  

 postEvent() 异步事件分发  
 sendEvent() 同步事件分发  

 ## 事件过滤器机制
 事件处理流程如图：  
 ![](https://img-blog.csdn.net/20180926153829700?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbW9ueXVjc2R5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
 事件的传送和处理流程第一步是事件过滤器eventFilter，对象A可以给对象B安装对象处理器，实现对对象B的监听和拦截，A为监听器，B为接收器。一个对象可以被多个事件监听。事件过滤器返回true表示事件处理完毕，否则传递给下一个监听器或接收器本身。

 ## 事件队列顺序
 事件循环是由exec()开启的
 QCoreApplication::exec();
 QApplication::exec();
 QDialog::exec();
 QThread::exec();
 QDrag::exec();
 QMenu::exec();
 可以使用QEventLoop::quit()终止事件循环
    1. 事件循环是可以嵌套的，在子事件循环时，父事件循环实际上处于中断状态，当子循环跳出才执行父循环中事件。**这不代表循环子循环时，类似父循环中的界面会被中断。因为子循环中也会有父循环大部分事件**
    2. 若子循环依然有效，父循环被强制跳出，此时父循环不会立即执行跳出，会等待子循环事件跳出后，父循环才会跳出。


## 参考资料
1. https://www.cnblogs.com/senior-engineer/p/11677098.html  
2. [qt 实战大佬]https://blog.csdn.net/liang19890820/article/details/50277095#json
3. [Qt事件循环机制]https://blog.csdn.net/simonyucsdy/article/details/82749539
4. [Qt 信号与槽]https://www.cnblogs.com/swarmbees/p/10816139.html
