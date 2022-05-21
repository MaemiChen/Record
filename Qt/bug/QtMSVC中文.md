## 说明
Qt Creator中显示的汉字正常，但编译的时候会出现“常量中有换行符”等一系列错误报警。其实，这也是文字编码的问题。如下图所示：
![](https://img-blog.csdnimg.cn/20181211205947935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXdlaWxoeQ==,size_16,color_FFFFFF,t_70)

## 原因
MSVC在编译时，会根据源代码文件有无BOM来定义源码字符集。如有BOM则按照BOM解释识别编码，如果没有，则使用本地字符集，对于简体中文来说就是GB2312。`当MSVC遇到一个没有BOM的UTF-8编码的文件时，它通常会把文件看作GB2312的来处理`

## MSVC
- MSVC 的编码字符集和执行字符集都默认为 GBK；而 QtCretor 编码字符集和执行字符集都默认为 UTF-8。
- MSVC 执行编译时候，发现近仅只能够将 “UFT-8 带 BOM” 格式的文件正确识别。而不带 BOM 的 UTF-8 实际也是被识别为 GBK 格式。
- MSVC 在编译时，无论 .cpp 文件源码字符集是 UTF-8、UTF-8 BOM、GBK 中的哪一种；只要没有声明为执行码字符集为 UFT-8，则最终在内存中，都会被强制转换 GBK 处理；若一旦声明，则也会被强制转换为 UTF-8 格式 。
```c++
// 若想声明执行字符集为 UFT-8，添加如下
#pragma execution_character_set("utf-8")
```
- 若测试的字符串 "xxxxxxx" 为偶数，恰好会有“错错得对”巧合


https://blog.csdn.net/liuweilhy/article/details/82321105

https://xmuli.tech/posts/c0862e62/#:~:text=MSVC%20%E7%9A%84%E7%BC%96%E7%A0%81%E5%AD%97%E7%AC%A6%E9%9B%86%E5%92%8C%E6%89%A7%E8%A1%8C%E5%AD%97%E7%AC%A6%E9%9B%86%E9%83%BD%E9%BB%98%E8%AE%A4%E4%B8%BA%20GBK%EF%BC%9B%E8%80%8C%20QtCretor%20%E7%BC%96%E7%A0%81%E5%AD%97%E7%AC%A6%E9%9B%86%E5%92%8C%E6%89%A7%E8%A1%8C%E5%AD%97%E7%AC%A6%E9%9B%86%E9%83%BD%E9%BB%98%E8%AE%A4%E4%B8%BA%20UTF-8%E3%80%82%20MSVC%20%E6%89%A7%E8%A1%8C%E7%BC%96%E8%AF%91%E6%97%B6%E5%80%99%EF%BC%8C%E5%8F%91%E7%8E%B0%E8%BF%91%E4%BB%85%E5%8F%AA%E8%83%BD%E5%A4%9F%E5%B0%86,%E6%A0%BC%E5%BC%8F%E7%9A%84%E6%96%87%E4%BB%B6%E6%AD%A3%E7%A1%AE%E8%AF%86%E5%88%AB%E3%80%82%20%E8%80%8C%E4%B8%8D%E5%B8%A6%20BOM%20%E7%9A%84%20UTF-8%20%E5%AE%9E%E9%99%85%E4%B9%9F%E6%98%AF%E8%A2%AB%E8%AF%86%E5%88%AB%E4%B8%BA%20GBK%20%E6%A0%BC%E5%BC%8F%E3%80%82