## 转载
https://blog.walterlv.com/post/wpf-x-reference.html
https://blog.walterlv.com/post/fix-wpf-binding-issues-in-context-menu

````C#
<TextBlock Text="444" Visibility="{Binding Source={x:Reference Name=btn}, Path=Visibility}"/>

<TextBlock Text="555" Visibility="{Binding ElementName=btn, Path=Visibility}"/>

<Button Click="Button_Click" Name="btn"/>
````

以上内容为444和555的功能都是一致的，就是当他们的visible绑定btn。  
区别 
- 在上下文中拥有不同边界命名时，ElementName无法找到绑定源
- ElementName是在运行时通过查找可视化树或逻辑树来确定边界的（NameScope），所以一定程度上性能不那么好。