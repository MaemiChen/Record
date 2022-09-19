## 说明
winform和wpf的线程模型非常接近，都是最后调用API（GetMessage或PeekMessage）来处理其他线程发送过来的消息，这些消息存储在系统的一个消息队列中。   
- winform框架中有一个`ISynchroizeInvoke`接口，所有的UI元素（Control）都继承自该接口。其中接口`InvokeRequired`属性表示当前线程是否是创建它的线程。接口中的Invoke和BeginInvoke方法负责将消息发送到消息队列中。这样UI线程就能够正确处理它了。
