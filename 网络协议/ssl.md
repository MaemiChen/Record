## HTTP和HTTPS
HTTPS由HTTP进行通信，但利用SSL/TLS来加密数据包。HTTPS主要目的是，提供对网站服务器的身份认证，保护交换数据的隐私和完整性
- HTTP 默认工作在TCP 80端口
- HTTPS 默认工作在TCP 443端口
- 使用HTTPS协议需要用到CA（Certificate Authority数字证书认证机构）申请证书。
- HTTP页面响应比HTTPS快，因为HTTPS除了TCP连接的三个包还包括ssl握手需要9个包，总共需要12个包来创建连接。

## 1. SSL简述
ssl 安全套接字(secure socket layer),是web浏览器和web服务器之间安全交换信息的协议，提供两个基本的服务：鉴别和保密   

TLS(Transport Layer Security 安全传输协议)用于两个通信应用程序之间提供保密性和数据完整性。和SSLv3类似。

### 1. ssl的三个特性
1. 保密：在握手协议中定义会话秘钥后，所有消息都将加密
2. 鉴别：可选的客户端认证，强制的服务器端认证
3. 传送的消息包括消息完整性检查（使用MAC）

### 2. SSL的位置
ssl位于http（应用层）和tcp（传输层）之间。应用层数据不直接传递给传输层，先传递给ssl进行加密，增加ssl头


## 2. ssl的工作原理

### 1. 握手协议(handshake protocol)

https://blog.csdn.net/qq_38265137/article/details/90112705