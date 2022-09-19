## .net关键字说明
<style>
table th:first-of-type {
    width: 20%;
}
table th:nth-of-type(2) {
    width: 10%;
}
table th:nth-of-type(3) {
    width: 50%;
}
</style>
| 关键字 | 默认值  | 说明   |
| - | - | - |
| Application Name | N/A | 应用程序名称，或者".Net SqlClient Data Provider"（如果不提供应用程序名称）|
| Asynch | false | 如果设置为true，启用异步操作支持|
| AttachedDBFilename/ extended properties/ initial File Name | N/A | 主数据库文件的名称，包括可连接数据库的完整路径名|
| Connect TimeOut/ Connection TimeOut | 15s | 一个到服务的连接在终止之前等待的时间长度 |
| Connection LifeTime |0 | 当一个连接被返回到连接池时，它的创建时间会与当前时间进行比对。如果这个时间跨度超过了连接的有效期的话，连接被取消。|
| Connection Reset | true | 表示一个连接在从连接池中被移除时是否被重置。一个伪的有效在获得一个连接的时候就无需再进行一个额外的服务器来回运作。|
| Current Language | N/A | SQL Server语言记录的名称 |
| Data Source(数据源) /Server(服务器) /Address(地址) /Network Address(网络地址)|N/A|SQL实例的名称或网络地址|
| Encrypt | | 当值为真时，如果服务器安装了授权整数，SQL Server会对客户和服务器之间传输的数据使用SSL加密|
| Enlist（登记）| true | 表示连接池程序是否会自动登记创建线程的当前事务语境中的连接|
| Database / Initial Catalog | | 数据库名称|
| Integrated Sercurity(集成安全) / Trusted Connection(受信连接) | false | 表示Windows认证是否被用来连接数据库|
| Max Pool Size （连接池的最大容量）| 100 | 连接池允许连接的最大值 |
| Min Pool Size （连接池的最小容量）| 0 | 连接池允许连接的最小值 |
| NetWork Library/Net | | 用来建立到一个SQL Server实例的连接的网络库 |
| Packet Size(数据包大小) | 8192 (0x2000) | 用来和数据库通信的网络数据包的大小|
| Password | | 与账户名对应的密码|
| Persist Sercurity Info| false | 用来确定一旦连接建立了以后安全信息是否可用。如果为真，说明像用户名和密码这样对安全性比较敏感的数据可用。为伪则不可用|
| Pooling | true| 确定是否使用连接池。如果值为真，连接就要从适当的连接池中获得|
| User ID | | 用来登录数据库的账户名|
| Workstation ID| 本地计算机的名称| 连接到SQL Server的工作站的名称|

## 连接池
连接到数据源可能需要很长时间。为了最大程度降低打开连接的成本，ADO.Net使用一种“连接池”的优化技术，可用最大程度降低重复打开和关闭连接造成的成本。 .NET Framework 数据提供程序处理连接池的方式有所不同。

## 参考链接
https://blog.csdn.net/weixin_30613727/article/details/96949515  
[.Net参考](https://blog.csdn.net/velly/article/details/6692037)