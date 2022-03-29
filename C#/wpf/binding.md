
展示层和逻辑层的沟通Data Binding来实现

### 1. wpf 数据绑定 SelectedValue SelectedValuePath DisplayMemberPath的区别

    data这个数据源具有id和name两个属性 。
    SelectedValuePath DisplayMemberPath在这两个属性都不设置的情况下，无论显示还是传值，都是Data这个对象
1. SelectedValuePath = id 传值的时候只传id
2. DisplayMemberPath = name 选项里面只显示Name
3. selectValue  
     没有设置SelectedValuePath的话，SelectedItem和SelectedValue的值是一样的，都是选中的那个实体对象（例子中的Data），但如果设置了SelectedValuePath。SelectedItem是Data，但SelectedValuePath就是选中项的id了。

### 2. binding 依赖属性DataContent
1. 依赖属性的一个重要特点，当没有为一个控件的依赖属性显示赋值时，控件会把自己容器的属性值“借过来”当做自己的属性值，表现为属性值根据UI元素树传递

### 3. 使用ObjectDataProvider作为数据绑定
 1. 参考深入浅出wpf的p112

### 4. 使用binding的relativesource
1. 参考深入浅出wpf，从自己依次往外查找

### 5. binding的数据校验
使用validationRules进行数据校验

## 参考链接
https://blog.csdn.net/kenjianqi1647/article/details/90320569  
https://www.cnblogs.com/tqq-okc/p/10101946.html