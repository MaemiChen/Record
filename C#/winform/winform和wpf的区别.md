## 说明
1. wpf的`线程模型`和winform的线程模型不同。
2. wpf的数据绑定用得比较多，而winform在列表中才用
3. wpf支持3D，winform本身没有，需要引入托管的DXWPF现成的动画机制。
4. winform没有wpf界面可以用xaml写，后台用C#，winform全部C#。WPF真正做到界面和逻辑分离。
5. wpf调用direct直接用显卡绘图，支持3D，性能高。winform调用GDI+绘图，需要手工GDI重绘。但是由于wpf比winform多了一层封装，所以实现简单界面性能不如winform。

## winform
1. winform市面上常有的UI统一处理控件：DevExpress DotenetBar
2. 界面和数据完全耦合。取数据在绑定之前已经知道数据的结构，在获取的数据的时候还要通过控件找数据。
3. winform调用GDI+绘图。

## wpf
wpf在支持winform旧模式的操作下，升级了新的模式，即MVVM（数据和视图分离），不再为每个元素添加固定名称，然后通过后台进行事件进行业务代码的编写。  
1. wpf是基于DirectX引擎的，支持GPU硬件加速，在不支持硬件加速的时候也可以使用软件绘制。

https://blog.csdn.net/fucong920618717/article/details/70212321