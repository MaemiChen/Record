1. binding  
  source = {binding} 和source = {binding RelativeSource={RelativeSource self},    Path=DataContext}效果相同

    理解：{binding} 不设定明确的绑定的source,这样binding就去从本控件类为开始根据可视树的层次结构自下而上查找不为空的Datacontext属性的值。

    {binding RelativeSource={RelativeSource self},Path=DataContext}中RelativeSource self的含义为绑定的source为控件自身，这样binding 就绑定了自身控件的Datacontext。

    https://www.cnblogs.com/zhlziliaoku/p/5867556.html

2. 委托和事件  
    1. 
    委托和事件起源是C/C++中的函数指针，C#中的委托更像一个函数指针链表。  
    2. 
    事件/消息机制是Windows的核心，其实提供事件功能的却是函数指针，你信么？接下来我们再看看C#事件(Event).在C#中事件是一类特殊的委托.
    
    https://www.cnblogs.com/liaocheng/p/4600008.html

3. C#中Invoke的用法  
    control.invoke(参数delegate)方法:在拥有此控件的基础窗口句柄的线程上执行指定的委托。
    control.begininvoke(参数delegate)方法:在创建控件的基础句柄所在线程上异步执行指定委托。

    invoke只能执行委托，event等是特殊的委托

4. 路由事件的三种策略  
    EventManager提供处理事件的工具方法  
    路由事件一般使用以下三种路由策略：
    1) 冒泡Bubble = 1,：由事件源向上传递一直到根元素。
    2) 直接Direct = 2：只有事件源才有机会响应事件。
    3) 隧道Tunnel = 0,：从元素树的根部调用事件处理程序并依次向下深入直到事件源。一般情况下，WPF提供的输入事件都是以隧道/冒泡对实现的。隧道事件常常被称为Preview事件。

5. DependencyProperty 依赖属性  
    1. 依赖属性就是一种可以自己没有值，并能通过使用Binding从数据源获得值（依赖在别人身上）的属性。拥有依赖属性的对象称为“依赖对象”。  
    2. WPF开发中，必须使用依赖对象作为依赖属性的宿主，使二者结合起来。依赖对象的概念被DependencyObject类所实现，依赖属性的概念则由DependencyProperty类所实现  
    https://www.cnblogs.com/sjqq/p/8458228.html  （依赖属性）  
    <font color = yellow>student类没有实现INotifyPropertyChange接口，当属性的值发生改变时与之关联的Binding对象依然可以得到通知，依赖属性默认带有这样的功能，天生就是合格的数据源。</font>

6. CLR属性、依赖属性与附加属性(WPF)  
    1. CLR属性：使用get set包装private字段的访问。CLR属性的编译结果是两个方法，仅仅是个语法糖衣
    2. 依赖属性（DependencyProperty）：在WPF中，推出了“依赖属性”这个概念。依赖属性自己没有值，通过Binding从数据源获得值的属性。拥有依赖属性的对象称为“依赖对象”。

    https://www.cnblogs.com/surpasslight/archive/2012/02/10/2345157.html

7. wpf binding（查看binding函数）
    1. Binding ElementName=rootControl,获取或设置要用作绑定源对象的元素的名称。
    2. Binding Path = FontSizeChangeProperty，需要绑定的查找路径
8. 一个依赖属性成为数据源的一个Path ?

9. volatile synchronized   
    volatile修饰后在所有线程中必须是同步的；任何线程中改变了它的值，所有其他线程立即获取到了相同的值  
    可能用到的情况：
    1) 并行设备的硬件寄存器（如：状态寄存器） 
    2) 一个中断服务子程序中会访问到的非自动变量(Non-automatic variables) 
    3) 多线程应用中被几个任务共享的变量 

10 x:static  

x:static 默认是不能进行类型转换的，但是可以使用  
````C#
Binding="{Binding Source={x:Static tcs:GlobalVariable.LoadOnlyOnlineEq}}"   //可以进行默认转换为propertypath  
Binding="{Binding Path={x:Static tcs:GlobalVariable.LoadOnlyOnlineEq}}"     //不可以进行默认转换
````

https://stackoverflow.com/questions/24221001/binding-source-vs-xstatic

11. xml:space="preserve"  
    这个是xml特性的一部分，是一个要么包括全部要不全部不包含的一个设置，一旦使用该设置，元素内所有空白字符被保存

12. IEnumerable IEnumerator yield  
参考链接https://blog.csdn.net/cyh1992899/article/details/52782818  
yield 为了简化遍历操作实现的语法糖[参考](https://www.cnblogs.com/kingcat/archive/2012/07/11/2585943.html)

13. Stopwatch  
提供一组方法和属性，可用于准确地测量运行时间。

14. ObjectDataProvider
包装和创建可以用作绑定源的对象。

15. ContextMenu
表示使控件能够公开特定于控件的上下文的功能的弹出菜单。
