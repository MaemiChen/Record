## 说明
wpf至少都有两个线程。一个用于UI绘制，隐藏于后台，另一个用于管理UI，包括相应用户输入执行后台代码。MSDN描述：“Typically, WPF applications start with two threads;one for handling rendering and anther for managing the UI.The rendering thread effectively runs hidden in background while the UI thread receives input,handles events,panints the screen,and run application code. Most application use a single UI thread,although in some situation it is best to use several.” the UI thread 就是所谓的UI线程。

wpf规定，UI元素只能由创建该元素的线程来访问。Dispather来维持这一规定，并组织着消息循环。dispatcher负责检测访问对象的线程和对象创建线程是否一致。不一致则抛出异常。  
其中dispatcher的消息循环中work item是有优先级的，可以让高优先级的项拥有更多的工作时间。`比如界面绘制比处理用户输入的优先级要高。`

## 对于阻塞的操作
### 1. 不开启新线程，降低处理它的work item 优先级

### 2. 使用invoke和begininvoke开启新线程

### 3. 创建新线程执行操作

### 4. background实质: 基于事件的异步模式

## dispatcher
dispatcher相当于win32的消息队列，在dispatcher的内部函数中仍然调用了传统的创建窗口类，创建窗口，创建消息泵。
- dispatcher本身是一个单例模式，构造函数私有，暴露一个静态的currentDispatcher()方法用于获得当前线程的dispatcher。dispatcher内部维护了一个static 的list\<Dispatcher> _dispatchers，当使用currentdispatcher方法时会遍历这个list，如果没有则创建新的dispatcher对象并加入list。创建dispatcher对象时会把当前线程复制给thread属性，下次遍历的时候可以通过thread匹配查找。
- 在dispatcher构造函数时，调用了HwndWarpper构造函数，在HwndWarpper构造函数中`创建窗口类和创建窗口`。
- dispatcher具有循环读取消息的消息泵。


https://www.cnblogs.com/zhouyinhui/archive/2008/01/27/1055261.html  
[深入理解dispatcher工作原理](https://cloud.tencent.com/developer/article/1341628)