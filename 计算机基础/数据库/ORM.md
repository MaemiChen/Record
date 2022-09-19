## ORM 简述
对象关系映射(Object Relational Mapping,简称ORM)模式是一种为了解决面向对象与关系数据库存在的互不匹配的现象的技术。简单的说，ORM是通过使用描述对象和数据库之前映射的元数据，`将程序中的对象自动持久化到关系数据库中`。  

关系型数据库有MySql，Oracle，Sql Server等，操作数据库大致分为以下两种方式
1. 直接使用数据库接口连接，每次操作都需要打开/关闭connection，频繁操作造成了浪费，不科学。
2. 使用ORM框架操作数据库，对象-关系设计（Object/Relation Mapping）是随着面向对象软件开发的发展而产生的。    

如何实现持久化？  
1. 硬编码方式，为每一种可能的数据库访问操作提供单独的方法。  
    1. 持久化层缺乏弹性，一旦出现业务需求的变更，必须修改持久化层的接口
    2. 持久化层同时与域模型与关系数据库模型绑定，不管域模型还是关系数据库模型发生改变，需要修改持久化层相关代码，增加软件维护难度。
2. ORM，它采用映射元数据来描述对象关系的映射，使得ORM中间件能在任何一个应用的业务逻辑层和数据库层之间充当桥梁。ORM方法论基于三个核心原则：
    1. 简单：以最基本的形式建模数据
    2. 传达性：数据库结构被任何人都能理解的语言文档化。
    3. 精确性：基于数据模型创建正确标准化了的结构。

## ORM技术特点
ORM解决的主要问题是对象关系的映射。域模型和关系模型分别是建立在概念模型的基础上的。域模型是面向对象的，而关系模型是面向关系的。`一般情况下，一个持久化类和一个表对应，类的每个实例对应表中的一条记录，类的每个属性对应表的每个字段`。

1. 提高了开发效率。由于ORM可以自动对Entity对象与数据库中的Table进行字段和属性的映射，所以我们不再需要一个专用的、庞大的数据访问层。
2. ORM提供了对数据库的映射，不用sql直接编码，能够像操作对象一样从数据库获取数据。

## ORM优缺点
ORM缺点：
1. 牺牲程序的执行效率和会固定思维模式。 
2. 从系统结构上看，采用ORM系统一般都是多层系统，系统的层次多了，效率就会降低。ORM是一种完全面向对象的做法，会对性能产生一定的影响。

## C#中使用ORM
orm类
```C#
 class ORM
{
    public static int SaveObject(Object model)
    {
        int n = -1;
        //获取实体对象的属性数组（包括属性名称、属性类型、以及属性对应的特性）
        PropertyInfo[] proArray = model.GetType().GetProperties();

        //获取标识列的名称（实际开发中，需要把非映射属性过滤。可以考虑获取数据库表结构）
        string[] unmappedAttr = new string[] { "Identify" };    
        string[] identityColumName = GetColumNameByAttribute(proArray,unmappedAttr);//需要过滤的特性名

        //根据属性和属性值组成sql语言
        StringBuilder sqlFiled = new StringBuilder(string.Format("insert into {0}(",model.GetType().Name)); //表名
        StringBuilder sqlValues = new StringBuilder("values (");

        //循环遍历所有反射到的属性名称和属性值
        foreach(PropertyInfo p in proArray)
        {
            //1. 过滤掉不需要的属性
            if (identityColumName.Contains(p.Name)) continue;
            //2. 过滤值为null的属性
            string columValue = p.GetValue(model, null) + "";
            if (columValue == null) continue;
            if (p.PropertyType.Name.Equals("DateTime"))
            {
                columValue = columValue.ToString(); //DateTime属性在数据库中用字符串类型表示
                columValue = string.Format("'{0}'", columValue);
            }
            if (p.PropertyType.Name.Equals("String"))
            {
                columValue = string.Format("'{0}'", columValue);
            }
            sqlFiled.Append(p.Name + ",");
            sqlValues.Append(columValue + ",");
        }
        //修改sql语句
        sqlFiled.Replace(',', ')', sqlFiled.Length - 1, 1);
        sqlValues.Replace(',', ')', sqlValues.Length - 1, 1);
        string sql = sqlFiled.ToString() + sqlValues.ToString();
        n = SqlHelper.ExecuteNonQuery(sql); //执行sql语句


        return n;
    }

    public static string[] GetColumNameByAttribute(PropertyInfo[] proArray,string[] attributeName)
    {
        List<string> listIllegalName = new List<string>();  //非映射字段
        foreach(PropertyInfo p in proArray)
        {
            object[] tableNameAttrs = p.GetCustomAttributes(false); //获取表名特性
            foreach(object obj in tableNameAttrs)
            {
                Attribute attr = (Attribute)obj;
                Type typeid = (Type)attr.TypeId;
                string typeName = typeid.Name;
                if (attributeName.Contains(typeName))
                {
                    listIllegalName.Add(p.Name);
                    break;
                }
            }
        }
        return listIllegalName.ToArray();
    }
    
}
```
sqlhelper类
```C#
public class SqlHelper
{
    private static string constr = System.Configuration.ConfigurationManager.ConnectionStrings["conStr"].ConnectionString;

    public static int ExecuteNonQuery(string sql)
    {
        int n = -1;
        using (SqlConnection con = new SqlConnection(constr))
        {
            using (SqlCommand cmd = new SqlCommand(sql, con))
            {
                con.Open();
                n = cmd.ExecuteNonQuery();
            }
        }
        return n;
    }
}
```

## C#中主流的ORM框架
https://www.cnblogs.com/jackytang/p/9111980.html

## 参考文件
1. https://blog.csdn.net/weixin_43670105/article/details/89070895
2. https://www.cnblogs.com/huanhang/p/6054908.html