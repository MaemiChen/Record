## 1. 简述
布局容器都是派生自system.windows.controls.panel 。

## 2. panel属性

### 2.1. panel类的公有属性
- background 如果想要接收鼠标事件，必须将该属性设置为非空值（可以将背景色设置为非空值）
- children 在面板中存储的条目集合（第一级条目）
- IsItemsHost (bool值)显示与itemscontrol相关联的项（如treeveiw控件中的节点等）该属性为true，如果需要自定义列表时，需要注意设置这个属性

如果需要创建自己的容器时，注意IsItemsHost和重写MeasureOverride和ArrangeOverride  

自动改变窗口大小 Windows下的SizeToContent

### 2.2 border
- background
- borderbrush
- borderThickness
- CornerRadius
- padding 在边框和内部内容之间添加控件（相对margin，在边框外部添加控件）

### 2.3 布局舍入、跨行列
当使用布局时面板切割时可以不完全对等，会出现模糊的情况，可以使用    
UseLayoutRounding="True" rowspan columnspan设置

### 2.4 共享尺寸组、层叠
1. 通过设置相同的SharedGroupSize的值表明这两个为共享尺寸，当一个尺寸改变时另一个也会随之改变
2. canvas.ZIndex设置大小可以确认层叠的方式，默认都为0，后来的覆盖前面的。

### 2.5 InkCanvas
定义了4个附加元素（left、top、right、bottom）与canvas使用类似，但是它支持手写功能