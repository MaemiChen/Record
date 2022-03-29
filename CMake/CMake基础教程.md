## 1. message 
为用户显示一条消息  
可选的关键字类型
- （无） = 重要消息
- STATUS = 非重要消息
- WARNING = CMake警告，会继续执行
- AUTHOR_WARNING = CMake警告（dev），会继续执行
- SEND_ERROR = CMake错误，继续执行但会跳过生成的步骤
- FAIL_ERROR = CMake错误，中止所有处理过程
https://blog.csdn.net/Zhanganliu/article/details/99850603

## 2. config_file
将文件复制到另一个位置并修改其内容。
````C++
configure_file(<input> <output>
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])

````

https://blog.csdn.net/qq_38410730/article/details/103741579

## 3. include
include指令一般用于语句的复用。也就是说如果有一些语句需要再很多的CMakeLists.txt中使用，为避免重读编写，可以将其写在.cmake文件中，然后再需要的CMakeLists.txt中进行include操作。  

为了使CMakeLists.txt找到该文件，需要指定文件的完整路径。如果指定了CMAKE_MODULE_PATH，就可以直接include该目录下的.cmake文件了。  

.cmake文件中包含了一些cmake命令和一些宏/函数，当CMakeLists.txt包含.cmake文件时，当编译运行时，该.cmake里的一些命令会在包含处得到执行，并再包含以后的地方能够调用该.cmake里的一些宏和函数。

## 4. macro function 
````C++
set(var "ABC")
set(ss "ddd")
# 这是一个宏定义（字符串替换）
macro(Mro arg argc)
message("arg = ${arg}")
set(arg "abc")
message("arg = ${arg}")
message("argc = ${argc}")
endmacro(Mro)

# 这是一个函数（使用变量，在命令过程中可以对变量进行修改）
function(Fun arg argc)
message("arg = ${arg}")
set(arg "abc")
message("arg = ${arg}")
message("argc = ${argc}")
endfunction(Fun arg argc)


message("== call macro ==")
MRO(${var} ${var})
message("== call funcation ==")
FUN(${var} ${ss})
````
<font color=yellow>无论是宏还是函数，当调用的时候如果使用的是set出来的变量，都必须通过${}将变量的内容传递进去，而不能只写上变量名。</font>

## 5. add_subdirectory
编译子文件夹中的CMakeLists.txt

## 6. add_executable
将文件声明可执行文件

## 7. add_custom_target add_custom_target
https://yngzmiao.blog.csdn.net/article/details/102797448

## 参考链接
1. https://zhuanlan.zhihu.com/p/119426899
2. https://github.com/ttroy50/cmake-examples (这个是一个cmake的详细教程)