# C++中static的简述

## staic 类成员
1. static类数据成员独立于其他数据成员存在，static类数据成员是于类关联的，不和类定义的对象有任何关系
2. static类对象必须在类外进行初始化。因为static修饰的对象先于对象存在，所以static修饰的类对象必须先于类初始化
3. static修饰的成员变量在对象中不占内存，在静态存储区生成，所以可以节省对象内存空间
4. static类属于静态存储区，不属于对象，所以没this指针。this指针是指向本对象的指针。因为没有this指针，所以static类成员函数不能访问非static的类成员。




## 参考资料
https://www.cnblogs.com/fuqia/p/8888938.html