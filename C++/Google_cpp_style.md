# 1： 类


## 1. 构造函数(Constructor)
> 1. 进行没有意义的初始化，使用Init()方法集中初始化有意义的数据。  
> 2. <font color = red>构造函数中不能调用虚函数，不会转发到子类中</font>  
> >在构造函数中调用虚函数，被调用的是这个函数的本地版本，虚机制在构造函数中不起作用  
> > 当一个构造函数被调用时，它做的首要的事情之一就是初始化它的VPTR。然而，它只能知道它属于“当前”类——即构造函数所在的类。于是它完全不知道这个对象是否是基于其它类。当编译器为这个构造函数产生代码时，它是为这个类的构造函数产生代码——既不是为基类，也不是为它的派生类（因为类不知道谁继承它）。所以它使用的VPTR必须是对于这个类的VTABLE。而且，只要它是最后的构造函数调用，那么在这个对象的生命期内，VPTR将保持被初始化为指向这个VTABLE。但如果接着还有一个更晚派生类的构造函数被调用，那么这个构造函数又将设置VPTR指向它的VTABLE，以此类推，直到最后的构造函数结束。VRTP的状态是由被最后调用的构造函数确定的。这就是为什么构造函数调用是按照从基类到最晚派生类的顺序的另一个理由。  
>> 但是，当这一系列构造函数调用正发生时，每个构造函数都已经设置VPTR指向它自己的VTABLE。如果函数调用使用虚机制，它将只产生通过它自己的VTABLE的调用，而不是最后派生的VTABLE（所有构造函数被调用后才会有最后派生的VTABLE）。另外，许多编译器认识到，如果在构造函数中进行虚函数调用，应该使用早绑定，因为它们知道晚绑定将只对本地函数产生调用。无论哪种情况，在构造函数中调用虚函数都不能得到预期的结果。
> 3. 对单参数构造函数使用explicit关键字  
>> <font color = yellow>C++中只有一个参数的构造函数除了具有构造函数功能外，还是一个隐式的类型转换符。  
>> 如下图，当没有explicit时，A a = 1;是可以进行隐式自动转换,使用显式声明explicit可以避免这种转换</font>
````C++
class A
{
private:
    int m_i;
public:
    explicit A(int i)
    {
        m_i = i;
    }
};

int main()
{
    A a = 1;  
    return 0;
}
````
> 4. 拷贝构造函数（copy constructors）
>> 仅在代码中需要拷贝一个类的对象的时候使用拷贝构造函数，不需要是使用宏DISALLOW_COPY_AND_ASSIGN
>> 大量的类不需要拷贝，不需要拷贝构造函数或赋值操作，<font color = yellow>编译器会自动生成public版本的拷贝构造函数</font>
---
## 2. 结构体和类（struct class）
只有数据时使用struct，其他使用class  
<font color = yellow>如果和STL结合，对于仿函数（functors）和特性（traits）可以使用struct而非class</font>  

---
## 3. 接口(interface)
接口是指满足特定条件的类，以interface为后缀（非必须）  
满足以下条件时为纯接口：
1. <font color = red>只有纯虚函数virtual fun = 0;和静态函数</font>
2. 没有非静态数据成员
3. 没有定义任何构造函数。如有，则不含参数，且为protected
4. 如果是子类，也只能继承自满足以上条件且为Interface的类


# 2：C++其他特性

## 1. 引用参数
按照引用传递的参数必须加上const。  
如果函数需要改变变量的值，形参(parameter)，int foo(int *pval)。
在函数形参中，引用为const，传出参数为指针 

## 2. 类型转换cast
1. static_cast 当指针的父类或子类明确向上转换（子类转换为基类）
2. const_cast 移除const属性
3. reinterpret_cast 指针类型或整型或其他不安全的相互转换，需要开发者谨慎使用
4. dynamic_cast 除测试外不要使用  

## 3. 前置自增自减
前置自增比后置自增的效率更高，因为后置自增需要对表达式i进行一次拷贝，++i，对于迭代器和模板类型，要使用前置自增

# 3： 命名约定

## 1. 文件命名
文件命名全部小写

## 2. 类型命名
类型命名全部大写字母开头，不包含下划线，所有类型（类、结构体、类型定义typedef、枚举）  
MyExcitingClass、 MyExcitingEnum


---
## 扩展1 -虚函数
### 1. 简述：
为满足多态和泛型编程，C++允许用户使用虚函数（virtual function）完成**运行时决议**，与一般的编译时决议有本质区别  
泛型技术：使用不变的代码实现可变的算法
### 2. 实现原理
由两方面组成，虚函数指针和虚函数表  
1. 虚函数指针（virtual function pointer）  
> 一个指向函数的指针，指向用户所定义的虚函数，是在子类里面的具体实现，子类调用虚函数时，通过虚函数指针找到接口。  
> 虚函数是确实存在的数据类型，在一个被实例化对象中，它被放在该对象的地址首位。这样的目的是为了保证运行的快速性。虚函数指针对外完全不可见，除非直接访问地址，或者使用debug模式。  
> 只有拥有虚函数的类才能拥有虚函数指针，每个虚函数对应一个虚函数指针。拥有虚函数类的对象会因虚函数产生额外的开销
2. 虚函数表（virtual table）  
> 由于每个实例化对象都拥有虚函数指针且都排列在对象地址首部<font color = red>虚函数表示类对象之间共享的，而非每个对象保存一份读取到的是虚指针</font>，所以它们按照一定顺序组织起来的，构成一种表状结构，称为虚函数表（virtual table）  
首先定义一个<font color = red>基类</font> 
````C++
class Base{
    public:
    virtual void f(){
        cout << "Base::f" << endl;
    }
    virtual void g(){
        cout << "Base::g" << endl;
    }
    virtual void h(){
        cout << "Base::h" << endl;
    }
}
````
![基类](https://images0.cnblogs.com/blog/514794/201307/11192917-932f16a9db654164b4f8a9d2b3602f09.jpg)  
> 子类重新实现虚函数
````C++
class Derived:public Base
{
	public:
    virtual void f(){cout<<"Derived::f"<<endl;}
    virtual void g1(){cout<<"Derived::g1"<<endl;}
    virtual void h1(){cout<<"Derived::h1"<<endl;}
}
````
> <font color = red>一般为覆盖继承，相同的覆盖，不同的增加</font>  
![虚函数表](https://images0.cnblogs.com/blog/514794/201307/11195425-795497a4edd54824815aecb5688ad0c3.jpg)  
> 多重继承的时候，表项增加，顺序体现为继承顺序，子函数的虚函数在第一个表项后
![](https://images0.cnblogs.com/blog/514794/201307/11202442-57573dac02f14024b79840ca7b0cc815.jpg) ![](https://images0.cnblogs.com/blog/514794/201307/11202508-9575b5f392f04f71a64194264dcbdd4d.jpg)  
### 3. 同名覆盖和const修饰符  
 > 只有子函数中的虚函数和父函数完全一致的时候（包括限定符const等）才能认为是真正的虚函数，否则为重载

 ### 4. 纯虚函数
1. 定义：  
定义一个虚函数表示允许基类的指针调用子类的函数，不代表为不被实现的函数。  
定义一个函数为纯虚函数，表示函数没有被实现  
纯虚函数是在基类声明的虚函数，在基类中没有定义，只提供了一个接口，要求派生类定义自己的实现方法。  
virtual fun() = 0  
基类一般为抽象类，不生成其对象

---
## 扩展2 -面向对象复合（composition）、委托（delegation）、继承（inheritance）
### 1. 复合composition
> 类A类B两个类，类A包含类B  
> 如下代码所示，<font color = red>queue通过deque已完成的功能去实现自己的功能，类似这种模式称为复合composition</font>
````C++
template<class T>
class queue{
    ...
protected:
    deque<T> c;

public:
    //使用对象c的操作函数实现
    bool empty() const{
        return c.empty();
    }
}
````
> 对于composition复合而言，<font color = red>composition构造函数由内到外，构造函数应先调用deque的默认default构造函数，然后再构造queue。析构函数是由外到内的</font>
---
### 2. 委托delegation composition by reference
> 可以看做为一种复合composition，但是用指针相连
````C++
template<class T>
class queue{
    ...
protected:
    deque<T> *c;

public:
    //使用指针c实现功能
    bool empty() const{
        return c.empty();
    }
}
````
---
### 3. 继承
......

---
## 扩展3-智能指针
### 1. 简述  
C++中有4个智能指针：<font color = red>auto_ptr unique_ptr shared_ptr weak_ptr</font>,其中auto_ptr已被弃用  
主要用于管理堆上分配的内存，将指针封装为一个栈对象，当栈的生命周期结束时，在析构函数中释放申请掉的内存，防止内存泄漏。  
常用的智能指针为shared_ptr，采用引用计数方法，记录当前内存资源被多少个智能指针引用。该引用计数在堆内分配。当新增引用计数加1，当过期引用计数减一。引用计数为0时，智能指针自动释放引用的内存资源。  
智能指针是一个类，当超出类的实例对象的作用域时，自动调用对象的析构函数，析构函数自动释放资源。
### 2. auto_ptr  
````C++
//采用所有权的方式，已被C++11所抛弃
auto_ptr(string) p1 (new string("I reigned lonely as cloud."));
auto_ptr(string) p2;
p2 = p1;    //不会报错，但是p2剥夺了p1的所有权，当程序访问p1时会报错，存在潜在的内存崩溃的问题。
````
### 3. unique_ptr
````C++
//采用所有权的方式
auto_ptr(string) p3 (new string("I reigned lonely as cloud."));
auto_ptr(string) p4;
p2 = p1;    //会报错，在同一时间内，只有一个智能指针可以指向该对象
````

### 4. shared_ptr
实现共享式拥有概念，多个智能指针可以指向相同对象，该对象和相关资源在“最后一个引用被销毁”时释放。  
use_count()资源所有者的个数。除了可以用new构造，还可以传入auto_ptr,unique_ptr,weak_ptr构造，调用release时，当前指针释放资源所有权，计数减一。  
成员函数  
use_count 引用计数的个数  
unique 是否独占所有权use_count 为1  
swap 交换shared_ptr对象  
reset 放弃内部对象的所有权或拥有对象的变更，会引起原有对象的引用计数的减少  
get 返回内部对象的指针，由于重载方法()，和直接使用对象是一样的。

### 5. weak_ptr
当两个对象互相使用一个shared_ptr成员对象指向对方，会造成循环引用，引用失效造成内存泄漏。  
weak_ptr是一种不控制对象生命周期的智能指针，指向一个shared_ptr管理的对象，进行该对象的内存管理的强引用的shared_ptr,weak_ptr只是提供管理对象的一个访问手段

## 扩展4-静态成员函数和非静态成员函数/数据成员

### 1. 成员函数(静态非静态)
静态和非静态都是属于类的，非静态由类的对象调用，静态函数由类名(::)或对象名(.)调用，静态函数不传递this指针，不识别对象个体。  

<font color = yellow>类的静态成员(变量和方法)属于类本身，在类加载的时候就会分配内存，可以通过类名直接访问；非静态成员属于类对象，只有在类对象创建时分配内存，通过对象去访问</font>  

静态成员可以实现对象之间的数据共享

### 2. 数据成员(静态非静态)
<font color = yellow>1.  静态数据成员在程序编译时分配空间的，在程序结束时释放空间。静态数据成员是在类外单独开辟空间。   
2.静态数据成员可以初始化，但只能在类外进程初始化 </font>

## 扩展5-RTTI
RTTI(run time type information/identification) 展示C++在运行过程中data type的机制，对数据类型进行动态判断。使用内建的RTTI运算符typeid和dynamic_cast
### 1. typeid
当需要具体的类型信息时，可以通过成员函数来提取
````C++
    int n = 100;
    const type_info &nInfo = typeid(n);  
    cout<<nInfo.name()<<" | "<<nInfo.hash_code()<<endl;//输出为int和hash值

    //获取一个结构体的类型信息
    const type_info &stuInfo = typeid(struct STU);
    cout<<stuInfo.name()<<" | "<<stuInfo.hash_code()<<endl;//输出为结构体类型和hash值
````
一般用于比较两个类型是否相等

### 2. static_cast
static_cast < type-id > ( exdivssion ) 
将表达式exdivssion转换为type-id类型，没有运行时类型检查保证转换的安全性  

用于类层次结构中基类和子类之间指针或引用的转换。  
1. 把子类转换为基类是安全的，把基类转换为子类，由于没有动态类型检查，所以是不安全的。
2. 用于基本类型之间的转换，把int转为char，int转为enum等，安全性需要开发人员确认
3. 把空指针转换为目标类型空指针
4. 把任何类型转换为void类型  

static_cast 不能转换掉exdivssion的const、volitale或_unaligned属性

### 3. dynamic_cast
exdivssion转换为type_id类型的对象，type_id必须是类的指针、类的引用或void*型  

如果type-id是类指针类型，那么exdivssion也必须是一个指针，如果type-id是一个引用，那么exdivssion也必须是一个引用。   

dynamic_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。
在类层次间进行上行转换时，dynamic_cast和static_cast的效果是一样的；
在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全。

````C++
class B
{
public:
    int m_iNum;
    virtual void foo();
};
 
class D : public B
{
public:
    char *m_szName[100];
};
 
void func(B *pb)
{
    D *pd1 = static_cast(pb);
    D *pd2 = dynamic_cast(pb);
}
````
在上面的代码段中，如果pb指向一个D类型的对象，pd1和pd2是一样的，并且对这两个指针执行D类型的任何操作都是安全的；
但是，如果pb指向的是一个B类型的对象，那么pd1将是一个指向该对象的指针，对它进行D类型的操作将是不安全的（如访问m_szName），
而pd2将是一个空指针。  
<font color = yellow>dynamic_cast 必须要有虚函数，因为运行时类型检查这个信息存在虚函数表里面，static_cast则没有这个约束</font>


## 参考文档
[虚函数实现](https://blog.csdn.net/haoel/article/details/1948051)   
[智能指针](https://www.cnblogs.com/WindSun/p/11444429.html)
