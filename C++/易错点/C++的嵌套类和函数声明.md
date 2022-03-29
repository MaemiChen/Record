
##
头文件
````C++
#pragma once
#ifndef A_H
#define A_H

class A1;
class  A
{
public:
	 A();
	~ A();

private:
	//类外部进行前置声明
	A1 *d;

	//类内部进行前置声明,A2在源文件中进行声明和定义（仅A可以使用A2）
	class A2;
	A2* d2;

	//嵌套类 在类中进行声明（仅A可以使用A3）
	class A3 {
		void Stop();
	};
	A3* d3;
};
#endif // A_H
````

源文件
````C++
#include "A.h"

class A1
{
	void Stop();
};
void A1::Stop() {

}

class A::A2
{
	void Stop();
}; 

void A::A2::Stop() {

}

void A::A3::Stop() {

}

A::A() {
	d = new A1();
	d2 = new A2();
	d3 = new A3();
}

A::~A() {
	delete d,d2,d3;
}
````

## 参考
嵌套类参考： https://blog.csdn.net/Poo_Chai/article/details/91596538
