## 函数的异常声明列表
```C++
//C++允许函数在声明或定义的时候，加上它所能抛出的列表
	void exception() throw(int, double);    //std::bad_typeid,std::bad_cast
//函数定义
    void container::exception()  {
	double m, n;
	std::cin >> m >> n;
	try {
		std::cout << "before dividing" << std::endl;
		if (n == 0) {
			throw - 1;
		}
		else if (m == 0) {
			throw - 1.0;
		}
	}
	catch (int i) {	//抓取throw为int类型的异常
		std::cout << "catch int: " << i << std::endl;
	}
	catch (double d) {	//抓取throw为double类型的异常
		std::cout << "catch double: " << d << std::endl;
	}
	catch (...) {	//处理所有异常
		std::cout << "catch all exception" << std::endl;
	}

	std::cout << "finish" << std::endl;
	
}
```
http://c.biancheng.net/view/422.html

## C++标准异常类
这些类都是从exception类派生来的。
- bad_typeid  
    在使用typeid运算符时，如果操作数是一个多态类的指针，而该指针的值为NULL，则会抛出异常
- bad_cast  
    在使用dynamic_cast进行从多态基类（或引用）到派生类的引用的强制类型转换，如果转换是不安全的，则会抛出此异常。
- bad_alloc  
    在用new运算符进行动态内存分配时，如果没有足够的内存，则会引发此异常。默认情况下，输入输出流对象不会抛出此异常。
- ios_base::failure  
    
- logic_error -> out_of_range  
    使用vector或string的at成员函数根据下表访问元素时，如果下标越界，则抛出此异常