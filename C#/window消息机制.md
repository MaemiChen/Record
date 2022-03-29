## windows消息机制
window系统以消息处理为其控制机制，系统通过消息为窗口过程（windows procedure）传递输出。系统和应用都可以产生消息。  
每个窗口都有一个处理windows系统发送消息的处理程序，称为窗口程序，它是隐藏在窗口后面的一段程序脚本，其中包含对事件进行处理的代码。  
<font color = yellow>Windows系统为每条消息指定了一个消息编号。eg,当窗口变为活动窗口时，它事实上是收到一条来自Windows系统的WM_ACTIVATE消息，消息编号为6.对应窗口的activate事件。</font>  

window消息处理机制为了能在应用程序中监控系统的各种事件消息，提供了挂接各种回调函数（hook）的功能，这种挂钩函数类似中断驱动程序，挂钩上可以挂接多个反调函数构成一个挂接函数链。<font color=yellow>系统产生的各种消息首先被送到各种挂接函数，挂接函数根据各自功能对消息进行监控、修改、控制，然后交还控制权或将消息传递给下一个挂接函数最后到达窗口程序</font>

## hook介绍 

Hook（钩子）是WINDOWS提供的一种消息处理机制平台，是指在程序正常运行中接受信息之前预先启动的函数，用来检查和修改传给该程序的信息，（钩子）实际上是一个处理消息的程序段，通过系统调用，把它挂入系统。每当特定的消息发出，在没有到达目的窗口前，钩子程序就先捕获该消息，亦即钩子函数先得到控制权。这时钩子函数即可以加工处理（改变）该消息，也可以不作处理而继续传递该消息，还可以强制结束消息的传递。  

  注意：安装钩子函数将会影响系统的性能。监测“系统范围事件”的系统钩子特
别明显。因为系统在处理所有的相关事件时都将调用您的钩子函数，这样您的系统将
会明显的减慢。所以应谨慎使用，用完后立即卸载。还有，由于您可以预先截获其它
进程的消息，所以一旦您的钩子函数出了问题的话必将影响其它的进程。记住：功能
强大也意味着使用时要负责任。

## hook链
window提供了14种不同类型的HOOKS；不同的HOOK处理不同的消息。  
Hook链表是一串由应用程序定义的回调函数（CALLBack Function），当某种类型的消息发生时，window向此种类型的hook链的第一个函数，（hook链顶部）发送该消息，在第一函数处理完该消息后由该函数向链表的下一个函数传递消息。  
最近安装的钩子放在链的开始，而最早安装的钩子放在最后，也就是后加入的先获得控制权。

## wpf处理Windows消息
1. Windows api钩子  
> SetWindowsHookEx（安装钩子）,UnhookWindowsHookEx（卸载钩子）,CallNextHookEx（指向hook链的下一个函数） API方式功能最强能实现系统级的消息处理  
2. WPF HwndSource  
> WPF提供的HwndSource可以使你更快的实现处理Windows消息。通过HwndSource.FromVisual得到的HwndSource可以添加(AddHook)移除(Remove)Hook。HwndSource有一定的局限性，如果你的WPF应用中包含Winform控件那你可能要有些失望了，这种方式不能接收到这类消息。
3. ComponentDispatcher
> 在互操作方案中，启用 Win32 与 WPF 之间的消息泵的共享控件，使用ComponentDispatcher的ThreadFilterMessage等事件可以更好的处理Windows消息。使用这种方式可以避免HwndSource的缺陷。
## 参考链接
1. https://www.cnblogs.com/wuyifu/archive/2013/01/17/2864399.html
2. https://www.cnblogs.com/Johness/archive/2012/12/28/2837977.html
3. https://www.cnblogs.com/therock/articles/2340465.html