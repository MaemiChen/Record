## 简述  

控件是数据表现形式和算法表现形式的双重载体  

- 控件的算法内容：指控件能够展示哪些数据、具有哪些方法、能响应哪些操作、激发什么事件，即控件功能，它是一组相关算法逻辑
- 控件的数据内容：控件展示的具体数据

在wpf中通过template将数据和算法解耦，wpf的template分为两大类

- controlTemplate算法内容的表现形式
- dataTemplate数据内容的表现形式

controltemplate决定它本身控件外观会长成什么样子，datatemplate决定数据外观

## 注意hierarchicalDataTemplate的作用目标