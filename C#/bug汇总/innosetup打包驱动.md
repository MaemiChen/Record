## 说明
主要的方法有以下三种  
- 在innosteup中直接安装驱动
- 使用批处理安装驱动

## 一：直接在innosetup中安装驱动
未成功验证，可以参考以下的blog
https://blog.csdn.net/chenlu5201314/article/details/54943946

## 二：使用批处理安装
### 1. 批处理文件
```bat
@echo off
if "%1"=="hide" goto begin
start mshta vbscript:createobject("wscript.shell").run("""%~0"" hide",0)(window.close)&&exit
:begin 

if exist %SystemRoot%\System32\pnputil.exe (
    set "SystemPath=%SystemRoot%\System32"
) else if exist %SystemRoot%\Sysnative\pnputil.exe (
    set "SystemPath=%SystemRoot%\Sysnative"
) else (
    echo ERROR: Cannot find pnputil.exe to install the driver.
    echo/
    pause
    goto :EOF
)
%SystemPath%\pnputil.exe -i -a "%~dp0\CH341SER.inf"
```

以下代码表示隐藏cmd弹出指令
```bat
if "%1"=="hide" goto begin
start mshta vbscript:createobject("wscript.shell").run("""%~0"" hide",0)(window.close)&&exit
:begin 
```
以下代码表示获取pnputil的路径
```bat
if exist %SystemRoot%\System32\pnputil.exe (
    set "SystemPath=%SystemRoot%\System32"
) else if exist %SystemRoot%\Sysnative\pnputil.exe (
    set "SystemPath=%SystemRoot%\Sysnative"
) else (
    echo ERROR: Cannot find pnputil.exe to install the driver.
    echo/
    pause
    goto :EOF
)
```
以下代码表示执行驱动安装
```bat
%SystemPath%\pnputil.exe -i -a "%~dp0\CH341SER.inf"
```

### 2. innosetup设置
由于pnputil.exe需要用管理员权限运行才能成功安装驱动，所以innosetup中需要获取管理员权限

```iss
[Setup]
PrivilegesRequired=admin

[Run]
Filename: "{app}\CH341SER\DriverCH341.bat";
```

使用流程如下：
1. 将“CH341SER”文件夹放到需要打包的exe文件同一个目录下。
2. 打包的应用需要以管理员权限运行。在“[Setup]”中添加“PrivilegesRequired=admin”。
若innosetup以前未设置过管理员权限，则参考如下：
找到Inno Setup安装目录下的SetupLdr.e32文件，使用Resource Hacker软件打开，将Manifest中requestedPrivileges的改成
<requestedExecutionLevel level="requireAdministrator"            uiAccess="false"/>
3. 在“[Run]”中添加bat文件执行。如：
Filename: "{app}\CH341SER\DriverCH341.bat";

### 3. 尝试错误说明
1. 使用bat2exe  
可以成功的将bat文件转换为exe文件，但是不能解决当开启pnputil.exe cmd powershell等的弹框。
2. 使用vbs文件实现隐藏  
可以成功编译但是无法执行批处理文件。考虑是否是因为没有管理员权限等原因。而且innosetup中无法运行vbs文件。  
[vbs具有管理员权限](https://blog.csdn.net/zw05011/article/details/113386320)
```vbs
Set ws = CreateObject("Wscript.Shell")
ws.run "cmd /c 批处理程序名",vbhide
```

