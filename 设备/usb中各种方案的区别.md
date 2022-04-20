# usb类
![](https://upload-images.jianshu.io/upload_images/15877540-1f10023d615d8a62.png?imageMogr2/auto-orient/strip|imageView2/2/w/492/format/webp)
![](https://ask.qcloudimg.com/http-save/yehe-2977066/njk6zsnjce.png?imageView2/2/w/1620)
## cdc
communication device class，CDC是通信设备级方案，是usb转其他接口的一类设备，如usb转RS232，usb转Ethernet
## dfu
device firmware upgrade
## hid
humman interface device 人机接口级方案，多为不需要驱动的键盘鼠标
## msc
mass storage class 大容量存储方案，多为移动存储设备

# usb基本
- 管道pipe  
    是主机与设备端点设备传输的连接通道，代表主机数据缓冲区与设备端点之间交换数据的能力。管道包括数据流管道和消息管道。
- 端点EndPoint  
    设备硬件上具有一定大小的数据缓冲区。usb设备中，每一个端点都有唯一的地址，是由设备地址和端点号给出的。默认设置端点0作为控制传输端点，其他端点必须在设备被主机配置后才能使用。

## USB数据通讯结构
usb数据是由二进制数字串构成的，首先数字串构成`域`（7种），域再构成`包`（令牌包、数据包、握手包），包再构成`事务`（In，Out，Setup），事务最后构成`传输`（中断传输，同步传输，批量传输，控制传输）
- 域 usb数据最小单位  
- 包 由域构成的包有四种类型，分别为令牌包、数据包、握手包和特殊包
- 事务 有in、out、setup三大事务，每一个事务都由令牌包、数据包、握手包三个阶段构成。
- 传输
    - 中断传输：由out事务和in事务构成，用于键盘鼠标等HID设备的数据传输种
    - 批量传输：由out事务和in事务构成，用于大容量数据传输，也不占用调款，当总线忙时，usb优先进行其他类型的数据传输，暂停批量传输
    - 同步传输：由OUT事务和IN事务构成，有两个特殊地方，第一，在同步传输的IN和OUT事务中是没有握手包阶段的；第二，在数据包阶段所有的数据包都为DATA0
    - 控制传输：最重要的也是最复杂的传输，控制传输由三个阶段构成（初始设置阶段、可选数据阶段、状态信息步骤），每一个阶段可以看成一个的传输，也就是说控制传输其实是由三个传输构成的，用来于USB设备初次加接到主机之后，主机通过控制传输来交换信息，设置地址和读取设备的描述符，使得主机识别设备，并安装相应的驱动程序，这是每一个USB开发者都要关心的问题。



https://www.armbbs.cn/forum.php?mod=viewthread&tid=100687 （usb基础）
https://blog.csdn.net/qq_34234087/article/details/86713907