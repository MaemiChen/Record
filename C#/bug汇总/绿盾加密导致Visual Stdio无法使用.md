## 前言
背景如下：公司要求安装的绿盾，我使用的是Visual Stdio 2019，在Installer中新增添加了一部分内容后，发现VS出现bug。

>“StreamJsonRpc.RemoteInvocationException: 未能加载文件或程序集“file:///E:\Program Files (x86)\Microsoft Visual Studio\2019\Professional\Common7\ServiceHub\Hosts\ServiceHub.Host.CLR.AnyCPU\Microsoft.ServiceHub.HostStub.dll”或它的某一个依赖项。异常来自 HRESULT:0xC00CE508 (ErrorKind: Error HResult: 80131500)
   在 StreamJsonRpc.JsonRpc.<InvokeCoreAsync>d__139`1.MoveNext()...”

现象是可以正常运行程序但是无法进行调试。这个时候开始怀疑是重要的配置被加密了，找到报错的路径位置，发现报错的地方并没有被加密。最后发现是devenv.exe.config被加密了。   

注意：这里的devenv.exe.config存在多个,这咯被加密的是在这个位置下的C:\Users\xxx\AppData\Local\Microsoft\VisualStudio\16.0_32de61ed。最好将所有的devenv.exe.config都进行解密一遍防止错误。