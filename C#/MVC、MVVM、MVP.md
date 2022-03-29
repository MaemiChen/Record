## 框架简述

- MVC（Model、View、Controller）
是一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方式组织代码，将业务逻辑聚集到一个部件中，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发转起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。

- MVVM

- MVP

## MVC
![](https://pic1.zhimg.com/80/v2-9fae41c5ee0f0e9d7f971a4cf56b4c5c_720w.jpg)
1. model   
应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据。
2. view  
应用程序中处理数据显示部分。通常view是依据model数据创建的。
3. controller  
应用程序中处理用户交互的部分，通常负责从视图中读取数据，控制用户输入，并向模型发送数据。  
controller本身不输出任何东西和任何处理，它只是接收请求并决定调用哪个model构件去处理请求，并决定使用哪个view来显示返回的数据。

## MVP
MVP将controller改为presenter

## MVVM
![](https://pic2.zhimg.com/80/v2-90372327ee9f61cc27df99ff28dd613d_720w.jpg)