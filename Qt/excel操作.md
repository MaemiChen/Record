1. 插入图片用法
````C++
//方法1
//第一个参数为列偏移数，第二个参数为行偏移数
double width = wf.width() * 0.75;   //图片相对左上角偏移的宽度
double height = wf.height() * 0.75; //图片相对左上角偏移的高度
shapes->dynamicCall("AddPicture( QString&, bool, bool, double, double, double, double",QDir::toNativeSeparators(filePath_img),true,true,0,120,width,height);


//方法2(尚未验证通过)
QImage image("c:/Users/jhlee/Dropbox/orgwiki/img/class01.png");
	QClipboard* clip = QApplication::clipboard();
	clip->setImage(image);
	QAxObjectPtr shapes(sheet->querySubObject("Shapes"));
	sheet->dynamicCall("Paste()");
	int shapeCount = shapes->property("Count").toInt();
	QAxObjectPtr shape(shapes->querySubObject(QString::fromLatin1("Item(%1)").arg(shapeCount).toAscii()));
````

## 参考文件

[Excel参考，快读，图片]https://blog.csdn.net/amars_ding/article/details/54580506
https://github.com/joonhwan/study/blob/master/qt/QtMoreActiveX/main.cpp