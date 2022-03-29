## decltype
decltype与auto功能一样，都是在编译期间进行类型推导。
> decltype 是declare type的缩写，译为声明类型

auto和decltype自动推导变量类型的区别
````C++
auto varName = value;
decltype(exp) varName2 = value;
````
auto根据=右边初始值推导出变量类型，而decltype根据exp表达式推导出变量类型和value无关。

### delctype推导规则
- exp是一个不被（）包围的表达式，或者是一个类成员访问表达式，或一个单独变量，那么decltype(exp)类型和exp的类型一致。
- 如果exp是函数调用，那么decltype(exp)的类型和函数返回值类型一致。
- 如果exp是一个左值，或者被（）包围，那么decltype(exp)的类型就是exp的引用。

http://c.biancheng.net/view/7151.html

## extern
可以在一个文件中引用另一个文件中定义的变量或函数。
https://zhuanlan.zhihu.com/p/348762602