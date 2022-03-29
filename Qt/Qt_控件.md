# QT的基本开发总结
## Segmentation fault
1. **段错误：访问内存超出系统所给的这个程序的内存空间。** 这个值是由gdtr来保存的，它是一个48位寄存器，其中32位保存指向它的gdt表，后13位保存gdt下标，最后3位包括了程序是否在内存中及程序在CPU中的运行级别
2. SIGSEGV产生的可能情况(涉及到操作系统、C库、编译器、链接器等)
 > 1. 错误的访问类型
 > 2. 访问了不属于进程地址空间的内存
 > 3. 访问了不存在的内存
 > 4. 内存越界、数组越界、变量类型不一致等
 > 5. 试图把一个整数按照字符串方式输出

2. 结构体和QByteArray的互相转换  
https://blog.csdn.net/weixin_43450564/article/details/103864783
 writeDatagram函数原型，发送成功返回字节数，否则-1

3. 不同操作系统下指针的内存大小  
https://blog.csdn.net/cloud_510/article/details/52734787  
32位存放指针大小为4byte(32bit)  
64位存放指针大小为8byte(64bit)

4. 设置关闭窗口时自动删除该对象
qwidget->setAttribute(Qt::WA_DeleteOnClose);

5. QSerialPort 成员函数
大多数读写函数继承自QIODevice

6. char byteArray
    1. char 该类型变量只能容纳一个字符，在大多数操作系统上，只使用一个字节的内存。  
        一个字节8位，对应两位16进制数。

        ASCII码是由0x00~0xFF的16进制数表示
    2. ByteArray  
        ![](./QT/QByteArray.png)  
        指针指向的是一个对象，不是6个对象，所以不能使用[]  
        现在是指针用[]，取到的是一个对象。对这个对象直接赋值，就需要看这个对象有没有实现对应的operater=了。即=的操作运算符重载。  
        然后你直接对对象用[]，取到的是这个bytearray的第几个元素，对这个元素赋值当然就可以。

7. 运算符重载
    operator=  
    使用拷贝构造函数时,如果构造函数中存在new的成分，需要delete原来的，重新new，避免发生浅拷贝的现象。  
    ````C++
    Mystr str1(1, "hhh");
    Mystr str2 = str1;
   ````

8. 事件循环实时
    QCoreApplication::processEvent()

9. IP地址和端口号
    自己的端口号在0~65535范围内，自己随便定义，但需要避开特殊占用的端口号，IP地址自己可以IPconfig获取，或者使用hostName()

10. 图形转换
    QTransform用于指定坐标系的2D转换-平移、缩放、扭曲、剪切、旋转、投影坐标系

11. notify eventFilter  
    QCoreApplication::notify() 捕获该程序所有事件  
    QCoreApplication::eventFilter() 事件过滤器，定义事件处理动作，筛选事件不起作用  
    https://www.cnblogs.com/wenhao-Web/p/12292340.html

12. QSerialPort
    串口的序列号是唯一的，不因为插在不同的USB口而改变

13. TCPsocket通信  
关于通信的具体使用，以及示波器的连接使用原理  
TCP socket有客户端和服务器端区分。服务器端通过监听某个端口是否有客户端连接到来，如果有，则建立新的socket连接。客户端通过IP和port连接服务器端，成功建立连接后就可以进行数据的收发了。    
a) 服务器端机制  
    通过listen建立对端口的监听m_tcpSocket->listen(QHostAddress::Any,port);  
    QHostAddress定义集中特殊IP地址  
    QHostAddress::Null 空地址  
    QHostAddress::LocalHost IPv4本机地址127.0.0.1  
    QHostAddress::LocalHostIPv6 IPv6本机地址  
    QHostAddress::Broadcast 广播地址255.255.255.255  
    QHostAddress::Any 任意IPv4地址  
    QHostAddress::AnyIPv6 任意IPv6地址  

    当信号new:newConnection()触发时，判断为接收到新的连接，并保存该连接  
    connect(m_TcpServe,SIGNAL(newConnect()),this,SLOT(ServerNewConnection()));  
    m_TcpServer = m_TcpServer->nextPendingConnection();  
    接收数据，发送数据  
    connect(m_TcpServer,SIGNAL(readyRead()),this,SLOT(ServerReadData()));  
    m_TcpServer->read(buffer,1024);  
    m_TcpServer->write(msg,strlen(msg));  
b) 客户端机制  
    通过m_TcpClient->connectToHost(ip,port);连接服务器端  
    接收数据和发送数据也是使用read和write

13. 统计代码行数  
    find  ./* -regex ".*\.h\|.*\.cpp" | xargs -I {} cat {} | wc -l




## 参考资料
1. [segmentation fault](https://blog.csdn.net/u010150046/article/details/77775114?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)  
2. [operator=](https://www.cnblogs.com/zpcdbky/p/5027481.html)
3. [postEvent](https://www.cnblogs.com/wanzaiyimeng/p/4609488.html)