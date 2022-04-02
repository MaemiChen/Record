## 窗口
- glad　　

GLAD是用来管理OpenGL的指针的，所以在调用前必须初始化GLAD
````C++
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
{
    std::cout << "fail initialize GLAD" << std::endl;
    return -1;
}
//给glad传入了用来加载系统相关的OpenGL函数指针地址的函数
````
- 双缓冲（double buffer）  
应用程序使用单缓冲绘图时可能会存在图像闪烁的问题。 这是因为生成的图像不是一下子被绘制出来的，而是按照从左到右，由上而下逐像素地绘制而成的。最终图像不是在瞬间显示给用户，而是通过一步一步生成的，这会导致渲染的结果很不真实。为了规避这些问题，我们应用双缓冲渲染窗口应用程序。前缓冲保存着最终输出的图像，它会在屏幕上显示；而所有的的渲染指令都会在后缓冲上绘制。当所有的渲染指令执行完毕后，我们交换(Swap)前缓冲和后缓冲，这样图像就立即呈显出来，之前提到的不真实感就消除了。

## 坐标空间
在OpenGL中，任何事物都在3D空间下，但屏幕和窗口是2D像素数组，导致OpenGL的大部分工作是把3D坐标转变为适应屏幕的2D像素。3D坐标转换为2D坐标的处理过程是由OpenGL的图形渲染管线（Graphic Pipeline，实际上是一组原始图形数据途径一个输送管道，期间经过各种变化最终出现在屏幕的过程）。

https://learnopengl-cn.github.io/