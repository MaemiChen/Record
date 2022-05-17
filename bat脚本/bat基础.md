## 说明
bat脚本就是DOS批处理脚本，就是将一些DOS命令按照一定顺序排列而形成的集合，运行在windows命令行环境上。

## 常用系统变量
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

## 字符串基本操作

1. 命令 `echo %var:~n,k% ` 其中"%var"，表示待截取字符的字符串，"~"取字符标志符，"n”表示字符截取起始位置，"k" 表示截取结束位置  
2. 字符串替换 `%var:old_str=new_str%`

## DOS基本命令
1. rem注释符，可以使用（::）替换
2. echo 显示信息，或启动或关闭命令回显。@字符放在命令前将关闭该命令回显，无论此时echo是否为打开状态。
3. pause 暂停并输出"请按任意键继续. . ."   
pause > nul 等待但不出现提示语  
echo wait a moment.. & pause > nul 输出指定输出语"wait a moment.."并等待操作
4. errorlevel 程序执行结果返回码，执行成功返回0，失败返回1
5. start 启动一个单独的窗口以运行指定的程序或命令，程序继续向下执行。
6. exit 退出cmd.exe 或当前批处理脚本
7. cls 清除屏幕内容
8. help 提供Windows命令的帮助信息

### 1. start

语法：`start ["title"] [/dPath] [/i] [/min] [/max] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/wait] [/b] [FileName] [parameters]`
- "title" 指定在“命令提示符”窗口标题栏中显示的标题。
- /dPath 指定启动目录。 start /d "C:\Windows\System32" /max cmd.exe
- /i 将 Cmd.exe 启动环境传送到新的“命令提示符”窗口。
- /min 启动新的最小化窗口。 
- /max 启动新的最大化窗口。
- /separate 在单独的内存空间启动 16 位程序。 
- /shared 在共享的内存空间启动 16 位程序。
- /low 以空闲优先级启动应用程序。 
/normal 以一般优先级启动应用程序。 
/high 以高优先级启动应用程序。 
/realtime 以实时优先级启动应用程序。 
/abovenormal 以超出常规优先级的方式启动应用程序。 
/belownormal 以低出常规优先级的方式启动应用程序。 
- /wait 启动应用程序，并等待其结束。
- /b 启动应用程序时不必打开新的“命令提示符”窗口。除非应用程序启用 CTRL+C，否则将忽略 CTRL+C 操作。使用 CTRL+BREAK 中断应用程序。

## 文件操作命令
1. {copy | copy /y (有相同文件时不提示)} 文件复制命令
2. {xcopy | xcopy /y (有相同文件时不提示)} 目录复制命令
3. type 显示文件内容命令
4. ren 重命名文件命令
5. del 删除文件命令
## 目录操作
1. cd
2. mkdir
3. rmdir
4. dir 显示目录下的子目录和文件
> /b表示去除摘要信息，仅显示完整路径；/s 表示循环列举文件夹中内容；/o:n表示根据文件名排序；/a:a表示只枚举文件而不枚举其他。

## for 语句
- /d 打印目录名
```bat
@echo off
for /d %%i in (c:/*) do (
  echo %%i
)
pause
```
- /r 递归查询指定目录下的匹配文件。默认使用当前目录。
```bat
@echo off
for /r d:/temp %%i in ( *.txt *.py ) do (
  echo %%i
)
pause
```
- /l 该集表示以增量形式从开始到结束的一个数字序列
```bat
@echo off
for /l %%i in (1,2,10) do (
  echo %%i
)
pause
```

## 如何实现自动交互
使用重定向  
利用重定向方式可以实现自动交互输入。假设需要交互的脚本为A.exe，脚本需要依次输入12、13、15。则可以采用如下形式：
```bat
del c.txt
echo 12 > c.txt
echo 13 >> c.txt
echo 15 >> c.txt
A.exe < c.txt
```

## 参考文件
https://www.cnblogs.com/linyfeng/p/8072002.html