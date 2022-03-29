## std::chrono 

- duration 表示时间间隔
    - hour
    - minutes
    - second
    - millisecond
    - microsecond
    - nanosecond

函数count()表示指定时间间隔对象中包含多少个时间周期  
- clock
    - system_clock 系统内时钟 当修改系统时钟时可能会改变其单调递增的性质  
    now()、to_time_t()、from_time_t()
    - steady_clock 单调时钟，一旦启动就与系统时间无关，根据物理时间向前移动  now()

- time_point 表示具体的时间点
https://blog.csdn.net/qfturauyls/article/details/107051902
https://blog.csdn.net/albertsh/article/details/105468687  
https://www.cnblogs.com/jwk000/p/3560086.html

## csignal
https://blog.csdn.net/sysleo/article/details/95984946

## cstdarg
https://www.cnblogs.com/cyssmile/p/13819754.html

## cstdio
### c库操作输入输出  
这个库使用流（stream）来操作物理设备，如：键盘、打印机、终端或其他所有支持的文件。流是所有与它交互的统一抽象。所有的流具有相似的属性独立于它们所关联的硬件的特征。  

在此库中stream是一个handle作为文件的指针。指向FILE对象指针唯一的标识一个流，并在该流操作中作为参数。  

这里有3个标准的stream,它们可以自动地打开所有使用它地程序
- stdin 
- stdout
- stderr

### 流的属性
流具有一些属性，这些属性定义了哪些函数可以用于流，以及这些函数将如何处理通过流输入或输出的数据。这些属性中的大多数都是在使用fopen函数将流与文件(打开)关联时定义的:
- Read/Write Access  
指定流是否对它们关联的物理媒体具有读或写访问权限(或两者都具有)。
- Text / Binary  
文本流被认为表示一组文本行，每一行都以换行字符结尾。根据应用程序运行的环境，可能会对文本流进行一些字符转换，以便某些特殊字符适应环境的文本文件规范。另一方面，二进制流是从物理介质中写入或读取的字符序列，无需转换，与流中读取或写入的字符一一对应。
- buffer  
缓冲区是在对相关文件或设备进行物理读或写入之前积累数据的一块内存。流可以是完全缓冲、行缓冲、无缓冲的。  
在完全缓冲流上，当缓冲区被填满时，数据被读写。在行缓冲流中，当遇到换行字符时，就会发生读写。在未缓冲流中，字符将被尽快的读取/写入。
- orientation  
在打开过程中，流是没有方向的。一旦对它们进行输入输出操作，它就会变成 byte-oriented 或 wide-oriented，取决于所采取的操作（总的来说，在\<cstdio>中是byte-oriented，在\<cwchar>中式 wide-oriented）
- indicators  
流有特定的内部指示器来指定它们当前状态，并影响它们执行的一些输入输出操作的行为。
    - error indicator
    - end-of-file indicator
    - positive indicator

### funcation
- tmpnam 生成临时文件名
- tmpfile 生成临时文件
- fflush 清空文件缓冲区
- setbuf 将缓冲区和流相关联，实现缓冲区直接操作文件流的功能 https://blog.csdn.net/fjt19900921/article/details/8122313
````C++
//将缓冲区和流关联
char outbuf[BUFSIZ];
setbuf(stdout, outbuf);	//将outbuf和输出流相连
puts("ssddafafw wdw");	//输出一个字符串
puts(outbuf);	//因为缓冲区和输出相连，所以outbuf等于输出的字符串
fflush(stdout);//刷新
puts(outbuf);	//由于puts输出两次，所以输出两个字符串
````
- setvbuf  将缓冲区和流关联

## cwchar


