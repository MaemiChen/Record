
## 转载翻译
https://stackoverflow.com/questions/60754206/difference-between-xstatic-and-staticresource-in-xaml-wpf

## 说明
在wpf xaml中使用{x:static}和{StaticResource}的区别是什么
如下
````C#
<StackPanel IsEnabled="{Binding Model.IsReadOnly, Converter={StaticResource BoolInverseConverter}}">
<StackPanel IsEnabled="{Binding Model.IsReadOnly, Converter={x:Static BoolInverseConverter}}">
````

这两个都是标记扩展。  
x:static表示对一个static特性的引用，在运行期间都不会发生改变。多用于枚举、常量、static属性字段等。这意味着第二行代码错误因为x:static只能引用一个属性而不能引用一个类。  
将BoolInverseConverter设置为Converter类中的一个静态属性，那么正确的代码如下
````C#
<StackPanel IsEnabled="{Binding Model.IsReadOnly, Converter={x:Static Converters.BoolInverseConverter}}">
````
> StaticResource 不意味着Resource在可访问方面是静态的，而是在资源查找方面是静态的。StaticResource指示XAML解析器查找资源树用来找到预定义的实例。  

> 同理DynamicResource，区别点在于StaticResource在编译时查找解析资源。而DynamicResource让XAML创建一个表达式，它将会在运行时执行。

下面的代码显示了Resource是怎样在XAML中引用的。一旦标记扩展StaticResource使用，它的查找就是静态的。这意味着一旦这个资源被找到，就不能替换为其他资源。同时，如果在编译时没有定义，软件会抛出异常“StaticResourceExtension”。如果key在运行时更改，引用将不会更新。
````C#
<StackPanel IsEnabled="{Binding Model.IsReadOnly, Converter={StaticResource BoolInverseConverter}}">
````

下一个代码同样是显示资源是怎么引用的，不过使用的是Dynamic。当它的key更改时，引用会更新
````C#
<StackPanel IsEnabled="{Binding Model.IsReadOnly, Converter={DynamicResource BoolInverseConverter}}">
````

后两个使用的分别是x:static和enum就不再赘述。