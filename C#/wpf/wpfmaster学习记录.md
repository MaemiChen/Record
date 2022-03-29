## 说明
github地址：https://github.com/microsoft/WPF-Samples  

## Data Binding

### 1. BindingToMethod
ObjectDataProvider可以让方法绑定到控件。方法的参数和方法的结果的绑定 建议参考blog https://www.cnblogs.com/T-ARF/p/12369534.html

### 2. BindingToStringFormat
参考blog https://www.cnblogs.com/xuliming/articles/StringFormat.html

### 3. BindingValidation
给Validation添加一个ToolTips错误提示
````C#
<Style.Triggers>
    <Trigger Property="Validation.HasError" Value="true">
        <Setter Property="ToolTip"
    Value="{Binding RelativeSource={x:Static RelativeSource.Self},
                    Path=(Validation.Errors)[0].ErrorContent}"/>
    </Trigger>
</Style.Triggers>
````

### 4. CodeOnlyBinding
1. BindingOperations这个静态类的GetBinding SetBinding ClearBinding等静态函数，其他函数的具体使用场景和路径？？TODO

### 5. CollectionBinding
IsSynchronizedWithCurrentItem="True" （每次选择会实时地改变SelectedItem属性，这个属性其实也是来自Selector类）会立即更新（同步）到对象集合(ListBox的Items属性)的当前项（Items对象的CurrentItem属性），反之亦成立。  
参考：https://blog.csdn.net/qq_16587307/article/details/79589882

### 6. CollectionViewSource
- CollectionView和CollectionViewSource
    - CollectionView   
    当后台数据列表绑定到一个列表控件时，wpf在数据列表和列表控件之间增加了层称为CollectionView（列表视图）的东西，支持很多高级操作，<font color=red>如排序，分组，过滤等</font>。这样可以将这个过程分成3个部分来看：数据列表（维持后台数据），列表视图（维持一些附加状态，如当前项/排序等），列表控件（负责对CollectionView的呈现，而不是Collection）  
    CollectionView是针对数据的，但它不会改变数据，仅对数据的“映像”进行“排列组合排序”等。一组数据可以有很多个CollectionView。针对后台给我们的一组数据可以同时为用户提供若干种展现方式。  
    举例ListBox可以针对数据，也可以针对CollectionView，Items种添加数据（针对数据本身），ItemSource指定数据来源（针对CollectionView，如果没有指定列表控件的CollectionView，wpf会自动插入一个）。不能同时使用Items和ItemSource会造成混乱。  

    - CollectionViewSource  
    CollectionViewSource是CollectionView的XAML代理。CollectionView不能再XAML中使用，可以使用CollectionViewSource，基本关系是“HAS A”

    - CollectionViewSource
    最主要的是可以对列表控件的显示进行排序（SortDescription）分组（GroupDescription）过滤（Filter）

    https://www.cnblogs.com/zhouyinhui/archive/2007/12/07/987076.html

### 7. CompositeCollection
<font color=red> 允许将多个集合和项作为一个列表显示。</font>

### 8. DataTemplatingIntro
https://www.cnblogs.com/qcst123/p/11918017.html  
- DataTemplateSelector   
    数据模板选择器主要运用于一些项容器中根据不同的数据类型选择不同的DataTemplate，以便展示不同的数据。  
    ItemContainerStyleSelector(同理于DataTemplateSelector)

### 9. EditingCollection
 注意使用 IEditableObject 能够提交或回滚作为数据源的对象所作的更改。  
 IEditableCollectionView Commit Cancel提交或取消挂起的新项。

### 10. PriorityBinding
PriorityBinding在wpf中通过绑定Collection来工作。Collection按照最高优先级到最低优先级的顺序进行排队。如果最高优先级绑定在处理时成功返回值，则永远不需要处理列表中的其他绑定。 这种情况可能是最高优先级的绑定需要很长的时间来计算，因此，在更高优先级的绑定成功返回值之前，将使用下一个最高优先级来成功返回值。

### 11. ValidateItemsInItemsControl
TODO 在test中再次进行验证学习

### 12. XmlnsBinding 
XmlNamespaceMapping 映射关系