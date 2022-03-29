# Qt 多线程详解

## 一： 最简方法
### 1. 创建线程类继承自QThread，重写run函数  
#### 简述
继承QThread只有run函数在新线程里面，其他函数都在QThread生成的线程里面  
任何继承自QThread的线程都是通过继承QThread的run函数来实现多线程的，必须重写QThread的run函数，复杂逻辑卸载QThread的run函数中  
<font color = red>QThread 在不调用exec() 情况下，exit() quit()是没有作用的，无法关闭线程  
terminate可以马上终止一个线程，但这个线程存在非常安定元素</font>  

````C++
//simpleThread.h
class simpleThread : public QThread
{
public:
    simpleThread();
    void run();
}

//simpleThread.cpp
void simpleThread::run(){
    while(true){
        qDebug() << "simpleThread run";
        sleep(20);
    }
}

//main.cpp
int main(int argc, char *argv[]){
    QApplication a(argc, argv);
    MainWindow w;
    w.show;

    simpleThread *SThread = new simpleThread;
    SThread->start();

    return a.exec();
}
````

#### 正确终止一个线程
添加一个bool变量，通过主线程修改bool变量来进行终止，但是可能存在访问冲突（即：其他在QThread中的函数修改了run函数的参数），需要加锁（#include \<QMutex> ）  
每次循环run的时候，判断lock== true，如果false则终止线程return
<font color= red>TODO::线程加锁问题，</font>


### 2. 创建线程类继承自QObject，通过SIGNAL/SLOT

````C++
//simple2Thread.h
class simple2Thread : QObject
{
public:
    simple2Thread();

public slots:
    void doSomething();
}

//simple2Thread.cpp
void simple2Thread::doSomething(){
    while(true){
        qDebug() << "simple2Thread run";
    }
}

//MainWindow.cpp
MainWindow::MainWindow(){
    QThread *objThread = new QThread;
    simple2Thread *S2Thread = new simple2Thread;

    simple2Thread->moveToThread(objThread);

    connect(this,SIGNAL(signal1()),simple2Thread，SLOT(doSomething));
    S2Thread->start();

    SendStart();
}

MainWindow::SendStart(){
    emit signal1();
}

````

## 二：线程基础
每个程序启动后拥有的第一个线程为主线程，即GUI线程，Qt所有组件类和几个相关的类只能工作在GUI线程。每个线程都有自己的栈，每个线程都有自己的调用历史和本地变量，线程共享相同的地址空间。  
QT提供了三种形式对线程的支持，分别为平台无关的线程类、线程安全的事件投递、跨线程的信号和槽的连接  
QThread 跨平台多线程解决方案   
QThreadStorage提供线程数据存储  
QMutex 提供相互排斥的锁，互斥量  
QReadWriteLock 同时读操作的锁  
QReadLocker、QWriteLocker自动对QReadWriteLock加锁解锁  
QSemaphore 提供整型信号量，是互斥量的泛化  
QWaitCondition 一种方法，线程可以在被另外线程唤醒之前休眠  

当线程启动和结束时，QThread会发送信号started()/finshed()，可以使用isStarted()/isFinished()查询线程状态


## 三：线程的同步
临界资源：每次只允许一个线程访问的资源  
线程间互斥：多个线程在同一时刻都需要访问临界资源  
线程锁能够保证临界资源的安全性，每个临界资源需要一个线程锁进行保护  
线程死锁：线程间互相等待临界资源而造成彼此无法继续执行

## 线程的优先级
TODO

## 参考资料
https://blog.csdn.net/u012635648/article/details/89504115  
https://www.pianshen.com/article/4123270938/  
https://blog.csdn.net/czyt1988/article/details/64441443 (两种继承方式的区别)
https://zhuanlan.zhihu.com/p/52612180（线程时间片）  
