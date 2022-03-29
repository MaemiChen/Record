## winsock 和winsock2区别
<socket.h>用于linux操作系统，但是<winsock.h>和<winsock2.h>除了版本不同外，还有什么不同呢?

windows.h在新的Windows版本下编译时会包含winsock2.h，而在老版本中包含winsock.h，但是问题不仅局限在windows.h。当winsock.h在winsock2.h前包含时，编译会报错，因为两个文件不能共存的很好。winsock2.h设计的目的是替代winsock.h，而不是扩展它。在winsock.h中定义的所有内容在winsock2.h中也都定义了。如果winsock2.h在winsock.h前包含，winsock2.h定义了_WINSOCKAPI_，阻止编译器去处理后面的winsock.h，编译不会报错。但是如果winsock.h在winsock2.h前包含，winsock2.h没有检测到这些，而去重新定义winsock.h已经定义的东西，从而编译报错。

在同一个项目中同时使用winsock.h和winsock2.h要格外小心。例如，当使用winsock2.h编写自己的代码，但是使用仍包含winsock.h的第三方库的时候。

## winsock 入门
https://blog.csdn.net/Explorer_day/article/details/83088488


https://blog.csdn.net/weixin_30629653/article/details/95182841?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164802647316780261968131%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164802647316780261968131&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-95182841.142^v3^pc_search_result_control_group,143^v4^control&utm_term=winsock2.h&spm=1018.2226.3001.4187



