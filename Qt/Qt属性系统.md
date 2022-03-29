## 1. 简述
Qt提供一个类似其他编译器提供商提供的负载属性系统(property system)。  
Q_PROPERTY()这个宏用来声明类的属性，类继承自QObject。属性就想类的成员变量，但是它有同感Meta-OBject system的其他功能
## 2. 声明属性的要求
1. 要在继承QObject的类中使用Q_PROPERTY()宏