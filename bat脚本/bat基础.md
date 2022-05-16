## 说明
bat脚本就是DOS批处理脚本，就是将一些DOS命令按照一定顺序排列而形成的集合，运行在windows命令行环境上。

### 常用系统变量
|||
| - | -|
| %CD%|获取当前目录|
| %PATH% | 获取命令搜索目录 |
| %DATE% | 获取当前日期|
| %RANDOM% | 获取0~32767之间的任意十进制数字|
| %ERRORLEVEL% | 获取上一命令执行结果码 |

其中%CD% 和%~dp0 都能表示当前目录，但是在不太使用场景下功能不同
- %CD% 表示当前工作目录（current working directory），为`变量`
- %~dp0 表示当前批处理文件所在完整目录（the batch file's directory），为`常量`

1. 变量读取  
` echo %SystemRoot%`
2. 变量设置  
使用set命令显示、设置、删除cmd环境变量。可以使用“set /?”或“help set”查看  
`SET [variable=[string]] #variable表示变量名，string表示变量值。`
 

## 参考文件
https://www.cnblogs.com/linyfeng/p/8072002.html