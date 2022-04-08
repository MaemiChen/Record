## 1. 简述
在Qt中，事件就是对象，派生自QEvent抽象类，用来表示应用程序中发生的事件，或者应用程序需要处理的外部活动。
events可以被任何QObject派生的子类实例对象接收处理

## 2. 事件发送
当事件发生时Qt构造一个合适的QEvent表示事件的发生，然后通过调用event函数，将该事件发送给特定的QObject对象

## 3. 事件类型
QT为多数事件建立了对应的类，常见的有：QResizeEvent、QPaintEvent、QKeyEvent、QCloseEvent，都继承自QEvent，并添加特定的事件函数。
1. Qt事件发送机制
QCoreApplication::notify(QObject * receiver, QEvent * event) virtual  
发送事件给接收者。返回从接受者事件处理程序返回的值。适用于应用程序的任何线程的任何对象。对于特定类型的事件（如鼠标和键盘事件），该事件被传送到接收者的parent并逐级上传。

## 4. 事件处理方式
1. 重写paintEvent、mousePressEvent，常用、简单、有限的方式
2. 重写QCoreApplication::notify() 非常强大，可以完全控制事件处理，但一次只可以用于一个子类
3. 为QCoreApplication::instance()安装一个事件过滤器。事件过滤器能够处理所有控件的所有事件，因此和重写notify一样强大。
4. 重写QObject::event(),如果重写了QObject::event，当tab按下时，可以在任何控件事件过滤器捕获
5. 给相应的接收对象安装一个事件过滤器

## 5. 发送事件
1. QCoreApplication::sendEvent(); 立即处理要发送的事件，当它返回时，表示对应的事件过滤器或目标对象已经处理完该事件。对于多数的事件类，有一个isAccept()函数判断该事件是否已经接受处理，或被拒绝处理了。
2. QCoreApplication::postEvent(); 将事件提交到事件队列中等待调度。在下一次Qt主事件循环运行时，会调度所有提交到队列的事件，以某种优化的方式。例如：有几个大小改变事件，它们会被压缩为一个事件，这同样适用于绘图事件QWidget::update()调用postEvent，避免多次画图来消除闪烁和提高速度


## 扩展1-事件过滤器
为事件设置一个事件过滤器QObject::eventFilter()，过滤器可以在需要的时候检测并丢弃事件。可以使用QObject::removeEventFliter()移除已安装的事件过滤器。  
可以使用installEventFilter，为目标对象设置一个事件过滤器
https://blog.csdn.net/qq_31877249/article/details/86014932