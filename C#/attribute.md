## 1. attribute是什么
Attribute是一种可由用户自有定义的修饰符（Modifier），可以用来修饰各种需要被修饰的目标，修饰符（比如private、public、static、override、virtual等等）是C#语言本身的关键字。
简单地说，Attribute就是一种“附着物”——就像牡蛎吸附在船底或礁石上一样。
这些附着物的作用是为它们的附着体追加上一些额外的信息（这些信息保存在附着物的体内）——比如“这个类是我写的”或者“这个函数以前出过问题”等等。

## 2. 使用反射读取被特性标记的类、方法、属性
```C#
1 // 使用反射读取被特性标记的类、方法、属性
 2 using System;
 3 
 4 namespace Demo03_Attribute
 5 {
 6     class Program
 7     {
 8         static void Main(string[] args)
 9         {
10             Student student = new Student();
11             var type = student.GetType();
12             //看看类是否被特性标记
13             if(type.IsDefined(typeof(CustomOneAttribute),true))
14             {
15                 //拿去被标记的特性
16                 var attributeArr = type.GetCustomAttributes(typeof(CustomOneAttribute), true);
17                 foreach (var attr in attributeArr)
18                 {
19                     //attr就是每个标记的自定义特性CustomOneAttribute
20                     
21                 }
22 
23             }
24 
25             //遍历查找被自定义特性标记的方法
26             foreach (var method in type.GetMethods())
27             {
28                 if (method.IsDefined(typeof(CustomOneAttribute), true))
29                 {
30                     //method就是被自定义特性标记的
31                     var attributeArr = method.GetCustomAttributes(typeof(CustomOneAttribute), true);
32                     foreach (var attr in attributeArr)
33                     {
34                         //attr就是每个标记的自定义特性CustomOneAttribute
35 
36                     }
37                 }
38             }
39 
40             //遍历查找被自定义特性标记的属性
41             foreach (var prop in type.GetProperties())
42             {
43                 if (prop.IsDefined(typeof(CustomOneAttribute), true))
44                 {
45                     //prop就是被自定义特性标记的
46                     var attributeArr = prop.GetCustomAttributes(typeof(CustomOneAttribute), true);
47                     foreach (var attr in attributeArr)
48                     {
49                         //attr就是每个标记的自定义特性CustomOneAttribute
50 
51                     }
52                 }
53             }
54 
55             Console.ReadKey();
56         }
57     }
58 }

```

## 3. 自定义有参特性
```C#
//创建有参特性
    class CustomTwoAttribute:System.Attribute
    {
        public int Age;
        private string _name;
        public CustomTwoAttribute(String name)
        {
            _name = name;
        }
    }
    //标记使用特性
    public class Animal
    {
        [CustomTwo("chen",Age =20)]
        public void Jump() { }
    }
    //使用反射将特性属性读出来
    public class Use
    {
        static void UseAttr()
        {
            Animal animal = new Animal();
            var Type = animal.GetType();
            foreach(var method in Type.GetMethods())
            {
                if (method.IsDefined(typeof(CustomTwoAttribute), true))
                {
                    var attributeArr = method.GetCustomAttributes(typeof(CustomTwoAttribute), true);
                    foreach(var attr in attributeArr)
                    {
                        CustomTwoAttribute custom = (CustomTwoAttribute)attr;
                    }
                }
            }
        }
    }
```

## 参考链接
https://blog.csdn.net/xiaouncle/article/details/70216951