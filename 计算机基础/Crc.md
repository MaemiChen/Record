# 循环冗余检测（CRC）算法原理
Cyclic Redundancy check循环冗余检测，基于数据计算一组校验码，用于核对传输过程中是否被更改或传输错误。
## 奇偶校验
假设传递字符的ASCII码为0100 1101，则末尾加0或1，使数据的1的数量凑齐奇数或偶数  
出错概率为50%

## 累加和校验
传递LOVE ，得到四个数字76（M）79（O）86（V）69（E），可得为310，同时限定在0-255范围。校验值为310-255=55  
出错概率为1/256

## Crc校验
需要传递的字符87（W），约定除数6  
将一切加减运算改为异或（XOR）  
![模二除法](https://image-static.segmentfault.com/161/236/1612363195-5c55948246e45_articlex)
