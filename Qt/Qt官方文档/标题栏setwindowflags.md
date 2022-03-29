## 1. Qt::windowsflag
主要的类型如下表  
| constant | value | 描述 |
| --- | --- | --- |
| widget | 0x00000000 | QWidget的默认类型，titleBar有最大化最小化 |
| window | 0x00000001 | 表面widget是一个window，通常带有系统框架和标题bar，不考虑widget是否有parent。如果widget没有父窗口则不可能不设置这个flag|
| dialog | 0x00000002 | 表明这个widget是一个Window装饰为一个dialog，没有最大最小化，它可以设置为模态化或者非模态化[modal](./扩展1-模块化)|
| ... | ... | ... |
| MSWindowsFixedSizeDialogHint | 0x00000100 | 给window一个固定大小的dialog |
| FramelessWindowHint | 0x00000800 | 无边框的window，且cannot move/resize



## 扩展1-模态modal
模态对话框modal dialog和非模态对话框在各个平台都存在。模态对话框指其没有被关闭前，用户不能与同一个应用程序的其他窗口进行交互，知道该对话框关闭。  
Qt显示对话框的方式有两种
1. exec()   总是以模态来显示对话框
2. show()   通过setModal来决定它是模态的还是非模态的，默认为非模态的

## 参考链接
https://blog.csdn.net/xuebing1995/article/details/96478891