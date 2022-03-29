## 1. 简述
提供一个滚动视图到widget，如果超过画面大小，则提供滚动条

## 2. 函数
1. setwidget()  //传入一个widget到滚动视图中
2. alignment和setAlignment确认widget的排列方式
3. ensureVisible(int x,int y,int xMargin = 50, int yMargin = 50);确认在位置xy处显示，像素点为50 50；ensureWidgetVisible同理
4. takeWidget移除当前的widget，widget获取当前的widget