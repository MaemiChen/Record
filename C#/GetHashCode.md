## 应用  
GetHashCode函数一般是在操作HashTable或者Dictionary之类的数据集的时候被调用，目的是产生一个key，方便在HashTable或者Dictionary中检索，每个类型都提供这个基本函数。  

GetHashCode函数中需要提供一个比较好的哈希函数，在最小的范围内实现数据分散，它的离散度决定HashTable的存取效率。