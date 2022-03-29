## mutex C++11互斥类
- std::mutex 普通互斥类
- std::timed_mutex 表示定时互斥锁
- std::recursive_mutex 表示递归互斥锁  
    线程从成功调用lock或try_lock开始占有recursive_mutex。
    递归互斥锁可以被同一个线程多次加锁，以获得互斥锁对象的多层所有权

- std::recursive_timed_mutex 表示带定时的递归互斥锁
- std::shared_timed_mutex(C++14) 允许多个线程共享所有权的互斥对象，如读写锁等。

## 用于互斥锁的RAII类模板
“资源分配时初始化（RAII Resource Acquisition Is Initialization）”  

std::lock_guard类的构造函数禁用拷贝构造，且禁用移动构造。std::lock_guard类除了构造函数和析构函数外没有其它成员函数。  

 std::lock_guard在构造时只被锁定一次，并且在销毁时解锁

- std::lock_guard 严格基于作用域（scope_based）的锁管理类模板，构造时是否加锁时可选的（不加锁时假定当前线程已经获得锁的所有权），析构时自动释放锁，所有权不可转移，对象生存期间不允许手动加锁和释放锁
- std::unique_lock 更加灵活的锁管理模板
- std::shared_lock(C++14) 用于管理可转移和共享所有权的互斥对象。

## 互斥对象管理类模板的加锁策略
由于std::lock_guard std::unique_lock std::shared_lock构造时是否加锁是可选的，C++11提供了3种加锁策略

| | | |
| - | - | -|
| 策略 | tag type | 描述|
| 默认 | 无 | 请求锁，阻塞当前线程指导成功获得锁|
| std::defer_lock | std::defer_lock_t | 不请求锁|
| std::try_to_lock | std::try_to_lock_t | 尝试请求锁，但不阻塞线程，锁不可用时也会立即返回|
| std::adopt_lock |std::adopt_lock_t | 假定当前线程已经获得互斥对象的所有权，所以不再请求锁|


https://blog.csdn.net/nirendao/article/details/50890486  
https://blog.csdn.net/fengbingchun/article/details/73521630  
https://www.cnblogs.com/diegodu/p/7099300.html