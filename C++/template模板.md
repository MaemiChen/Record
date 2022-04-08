## 函数模板

```C++
template<typename T> void swap(T& t1, T& t2){
    T temT;
    temT = t1;
    t1 = t2;
    t2 = temT;
}
```

## 类模板
```C++
template<class Item> class MaxHeap{

}
```
- `类模板只能声明和定义在一起实现，不能分开声明和定义??`

## 模板之typename和class关键字的区别  

- 在大多数情况下，使用typename和class是一样的。（在声明一个template type parameter）
- 在C++中，有的时候必须使用typename，`当T是一个类，而这个类又有子类(假设名为innerClass)，应该使用template<typename>??`。  
- typename可以使用嵌套依赖类型，也就是类型可以嵌套使用。

```C++
template<class T>
struct man 
{
	public:
		typedef typename T::value_type	value_type;
		typedef typename T::pointer		pointer;
		typedef typename T::reference	reference;
		void print()
		{
			cout << "man" << endl;
		}
};
```
以上的typename表示告诉编译器这不是个函数，也不是变量，而是一个类型。这里使用typedef将这个类型起个别名。

https://blog.csdn.net/Function_Dou/article/details/84644963

## 模板的实例化过程
https://blog.csdn.net/qianqin_2014/article/details/51586846  
https://www.cnblogs.com/tonychen-tobeTopCoder/p/5199655.html （模板声明和定义相分离）    

传统情况下分离声明和定义的话，编译器会生成两个.obj，一个是templateTest.obj，一个是main.obj。  
在main.obj中对函数的调用只会生成一行call指令

`由于函数模板需要实例化之后才能成为真正的函数，编译器无法实例化该模板，最终导致链接错误`

？
模板需要在`标准期`就解析，展开为对应类型的代码。