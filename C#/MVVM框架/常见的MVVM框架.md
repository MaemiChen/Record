## 说明
现有的MVVM框架都是基于ICommand，INotifyPropertyChanged的作用进行扩展，实现其通知、绑定、命令等功能。
| | | | |
| - | -| -|-| 
| 功能/框架名| Prism | MvvmLight | Microsoft.Toolkit.Mvvm |
| 通知 | BindingBase | ViewModelBase | ObservableObject |
| 命令 | DelegateCommand | RelayCommand | Async/RelayCommand |
| 聚合器 | IEventAggregator | IMessenger | IMessenger |
| 模块化 | 1 | 0 |0|
| 容器 | 1 | 0|0|
| 依赖注入 | 1|0 | 0|
| 导航 | 1 | 0| 0|
| 对话 | 1 | 0 | 0|
| | | |



https://www.cnblogs.com/zh7791/p/14140662.html