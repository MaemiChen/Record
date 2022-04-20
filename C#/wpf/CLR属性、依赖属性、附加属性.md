## 依赖属性
```C#
public int TT
{
    get { return (int)GetValue(TTProperty); }
    set { SetValue(TTProperty, value); }
}

// Using a DependencyProperty as the backing store for TT.  This enables animation, styling, binding, etc...
public static readonly DependencyProperty TTProperty =
    DependencyProperty.Register("TT", typeof(int), typeof(MainWindow), new PropertyMetadata(0));
```
`依赖属性本质是一个单例模式，然后往DependencyProperty里面注册`  
### DependencyObject的存储
每个DependencyObject内部维护了一个`EffectiveValueEntry`的数组，这个EffectiveValueEntry是一个结构，封装了DependencyProperty的各个状态值animatedValue（作动画），baseValue（原始值），coercedValue（强制值），expressionValue（表达式值），当我们使用DependencyProperty.GetValue(TTProperty)时，先取到对应的EffectiveValueEntry，然后返回当前的Value。

为什么使用依赖属性：
- 内存使用量：  
    我们设计控件时会设计很多控件的属性，高度，宽度等，这样会有大量私有字段的存在，一个继承树下来会很膨胀。相当于只维护了一个有效的、设置过值得列表，减少内存消耗。
- 传统属性的局限性

https://www.cnblogs.com/surpasslight/archive/2012/02/10/2345157.html