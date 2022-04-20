## 
在代码中引用System.Windows.Interactivity.dll，引用这个dll就可以将方法映射到ViewModel层

```C#
xmlns:i="clr-namespace:System.Windows.Interactivity;assembly=System.Windows.Interactivity"
xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity" //或这个  

<TextBox Text="CommandBinding">
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="LostFocus">
            <i:InvokeCommandAction Command="{Binding OnTextLostFocus}"
                                   CommandParameter="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorLevel=1, AncestorType={x:Type TextBox}}}"/>
        </i:EventTrigger>
        <i:EventTrigger EventName="GotFocus">
            <i:InvokeCommandAction Command="{Binding OnTextGotFocus}"
                                   CommandParameter="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorLevel=1, AncestorType={x:Type TextBox}}}"/>
        </i:EventTrigger>
    </i:Interaction.Triggers>
</TextBox>
```

https://www.cnblogs.com/seekdream/p/14269488.html  
https://www.cnblogs.com/jiangyan219/articles/12245892.html （事件转命令）