## 1. 简介
QSlider提供一个垂直或水平滚动条、允许用户沿水平或垂直方向移动滑块、并设置当前为宗旨为一个合法范围的值

## 2. 函数
1. setValue 设置滑块的当前值，
2. triggerAction 模拟点击效果 
3. setSingleStep、setPageStep设置步长
4. setMinimum、setMaximunm定义滚动条的范围

5. setTickPosition 刻度标记的位置
6. setTickInterval指定刻度的间隔
7. 当前刻度位置和间隔tickPosition、tickInterval

## 3. 信号
1. valueChanged 滑块的值发生改变
2. sliderPressed 按下滑块，发送信号
3. sliderMoved 拖动滑块，发送此信号
4. sliderRelease 释放滑块，发送此信号