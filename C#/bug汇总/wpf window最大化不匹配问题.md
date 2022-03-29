## 1. 错误点

wpf window软件最大化时出现ActualWidth、ActualHeight不等于屏幕大小。出现原因不明。  

xmal设置
````C#
<Window x:Class="Tdcs.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Tdcs" 
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        mc:Ignorable="d" ResizeMode="CanResizeWithGrip" d:DesignWidth ="1920" d:DesignHeight="1040" 
        WindowState="Maximized"   Background="Transparent" 
        WindowStartupLocation="CenterScreen" FontFamily="Microsoft YaHei UI" Name="mainWindow"  BorderThickness="1,0,1,0" BorderBrush="Gray" WindowStyle="None",AllowsTransparency="True"
        Title="MainWindow">
    <Grid Name="gdMain">
        <Button Click=Btn_normal_Click>
    </Grid>
    
</Window>

````
cs使用
````C#
private void Btn_normal_Click(object sender, RoutedEventArgs e)
        {

            Console.WriteLine($"width:{this.ActualHeight}");
            Console.WriteLine($"height:{this.ActualWidth}");
            
            if (this.WindowState == WindowState.Normal)
            {

                this.WindowState = WindowState.Maximized;
            }
            else if (this.WindowState == WindowState.Maximized)
            {
                this.WindowState = WindowState.Normal;
            }
        }
````

## 2. 解决措施
1. 删除 WindowStyle="None",AllowsTransparency="True"
2. 修改为  
xaml
````C#
<Window x:Class="Tdcs.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Tdcs" 
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        mc:Ignorable="d" ResizeMode="CanResizeWithGrip" d:DesignWidth ="1920" d:DesignHeight="1040" 
        WindowState="Maximized"   Background="Transparent" 
        WindowStartupLocation="CenterScreen" FontFamily="Microsoft YaHei UI" Name="mainWindow"  BorderThickness="1,0,1,0" BorderBrush="Gray" 
        Title="MainWindow">

    <WindowChrome.WindowChrome>
        <WindowChrome />
    </WindowChrome.WindowChrome>
    <Grid Name="gdMain">
        <Button Click=Btn_normal_Click>
    </Grid>
</Window>
````
cs
````C#

private void Btn_normal_Click(object sender, RoutedEventArgs e)
        {

            Console.WriteLine($"width:{this.ActualHeight}");
            Console.WriteLine($"height:{this.ActualWidth}");
            
            if (this.WindowState == WindowState.Normal)
            {
                gdMain.MaxWidth = SystemParameters.WorkArea.Width;
                gdMain.MaxHeight = SystemParameters.WorkArea.Height;
                this.WindowState = WindowState.Maximized;
            }
            else if (this.WindowState == WindowState.Maximized)
            {
                this.WindowState = WindowState.Normal;
            }
        }
````

## 3. standard window
以下内容参考自微软[官方文档](https://docs.microsoft.com/en-us/dotnet/api/system.windows.shell.windowchrome?view=windowsdesktop-6.0)，进行了大概的翻译
> ![](https://docs.microsoft.com/en-us/dotnet/media/windowoverviewfigure1.png?view=windowsdesktop-6.0)  
> 标准窗口是由两个重叠的矩形组成。如上图  
>1. 外部的矩形为 "non-client area",表述为chrome，即我们上文修改的WindowChrome。它的绘制和托管是由system window manager。它的尺寸由标准操作系统设置决定。"non-client area"提供了标准的window功能和行为。比如包括了最大化、最小化、关闭等按钮，window边框，移动和修改行为、软件图标标题、系统目录。  
> 2. 内部的矩形为 "client area",它仅仅包含你的软件本身，它的绘制和托管由你的程序自身。

### 3.1 windowstyle.none
可以使用 WindowStyle="None" AllowsTransparency="True"去除外部矩形，然后在内部矩形内自定义标题栏等操作。  
windowstyle有4中模式
- None 仅在客户端区域可见-不显示标题栏和边框。
- SingleBorderWindow  具有单个边框的窗口。 这是默认值。
- ThreeDBorderWindow  一个带有窗口 三维 边框。
- ToolWindow 固定的工具窗口

### 3.2 windowchrome
推荐使用windowchrome进行自定义标题栏，即修改外部矩形的设置，这样可以拥有外部矩形的功能

## 参考链接
1. https://blog.csdn.net/WPwalter/article/details/81121829
2. https://docs.microsoft.com/en-us/dotnet/api/system.windows.shell.windowchrome?view=windowsdesktop-6.0
3. https://www.cnblogs.com/dino623/p/custom_window_style_using_WindowChrome.html