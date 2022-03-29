## 1. 简述

### 1. CIL JIT
1. 
    在编译.netframework库文件时，不立即创建作用于操作系统的代码，而是把代码编译为<font color=yellow>通用中间语言(Common intermedite language) (Microsoft intermediate language同名)</font>,这些代码并非专用于某一种操作系统，也不是某种语言。基于.net的程序都生成为CIL代码

2. 
    JIT(just in time)将CIL代码转换为专用于OS和目标机器的代码，CIL代码仅在需要时才编译
### 2. 自动垃圾回收机制

### 3. 运行组合
    C#代码 => （链接）CIL程序集 =>(当执行代码时编译JIT)本机代码
