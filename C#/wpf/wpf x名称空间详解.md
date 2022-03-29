## x名称空间
x名称空间映射的是http://schemas.microsoft.com/winfx/2006/xaml。包含的类与解析XAML语言相关，所以亦称为“XAML名称空间”

XMAL语言被解析并编译，最后形成微软中间语言保存在程序集中。

- x:Class Attribute  
    告诉编译器将xaml编译结果和后台编译结果的哪个类进行合并
    - 此attribute只能用于根节点
    - 使用x:Class的根节点类型与x:Class的值类型一致，且必须使用partial关键字
- x:ClassModiffer Attribute  
    告诉编译器编译的类的访问级别
- x:Name Attribute  
    - 告诉编译器除了生成一个实例外还要生命一个引用变量，变量名就是x:Name的值
    - 将Name属性（如果有）也设置为x:Name，并把这个值注册到UI树中，方便查找。
- x:FieldModifier Attribute  
    标签对应的实例的访问级别
- x:Key Attribute  
- x:Shared Attribute
    如果把某个对象作为资源放入资源字典里后我们就可以把它们检索起来重复使用。那么每当我们检索到一个对象，我们得到的究竟是同一个对象呢，还是这个对像的一个副本呢？这就要看我们为x:Shared赋什么值了。x:Shared一定要与x:Key配合使用，如果x:Shared值为true,那么每次检索这个对象的时候，我们得到的都是同一个对象，反之，我们得到的就是这个对象的一个副本。默认这个值是true。也就是说我们使用的都是同一个对象。

标记扩展实际就是一些MarkupExtension类的直接或间接派生类。x名称空间包含一些这样的类，所以称为x名称空间的标记扩展。


- x:Array 标签扩展
- x:Type 标签扩展
- x:Null 标签扩展
- x:Static 标签扩展

- x:Code 指令元素
- x:XData 指令元素

## 参考
1. https://wiggins.blog.csdn.net/article/details/8098742?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&utm_relevant_index=1（转载）