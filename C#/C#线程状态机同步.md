## ManualResetEvent
ManualResetEvent 和AutoResetEvent一样，是另外一种.NET线程同步技术。  
多个线程可以通过调用ManualResetEvent对象的WaitOne方法进入等待或阻塞状态。当控制线程调用Set()方法，所有等待线程将恢复并继续执行。  

我们初始化了一个值为False的ManualResetEvent对象，这意味着所有调用WaitOne放的线程将被阻塞，直到有线程调用了 Set() 方法。而如果我们用值True来对ManualResetEvent对象进行初始化，所有调用WaitOne方法的线程并不会被阻塞，可以进行后续的执行。  

通过waitOne的返回值可以安全结束线程的状态

## Task
Task.Run(() => WorkThread());
注意：虽然可以通过代码强行结束一个任务，但强烈建议不要这样做，应该给它一个通知让其自己结束。

## ThreadPool

## 并行编程Parallel
Parallel实际是通过线程池进行任务的分配，线程池的最小线程数和最大线程数将影响到整个程序的性能，需要合理设置。（最小线程默认为8。）  
````C#
Parallel.For(1, 10000, x=> 
           {
                bool b = IsPrimeNumber(x);              
                Console.WriteLine($"{i}:{b}");
            });

````

## async await
可以简单的实现异步编程，程序判断await的位置，然后根据生成异步线程处理await后的部分，返回一个task，可以获取这个result值。然后回到主线程

## 参考链接
https://www.cnblogs.com/seabluescn/p/12973936.html
https://www.cnblogs.com/forever-Ys/p/11675953.html
https://www.cnblogs.com/zhaoshujie/p/11192036.html