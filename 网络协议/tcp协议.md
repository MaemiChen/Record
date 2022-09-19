## TCP (Transmission Control Protocol)
面向连接的，可靠的，基于字节流的传输层协议

特点
- 基于连接的：数据传输之前需要建立连接。
- 全双工的：可以双向传输。
- 字节流：不限制字节大小，打包为报文段，保证有序接受，重复报文自动丢弃
- 流量缓冲：解决双方处理能力的不匹配
- 可靠的传输服务：保证可达，丢包时通过重发机制实现可靠性
- 拥塞控制：防止网络出现恶意拥堵

## TCP报文格式
![](https://pic.rmb.bdstatic.com/bjh/down/050522d590b97179332c1241826f57cb.png@wm_2,t_55m+5a625Y+3L+e8lueoi+mYtue6p0lJSQ==,fc_ffffff,ff_U2ltSGVp,sz_17,x_11,y_11)
- source port 源端口号
- dest port 目的端口号
- sequence number 序列号
- acknowledgement number 应答序列号
- receive window 当前服务器可以接受的数据窗口大小的值

- CWR ECE 用于解决拥塞控制的
- URG PSH 发送紧急数据包（不能缓存，立即发送）
- ACK RST SYN　FIN　用来进行连接控制
- hearder length 表示TCP头部长度

`为什么需要三次握手`  
为了避免重复连接。  
原因是服务器发送应答包的时候，客户端可能还没收到。导致客户端超时重发请求连接包。