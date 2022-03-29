## 成员函数
- get_id() 获取线程id,返回std::thread::id的对象
- joinable() 检查线程是否可被join，检查thread对象是否标识为一个active的可行性线程
- join() 阻塞当前线程
- detach() 将当前线程对象所代表的执行实例和该线程对象分离，使线程的执行可以单独进行。一旦线程执行完毕，它分配的资源将会被释放。
- native_handle() 该函数返回std::thread 具体实现相关的线程句柄。native_handle_type是连接thread类和操作系统SDK API之间的桥梁
- swap() 交换两个线程对象所代表的底层句柄
- hardware_concurrency() static函数，返回当前计算机最大的硬件并发线程数目。基本上可视为处理器的核心数目。

https://blog.csdn.net/sevenjoin/article/details/82187127  
https://blog.csdn.net/coolwriter/article/details/79883253