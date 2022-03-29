## C类型转换
(类型说明符)表达式

## C++类型说明符
- static_cast<类型说明符>(表达式)
    >在编译期内即可决定其类型的转换  

- dynamic_cast<类型说明符>(表达式)  
    > 子类和父类之间的多态类型转换  

- const_cast<类型说明符>(表达式)
    >去掉const属性的类型转换，**目标类型只能是指针或引用**  

- reinterpret_cast<类型说明符>(表达式)  
    > reinterpret_cast不会改变运算对象的值，而是对该对象从位模式上进行重新解释  



https://zhuanlan.zhihu.com/p/33040213  
https://blog.csdn.net/qq_40421919/article/details/90677220