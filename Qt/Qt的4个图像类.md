## QPixmap  
QPixmap为绘图而生
## QImage
QImage为IO，图片像素访问及修改设计的。或者借助于QPainter操作像素
## QBitmap
一个继承于QPixmap的简单类,是QPixmap的子类，确保图片深度为1，提供单色图像，可以用来制作游标(QCursor)或者笔刷(QBrush)
## QPicture
QPicture绘画设备类，记录并重演了QPainter的命令。可以用QPainter的begin()在指定QPicture上绘画，用end()结束绘画
TODO

1. pixmap转bytearray，将界面图片传入数据库中，将图片转换为bytearray类型
https://www.cnblogs.com/that-boy-done/p/10777054.html