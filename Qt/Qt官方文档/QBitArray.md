https://doc.qt.io/qt-6/qbitarray.html#details

# QBitArray
bitArray 是一个数组，保存数据为bit位
````C++
QBitArray ba;
ba.resize(3);
//设置第0位为true
ba.setBit(0,true);
ba[0] = true;
````

## 1. empty null
由于历史原因，区分null和empty。
1. null
表示bitarray由QBitArray默认构造函数初始化
2. empty
表示size==0的bitarray

````C++
QBitArray().isNull();           // returns true
QBitArray().isEmpty();          // returns true

QBitArray(0).isNull();          // returns false
QBitArray(0).isEmpty();         // returns true

QBitArray(3).isNull();          // returns false
QBitArray(3).isEmpty();         // returns false
````

推荐使用isEmpty

## 2. 函数
1. bool QBitArray::at(qsizetype i) const
2. const char *QBitArray::bits() const
表示将QBitArray转换为char的指针
3. QBitArray QBitArray::fromBits(const char *data,qsizetype size)
表示将char类型指针指向的数据数据转换为bitarray格式
4. void QBitArray::clear()
清除数组，变为empty
5. void QBitArray::clearBit(qsizetype i)
将index为i的值设置为0
6. bool QBitArray::fill(bool value, qsizetype)  
bool QBitArray::fill(bool value, qsizetype begin, qsizetype end)
````C++
// 如果传入size，则数组resize到希望的大小
QBitArray ba(8);
ba.fill(true);
// ba: [ 1, 1, 1, 1, 1, 1, 1, 1 ]

ba.fill(false, 2);
// ba: [ 0, 0 ]

//部分填充
QBitArray ba(4);
ba.fill(true, 1, 2);            // ba: [ 0, 1, 0, 0 ]
ba.fill(true, 1, 3);            // ba: [ 0, 1, 1, 0 ]
ba.fill(true, 1, 4);            // ba: [ 0, 1, 1, 1 ]
````
7. void QBitArray::swap(QBitArray &other)
交换两个数组，速度很快且不会失败
8. bool QBitArray::testBit(qsizetype i) const
9. quint32 QBitArray::toUint32(QSysInfo::Endian endianness, bool *ok = nullptr)
将数组里面的参数转换为int类型，在Qt6时更新
10. bool QBitArray::toggleBit(qsizetype i)
invert value
11. void QBitArray::truncate(qsizetype pos)截断数组