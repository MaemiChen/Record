## 1. 简介
wpf自行渲染所有用户界面元素，当使用不同分辨率的显示器时，仅是使用不同像素点去渲染该控件

## 2. wpf的单位
WPF程序中的单位是与设备无关的单位，每个单位是1/96英寸，如果电脑的DPI设置为96（每个英寸96个像素），那么此时每个WPF单位对应一个像素，不过如果电脑的DPI设备为120（每个英寸120个像素），那此时每个WPF单位对应应该是120/96=1.25个像素   
也就是说wpf中的控件在不同分辨率的情况下会自动调整，使用不同的像素点渲染，以适应屏幕显示。  


一般在程序中我们常常需要得到当前屏幕的宽和高，常见做法有：

1. System.Windows.Forms.Screen.PrimaryScreen.Bounds.Width

   System.Windows.Forms.Screen.PrimaryScreen.Bounds.Height

这两个方法可以返回当前屏幕选择的分辨率，该分辨率是以像素为单位，在DPI为96的情况下我们可以利用它们来做一些控件的定位，因为此时WPF单位对应一个像素，而当DPI非96的情况下，用该分辨率来做定位就会发现误差了，因此此时每个WPF单位并不是对应于一个像素

2. SystemParameters.PrimaryScreenWidth

   SystemParameters.PrimaryScreenHeight

这两个方法可以返回当前屏幕的宽和高,它是与设备无关的单位（1/96英寸），因此用它来做控件的定位，在DPI改变的情况下，也不会发生定位上的误差

3. SystemParameters.WorkArea.Size.Width

   SystemParameters.WorkArea.Size.Height

这两个方法可以返回当前屏幕工作区的宽和高（除去任务栏）,它也是与设备无关的单位，通常我们可以结合2和3来得到任务栏的高度

## 3. 参考
1. wpf编程宝典1.3
2. https://www.jianshu.com/p/5d56826d36e0