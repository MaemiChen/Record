https://doc.qt.io/qt-6/qbytearray.html#details

# QByteArray
1. 可以存储raw bytes和传统的8bit以'\0'结束的字符串。使用QByteArray比使用const char *更加方便。它始终确保数据以'\0'结束，同时使用implicit sharing  

at() for read-only access, can faster than operator \[](), 因为不会产生深拷贝

## 1. 初始化
````C++
QByteArray ba1("ca\0r\0t");
ba1.size();                     // Returns 2.
ba1.constData();                // Returns "ca" with terminating \0.

QByteArray ba2("ca\0r\0t", 3);
ba2.size();                     // Returns 3.
ba2.constData();                // Returns "ca\0" with terminating \0.

QByteArray ba3("ca\0r\0t", 4);
ba3.size();                     // Returns 4.
ba3.constData();                // Returns "ca\0r" with terminating \0.

const char cart[] = {'c', 'a', '\0', 'r', '\0', 't'};
QByteArray ba4(QByteArray::fromRawData(cart, 6));
ba4.size();                     // Returns 6.
ba4.constData();                // Returns "ca\0r\0t" without terminating \0.
````
如果想要计算'\0'，可以使用strlen  
只有在使用fromRowData导入到QByteArray时，才有可能不以'\0'为结尾，其他默认加上'\0'

## 2. 修改
可以使用以下函数修改QByteArray的值
1. append() 往后添加
2. prepend() 往前添加
3. insert() 插入
4. replace() 替换
5. remove() 删除
6. void chop(qsizetype n) 从末尾删除几个元素
7. QByteArray chopped(qsizetype len) 从末尾删除几个元素，并返回修改后的值
````C++
QByteArray x("and");
x.prepend("rock ");         // x == "rock and"
x.append(" roll");          // x == "rock and roll"
x.replace(5, 3, "&");       // x == "rock & roll"
````

## 3. 大小
1. reserve() 告诉编译系统需要使用的QByteArray的大小，可以预编译。
2. capacity() how much memory the QByteArray actually has allocated

## 4. member type documentation
enum QByteArray::Base64Option flags QByteArray::Base64Optioms  
这个enum包括Base64的编码解码操作
1. QByteArray::Base64Encoding 0 (default) 普通的64字节"base64"  
2. QByteArray::Base64UrlEncoding 1 另一个字符"base64url",代表两个类型对URL更加友好  
3. QByteArray::KeepTrailingEquals 0 (default)保持在编码后侧填充等号，让data的大小始终为4的倍数
4. QByteArray::OmitTrailingEquals 2 省去自动在尾部填充等号
5. QByteArray::IgnoreBase64DecodingErrors 0 当按照base-64编码时，忽略输入错误，无效字符被跳过。
6. QByteArray::AbortOnBase64DecodingErrors 4 当按照base-64编码时，在第一个编码错误处停下

## 5. fromCFData(CFDataRef data) fromNSData(const NSData *data)
创建一个QByteArray包含从CFData/NSData拷贝过来的数据

## 6. fromPercentEncoding(const QByteArray &input,char percent = '%')
````C++
QByteArray text = QByteArray::fromPercentEncoding("Qt%20is%20great%33");
text.data();            // returns "Qt is great!"
````

## 7. qsizetype indexOf(QByteArrayView bv, qsizetype from = 0)
查找QByteArrayView的下标的位置，返回第一个找到的下标值，如果没有则返回-1
````C++
QByteArray x("sticky question");
QByteArrayView y("sti");
x.indexOf(y);               // returns 0
x.indexOf(y, 1);            // returns 10
x.indexOf(y, 10);           // returns 10
x.indexOf(y, 11);           // returns -1
````
