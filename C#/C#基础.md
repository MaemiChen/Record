## 数据类型
- 值类型
- 引用类型
引用类型不包含存储在变量中的实际数据，但包含对对变量的引用
    * object  
    1. 对象（Object）类型 是 C# 通用类型系统（Common Type System - CTS）中所有数据类型的终极基类
    2. Object可以分配任何类型，值类型、引用类型、预定义类型、用户自定义类型
    * dynamic 
    1. 可以任何类型的值在动态数据类型变量中，变量类型检查是在运行时发生的。
    * string
    1. 字符串（String）类型的值可以通过两种形式进行分配：引号和 @引号。
    2. C# string 字符串的前面可以加 @（称作"逐字字符串"）将转义字符（\）当作普通字符对待，@ 字符串中可以任意换行，换行符及缩进空格都计算在字符串长度之内
- 指针类型

## 访问修饰符
public 、 private 、protected 、 internal 、 protected internal

## 可空类型
````C#
int ? a = null;
int ? b = 2;

double? num1 = null;
double? num2 = 3.14157;
double num3;
num3 = num1 ?? 5.34;      // num1 如果为空值则返回 5.34
````

## 数组
数组是一个引用类型，需要通过new来创建数组的实例。
double[] balance = new double[10];  

在声明数组的同时给数组赋值
double[] balance = { 2340.0, 4523.69, 3421.0};

1. 多维数组
int[,] array = {{1,2},{3,4}};
int val = array[1,0];
2. 交错数组  
数组的数组  
int[][] scores = new int[2][]{new int[]{92,93,94},new int[]{85,66,87,88}};
3. 参数数组
声明一个方法时，您不能确定要传递给函数作为参数的参数数目。  
C# 提供了 params 关键字，使调用数组为形参的方法时，既可以传递数组实参，也可以传递一组数组元素。

## C#结构体特点
结构特点：
1. 结构不能声明默认的构造函数，不能定义析构函数，这都是自动定义的。
2. 结构体不支持继承，包括继承和被继承
3. 类是引用类型，结构是值类型

需要一个带有参数的构造函数可以有参数，这种构造函数叫做参数化构造函数。

C# 类不允许多重继承，可以使用接口来实现多重继承

## 多态
1. 静态多态  
函数重载  
运算符重载
2. 动态多态  
使用abstract 创建抽象类，派生类继承自抽象类
类前面增加sealed修饰，表示为密封类，不能被继承  

    虚函数

## 异常处理
一个方法使用 try 和 catch 关键字捕获异常。try/catch 块内的代码为受保护的代码。
1. C# 中的异常类主要是直接或间接地派生于 System.Exception 类。System.ApplicationException 和 System.SystemException 类是派生于 System.Exception 类的异常类。
2. System.ApplicationException 类支持由应用程序生成的异常。所以程序员定义的异常都应派生自该类。


|异常类 |	描述 |
| --- | --- |
| System.IO.IOException	| 处理 I/O 错误。|
System.IndexOutOfRangeException|	处理当方法指向超出范围的数组索引时生成的错误。
System.ArrayTypeMismatchException	|处理当数组类型不匹配时生成的错误。
System.NullReferenceException	|处理当依从一个空对象时生成的错误。
System.DivideByZeroException|	处理当除以零时生成的错误。
System.InvalidCastException|	处理在类型转换期间生成的错误。
System.OutOfMemoryException|	处理空闲内存不足生成的错误。
System.StackOverflowException|	处理栈溢出生成的错误。
| | |
````C#
using System;
namespace ErrorHandlingApplication
{
    class DivNumbers
    {
        int result;
        DivNumbers()
        {
            result = 0;
        }
        public void division(int num1, int num2)
        {
            try
            {
                result = num1 / num2;
            }
            catch (DivideByZeroException e)
            {
                Console.WriteLine("Exception caught: {0}", e);
            }
            finally
            {
                Console.WriteLine("Result: {0}", result);
            }

        }
        static void Main(string[] args)
        {
            DivNumbers d = new DivNumbers();
            d.division(25, 0);
            Console.ReadKey();
        }
    }
````

## 特性
特性的指定方法为：将括在方括号中的特性名置于其应用到的实体的声明上方。

特性（Attribute）用于添加元数据，如编译器指令和注释、描述、方法、类等其他信息。.Net 框架提供了两种类型的特性：<font color=yellow>预定义特性和自定义特性。</font>
### 预定义特性
- AttributeUsage
描述如何自定义特性类
````C#
[AttributeUsage(
   validon,
   AllowMultiple=allowmultiple,
   Inherited=inherited
)]

    // 参数 validon 规定特性可被放置的语言元素。它是枚举器 AttributeTargets 的值的组合。默认值是 AttributeTargets.All。
    // 参数 allowmultiple（可选的）为该特性的 AllowMultiple 属性（property）提供一个布尔值。如果为 true，则该特性是多用的。默认值是 false（单用的）。
    // 参数 inherited（可选的）为该特性的 Inherited 属性（property）提供一个布尔值。如果为 true，则该特性可被派生类继承。默认值是 false（不被继承）。

[AttributeUsage(AttributeTargets.Class |
AttributeTargets.Constructor |
AttributeTargets.Field |
AttributeTargets.Method |
AttributeTargets.Property, 
AllowMultiple = true)]
````
- Conditional  
它会引起方法调用的条件编译，取决于指定的值，比如 Debug 或 Trace。例如，当调试代码时显示变量的值。  
[Conditional("DEBUG")]
- Obsolete  
预定义特性标记了不应被使用的程序实体。  
它可以让您通知编译器丢弃某个特定的目标元素。
````C#
//语法
[Obsolete(
   message
)]
[Obsolete(
   message,
   iserror
)]


    // 参数 message，是一个字符串，描述项目为什么过时以及该替代使用什么。
    // 参数 iserror，是一个布尔值。如果该值为 true，编译器应把该项目的使用当作一个错误。默认值是 false（编译器生成一个警告）。

````
### 自定义特性

...

## 反射
反射指程序可以访问、检测和修改它本身状态或行为的一种能力。
