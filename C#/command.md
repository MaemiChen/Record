## 简述
- wpf命令使命令源（即命令发送者，也称调用程序）和命令目标（命令执行者，也称处理程序）分离。  
- 对于程序而言，命令就是一个个任务，如保存、复制、剪切这些操作。
- 使得控件的启用状态和相应的命令状态保持同步，即命令被禁用时，绑定命令的控件也会被禁用。

## 命令模型介绍
命令模式属于对象的行为型模式。命令模式是把一个操作或者行为抽象为一个对象中。通过对命令的抽象化使得发出命令的责任和执行命令的责任分隔开。命令模式的实现可以提供命令的撤销和恢复功能。

## 参考链接
https://www.cnblogs.com/zhili/p/WPFCommand.html  
https://www.cnblogs.com/Jax/archive/2009/10/12/1581109.html （AttachBehavior）