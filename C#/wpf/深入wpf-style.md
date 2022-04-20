#
> style用来在类型不同的实例之间共享属性、资源和事件处理程序。可以将style看作一组属性值应用到多个元素的捷径。  

好处：
- 把控件的通用设置抽出来变成style，使这些控件具有统一的风格，修改style中属性值，可以方便作用在所有应用该style的控件上。
- 可以对同一类型的控件定义多个style，通过替换style方便的修改控件样式。

## setter trigger
style使用`Setter`和`EventSetter`分别来设置控件的属性和事件处理。style不仅支持对属性的批处理，也共享资源和事件处理。  

## style & dependency property
dependency property是wpf的核心,style就是基于Dependency Property。`style的Setter作用在DP上`，如果在控件中定义了一个CLR属性，Style是不能设置的。Dependency Property设计精髓在于把字段和对象剥离，一个属性值内部用多个字段来存储，根据`取值优先级`决定当前属性应该取哪个字段。  

Dependency Property取值条件的优先级是（从上到下优先级从低到高）：
```C#
public enum BaseValueSource
{
     Unknown,
     Default,
     Inherited,
     DefaultStyle,
     DefaultStyleTrigger,
     Style,
     TemplateTrigger,
     StyleTrigger,
     ImplicitStyleReference,
     ParentTemplate,
     ParentTemplateTrigger,
     Local
 }
```

## style FrameworkElement
Style作为一个属性定义在FrameworkElement上，所有继承自FrameworkElement的控件都可以使用style。Framework定义了多个style：`Style` `ThemeStyle` `FocusVisualStyle`  

- FocusVisualStyle 当控件获得键盘焦点时，显示在外面的虚线框，这个style并没有直接作用在对应的FrameworkElement上，而是当控件获得键盘焦点时AdornLayer创建了一个新的Control，然后再在这个control中使用FocusVisualStyle，把它盖在FrameworkElement上形成一个虚框效果。
- Style 默认
- ThemeStyle 暂时没有找到  
    wpf默认提供了很多控件，Button Listbox等，当我们使用这些控件时，没有指定它的样式，wpf提供默认style。这个默认Style和Windows主题相关。`当我们切换Windows主题时，wpf窗口控件外观也会发生变化。`  
    这些默认的Style是以ResourceDictionary的形式保存在PresentationFramework.Aero.dll，PresentationFramework.Classic.dll等dll中的，这里的命名规则是：程序集名称+Theme名称+.dll。

## style ResourceDictionary
WPF 的Resource系统使用ResourceDictionary来储存Resource。ResourceDictionary按照键/值存储。在创建style时，指定了对应键值(x:Key)，然后使用StaticResource来引用这个键值。  
在ResourceDictionary不指定键(x:Key)是不能通过编译的。隐式(Implicit)Style会自动生成键值，这个键值不是String，而是一个类型的object。
```C#
<Style TargetType="{x:Type Button}" x:Key="{x:Type Button}">
```
`Resource 查找遵循最基本原则就是就近原则`

## Style Merge
Style相当域一个属性的批处理，对于一个属性，有多个style的情况。
- 1. 找到所有的Style
- 2. 确定Style的优先级，根据优先级来合并Style  

例子如下Button：  
1. 当前Windows的Theme是Aero，启动后会从PresentationFramework.Aero.dll找到对应的ThemeStyle。
2. 如果在Button上使用StaticResource或者DynamicResource指定了Style，会通过键值在Resource系统中找到对应的Style。
3. 如果没有在Button上显式指定Style，会通过Resource系统查找隐式Style（x:Type Button）。
4. 第二步和第三步是排他的，这两步只能确定一个Style，然后把这个Style和ThemeStyle进行合并（Merge）得到Button最终的效果。  

从合并来说，显式或隐式Style的优先级高于ThemeStyle，如果Style和ThemeStyle都对同一属性进行了预设，那么取Style中的Setter忽略ThemeStyle的Setter。如果是`EventSetter`，则两个style的EventSetter对同一个RoutedEvent进行了设置，都会注册到RoutedEvent中。  

前面看到，显式和隐式Style是排他的，两者只能取一，在实际项目中，在全局定义好Button的基本样式，然后具体使用上再根据基本样式做一些特殊处理，这种需求是很常见的。为了解决这种需求，Style提出了`BasedOn`属性，来表示`继承关系`  

WPF style机制是一个Seal机制，当合并完成后，Style就被密封，内部的Setter等不允许被修改。

- 基类控件的隐式Style不会作用到派生控件中。
- Style的BaseOn属性只支持StaticResource引用，因为Style继承自DispatchObject而不是DependencyObject，DynamicResource只支持DP。

https://www.cnblogs.com/Zhouyongh/archive/2011/08/01/2123610.html