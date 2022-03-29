## 1. 简述
![](https://img-blog.csdnimg.cn/20190409093530841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjEwMzAyNg==,size_16,color_FFFFFF,t_70)

## 2. 数组
````C#
// Array
String[] array = new String[3];
array[0] = "hello";
// ArrayList
ArrayList list = new ArrayList();
list.add("zhangsan");
list.add(11);
list.add(true);
// List<T>
List<Person> listP = new List<Person>();

````

## 3. LinkedList<T>
LinkedList<T> 是一个双向链表，集合类没有非泛型的版本

## HashTable Dictionary
HashTable是非泛型的，Dictionary是泛型的
### HashTable使用情况
1. 某些数据被高频率查询
2. 数据量大
3. 查询字段包含字符串类型
4. 数据类型不唯一

### 比较
1. Dictionary<K,V>是泛型的，当K或V是值类型时，其速度远远超过Hashtable
2. 单线程程序中推荐使用 Dictionary, 有泛型优势, 且读取速度较快, 容量利用更充分.
3. HashTable是单线程写入多线程读取安全的。对 Hashtable 进一步调用 Synchronized() 方法可以获得完全线程安全的类型. 而 Dictionary 非线程安全, 必须人为使用 lock 语句进行保护, 效率大减.
4. Dictionary 有按插入顺序排列数据的特性 (注: 但当调用 Remove() 删除过节点后顺序被打乱), 因此在需要体现顺序的情境中使用 Dictionary 能获得一定方便.

## 4. HashSet<T>
一组不包含重复元素的集合
````C#
HashSet<string> set = new HashSet<string>();
set.add("11");
set.add("22");
set.add("11");
````

## 5. Queue/Stack
Queue 具有泛型和非泛型，队列先入先出
Stack 具有泛型和非泛型，栈先入后出

## 6. SortedList、SortedList<TKey,TValue>
也是一个key value对，但是可以通过下标的方式访问，按照key来进行排序

## 参考链接  
1. https://blog.csdn.net/weixin_42103026/article/details/89080836
2. https://www.cnblogs.com/zk-zhou/p/6672494.html